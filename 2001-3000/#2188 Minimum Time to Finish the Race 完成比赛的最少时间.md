# 2188 Minimum Time to Finish the Race 完成比赛的最少时间

__Description:__

You are given a __0-indexed__ 2D integer array `tires` where `tires[i] = [fi, ri]` indicates that the `i ^ th` tire can finish its `x ^ th` successive lap in `fi * ri ^ (x-1)` seconds.

- For example, if `fi = 3` and `ri = 2`, then the tire would finish its `1 ^ st` lap in `3` seconds, its `2 ^ nd` lap in `3 * 2 = 6` seconds, its `3 ^ rd` lap in `3 * 2 ^ 2 = 12` seconds, etc.

You are also given an integer `changeTime` and an integer `numLaps`.

The race consists of `numLaps` laps and you may start the race with __any__ tire. You have an __unlimited__ supply of each tire and after every lap, you may __change__ to any given tire (including the current tire type) if you wait `changeTime` seconds.

Return the minimum time to finish the race.

__Example:__

Example 1:

```text
Input: tires = [[2,3],[3,4]], changeTime = 5, numLaps = 4
Output: 21
Explanation: 
Lap 1: Start with tire 0 and finish the lap in 2 seconds.
Lap 2: Continue with tire 0 and finish the lap in 2 * 3 = 6 seconds.
Lap 3: Change tires to a new tire 0 for 5 seconds and then finish the lap in another 2 seconds.
Lap 4: Continue with tire 0 and finish the lap in 2 * 3 = 6 seconds.
Total time = 2 + 6 + 5 + 2 + 6 = 21 seconds.
The minimum time to complete the race is 21 seconds.
```

Example 2:

```text
Input: tires = [[1,10],[2,2],[3,4]], changeTime = 6, numLaps = 5
Output: 25
Explanation: 
Lap 1: Start with tire 1 and finish the lap in 2 seconds.
Lap 2: Continue with tire 1 and finish the lap in 2 * 2 = 4 seconds.
Lap 3: Change tires to a new tire 1 for 6 seconds and then finish the lap in another 2 seconds.
Lap 4: Continue with tire 1 and finish the lap in 2 * 2 = 4 seconds.
Lap 5: Change tires to tire 0 for 6 seconds then finish the lap in another 1 second.
Total time = 2 + 4 + 6 + 2 + 4 + 6 + 1 = 25 seconds.
The minimum time to complete the race is 25 seconds.
```

__Constraints:__

- `1 <= tires.length <= 10 ^ 5`
- `tires[i].length == 2`
- `1 <= fi, changeTime <= 10 ^ 5`
- `2 <= ri <= 10 ^ 5`
- `1 <= numLaps <= 1000`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `tires` ，其中 `tires[i] = [fi, ri]` 表示第 `i` 种轮胎如果连续使用，第 `x` 圈需要耗时 `fi * ri ^ (x-1)` 秒。

- 比方说，如果 `fi = 3` 且 `ri = 2` ，且一直使用这种类型的同一条轮胎，那么该轮胎完成第 `1` 圈赛道耗时 `3` 秒，完成第 `2` 圈耗时 `3 * 2 = 6` 秒，完成第 `3` 圈耗时 `3 * 2 ^ 2 = 12` 秒，依次类推。

同时给你一个整数 `changeTime` 和一个整数 `numLaps` 。

比赛总共包含 `numLaps` 圈，你可以选择 __任意__ 一种轮胎开始比赛。每一种轮胎都有 __无数条__ 。每一圈后，你可以选择耗费 `changeTime` 秒 __换成__ 任意一种轮胎（也可以换成当前种类的新轮胎）。

请你返回完成比赛需要耗费的 最少 时间。

__示例:__

示例 1：

```text
输入：tires = [[2,3],[3,4]], changeTime = 5, numLaps = 4
输出：21
解释：
第 1 圈：使用轮胎 0 ，耗时 2 秒。
第 2 圈：继续使用轮胎 0 ，耗时 2 * 3 = 6 秒。
第 3 圈：耗费 5 秒换一条新的轮胎 0 ，然后耗时 2 秒完成这一圈。
第 4 圈：继续使用轮胎 0 ，耗时 2 * 3 = 6 秒。
总耗时 = 2 + 6 + 5 + 2 + 6 = 21 秒。
完成比赛的最少时间为 21 秒。
```

示例 2：

```text
输入：tires = [[1,10],[2,2],[3,4]], changeTime = 6, numLaps = 5
输出：25
解释：
第 1 圈：使用轮胎 1 ，耗时 2 秒。
第 2 圈：继续使用轮胎 1 ，耗时 2 * 2 = 4 秒。
第 3 圈：耗时 6 秒换一条新的轮胎 1 ，然后耗时 2 秒完成这一圈。
第 4 圈：继续使用轮胎 1 ，耗时 2 * 2 = 4 秒。
第 5 圈：耗时 6 秒换成轮胎 0 ，然后耗时 1 秒完成这一圈。
总耗时 = 2 + 4 + 6 + 2 + 4 + 6 + 1 = 25 秒。
完成比赛的最少时间为 25 秒。
```

__提示：__

- `1 <= tires.length <= 10 ^ 5`
- `tires[i].length == 2`
- `1 <= fi, changeTime <= 10 ^ 5`
- `2 <= ri <= 10 ^ 5`
- `1 <= numLaps <= 1000`

__思路:__

```text
动态规划
设 dp[i] 表示第 i 圈的最小耗时
假定全部用 1 种轮胎跑
那么 f * r ^ (x - 1) <= changeTime + f, 否则换轮胎更快
即 r ^ (x - 1) <= 1 + changeTime / f
取 f = 1, r = 2, 得到 x 的最大值为 17, 保险一点可以取 18
设 min_sec[x] 表示第 x 圈的最小耗时
那么 min_sec[x] = min(min_sec[x], sum(f * r ^ (x - 1)))
dp[i] = min(dp[i], dp[i - j] + min_sec[j]) + changeTime, 其中 1 <= j <= min(17, i)
初始化 dp[0] = -changeTime, 因为第 0 圈不需要换轮胎
最后返回 dp[numLaps]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumFinishTime(vector<vector<int>>& tires, int changeTime, int numLaps) 
    {
        vector<int> min_sec(18, 0x3f3f3f3f), dp(numLaps + 1, INT_MAX);
        dp.front() = -changeTime;
        for (const auto& tire : tires)
        {
            long f = tire.front();
            for (int x = 1, s = 0; f <= changeTime + tire.front(); x++)
            {
                s += f;
                min_sec[x] = min(min_sec[x], s);
                f *= tire.back();
            }
        }
        for (int i = 1; i <= numLaps; i++)
        {
            for (int j = 1, n = min(17, i); j <= n; j++) dp[i] = min(dp[i], dp[i - j] + min_sec[j]);
            dp[i] += changeTime;
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumFinishTime(int[][] tires, int changeTime, int numLaps) {
        int minSec[] = new int[18], dp[] = new int[numLaps + 1], inf = 0x3f3f3f3f;
        Arrays.fill(minSec, inf);
        Arrays.fill(dp, Integer.MAX_VALUE);
        for (int[] tire : tires) {
            long f = tire[0];
            for (int x = 1, s = 0; f <= changeTime + tire[0]; x++) {
                s += f;
                minSec[x] = Math.min(minSec[x], s);
                f *= tire[1];
            }
        }
        dp[0] = -changeTime;
        for (int i = 1; i <= numLaps; i++) {
            for (int j = 1, n = Math.min(17, i); j <= n; j++) dp[i] = Math.min(dp[i], dp[i - j] + minSec[j]);
            dp[i] += changeTime;
        }
        return dp[numLaps];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumFinishTime(self, tires: List[List[int]], changeTime: int, numLaps: int) -> int:
        min_sec, dp = [inf] * 18, [-changeTime] + [0] * numLaps
        for f, r in tires:
            x, time, s = 1, f, 0
            while time <= changeTime + f:
                s += time
                min_sec[x] = min(min_sec[x], s)
                time *= r
                x += 1
        for i in range(1, numLaps + 1):
            dp[i] = changeTime + min(dp[i - j] + min_sec[j] for j in range(1, min(18, i + 1)))
        return dp[-1]
```
