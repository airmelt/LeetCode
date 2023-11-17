# 1883 Minimum Skips to Arrive at Meeting On Time 准时抵达会议现场的最小跳过休息次数

__Description:__

You are given an integer `hoursBefore`, the number of hours you have to travel to your meeting. To arrive at your meeting, you have to travel through `n` roads. The road lengths are given as an integer array `dist` of length `n`, where `dist[i]` describes the length of the `i ^ th` road in __kilometers__. In addition, you are given an integer `speed`, which is the speed (in __km/h__) you will travel at.

After you travel road `i`, you must rest and wait for the __next integer hour__ before you can begin traveling on the next road. Note that you do not have to rest after traveling the last road because you are already at the meeting.

- For example, if traveling a road takes `1.4` hours, you must wait until the `2` hour mark before traveling the next road. If traveling a road takes exactly `2` hours, you do not need to wait.

However, you are allowed to skip some rests to be able to arrive on time, meaning you do not need to wait for the next integer hour. Note that this means you may finish traveling future roads at different hour marks.

- For example, suppose traveling the first road takes `1.4` hours and traveling the second road takes `0.6` hours. Skipping the rest after the first road will mean you finish traveling the second road right at the `2` hour mark, letting you start traveling the third road immediately.

Return _the __minimum number of skips required__ to arrive at the meeting on time, or_ `-1` _if it is __impossible___.

__Example:__

Example 1:

```text
Input: dist = [1,3,2], speed = 4, hoursBefore = 2
Output: 1
Explanation:
Without skipping any rests, you will arrive in (1/4 + 3/4) + (3/4 + 1/4) + (2/4) = 2.5 hours.
You can skip the first rest to arrive in ((1/4 + 0) + (3/4 + 0)) + (2/4) = 1.5 hours.
Note that the second rest is shortened because you finish traveling the second road at an integer hour due to skipping the first rest.
```

Example 2:

```text
Input: dist = [7,3,5,5], speed = 2, hoursBefore = 10
Output: 2
Explanation:
Without skipping any rests, you will arrive in (7/2 + 1/2) + (3/2 + 1/2) + (5/2 + 1/2) + (5/2) = 11.5 hours.
You can skip the first and third rest to arrive in ((7/2 + 0) + (3/2 + 0)) + ((5/2 + 0) + (5/2)) = 10 hours.
```

Example 3:

```text
Input: dist = [7,3,5,5], speed = 1, hoursBefore = 10
Output: -1
Explanation: It is impossible to arrive at the meeting on time even if you skip all the rests.
```

__Constraints:__

- `n == dist.length`
- `1 <= n <= 1000`
- `1 <= dist[i] <= 10 ^ 5`
- `1 <= speed <= 10 ^ 6`
- `1 <= hoursBefore <= 10 ^ 7`

__题目描述:__

给你一个整数 `hoursBefore` ，表示你要前往会议所剩下的可用小时数。要想成功抵达会议现场，你必须途经 `n` 条道路。道路的长度用一个长度为 `n` 的整数数组 `dist` 表示，其中 `dist[i]` 表示第 `i` 条道路的长度（单位:__千米__）。另给你一个整数 `speed` ，表示你在道路上前进的速度（单位:__千米每小时__）。

当你通过第 `i` 条路之后，就必须休息并等待，直到 __下一个整数小时__ 才能开始继续通过下一条道路。注意:你不需要在通过最后一条道路后休息，因为那时你已经抵达会议现场。

- 例如，如果你通过一条道路用去 `1.4` 小时，那你必须停下来等待，到 `2` 小时才可以继续通过下一条道路。如果通过一条道路恰好用去 `2` 小时，就无需等待，可以直接继续。

然而，为了能准时到达，你可以选择 跳过 一些路的休息时间，这意味着你不必等待下一个整数小时。注意，这意味着与不跳过任何休息时间相比，你可能在不同时刻到达接下来的道路。

- 例如，假设通过第 `1` 条道路用去 `1.4` 小时，且通过第 `2` 条道路用去 `0.6` 小时。跳过第 `1` 条道路的休息时间意味着你将会在恰好 `2` 小时完成通过第 `2` 条道路，且你能够立即开始通过第 `3` 条道路。

返回准时抵达会议现场所需要的 __最小跳过次数__ ，如果 __无法准时参会__ ，返回 `-1` 。

__示例:__

示例 1：

```text
输入：dist = [1,3,2], speed = 4, hoursBefore = 2
输出：1
解释：
不跳过任何休息时间，你将用 (1/4 + 3/4) + (3/4 + 1/4) + (2/4) = 2.5 小时才能抵达会议现场。
可以跳过第 1 次休息时间，共用 ((1/4 + 0) + (3/4 + 0)) + (2/4) = 1.5 小时抵达会议现场。
注意，第 2 次休息时间缩短为 0 ，由于跳过第 1 次休息时间，你是在整数小时处完成通过第 2 条道路。
```

示例 2：

```text
输入：dist = [7,3,5,5], speed = 2, hoursBefore = 10
输出：2
解释：
不跳过任何休息时间，你将用 (7/2 + 1/2) + (3/2 + 1/2) + (5/2 + 1/2) + (5/2) = 11.5 小时才能抵达会议现场。
可以跳过第 1 次和第 3 次休息时间，共用 ((7/2 + 0) + (3/2 + 0)) + ((5/2 + 0) + (5/2)) = 10 小时抵达会议现场。
```

示例 3：

```text
输入：dist = [7,3,5,5], speed = 1, hoursBefore = 10
输出：-1
解释：即使跳过所有的休息时间，也无法准时参加会议。
```

__提示：__

- `n == dist.length`
- `1 <= n <= 1000`
- `1 <= dist[i] <= 10 ^ 5`
- `1 <= speed <= 10 ^ 6`
- `1 <= hoursBefore <= 10 ^ 7`

__思路:__

```text
动态规划
设 dp[i][j] 表示通过前 i 条路跳过休息 j 次所用时间最小值
对于第 i 条路, 有两种选择:
    1. 不跳过休息, dp[i][j] = dp[i - 1][j] + dist[i - 1] / speed(向上取整)
    2. 跳过休息, dp[i][j] = dp[i - 1][j - 1] + dist[i - 1] / speed
两者取最小值, 即
dp[i][j] = min(dp[i - 1][j] + dist[i - 1] / speed, dp[i - 1][j - 1] + dist[i - 1] / speed)
初始化 dp[0][0] = 0, dp[0][j] = INT_MAX, j > 0
最终答案为 dp[n][j] <= hoursBefore * speed 的最小 j
注意 j = 0 时不能从 dp[i - 1][j - 1] 转移, 即不能跳过, j = i 时不能从 dp[i - 1][j] 转移, 即不能不跳过
又因为每次都向上取整可以记录路程 dp[i][j] = ((dp[i - 1][j] + dist[i - 1] - 1) / speed + 1) * speed
由于 dp[i][j] 只与 dp[i - 1][j] 和 dp[i - 1][j - 1] 有关, 可以使用滚动数组优化空间复杂度
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSkips(vector<int>& dist, int speed, int hoursBefore) 
    {
        int n = dist.size();
        vector<long long> dp(n + 1, INT_MAX);
        dp.front() = 0;
        for (int i = 1; i <= n; i++) 
        {
            vector<long long> cur(n + 1, INT_MAX);
            for (int j = 0; j <= n; j++) 
            {
                if (j != i) cur[j] = min(cur[j], ((dp[j] + dist[i - 1] - 1) / speed + 1) * speed);
                if (j != 0) cur[j] = min(cur[j], dp[j - 1] + dist[i - 1]);
            }
            dp = cur;
        }
        for (int j = 0; j <= n; j++) if (dp[j] <= 1L * hoursBefore * speed) return j;
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSkips(int[] dist, int speed, int hoursBefore) {
        int n = dist.length;
        long dp[] = new long[n + 1], INF = 0x3f3f3f3f;
        Arrays.fill(dp, INF);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            long[] cur = new long[n + 1];
            Arrays.fill(cur, INF);
            for (int j = 0; j <= n; j++) {
                if (j != i) cur[j] = Math.min(cur[j], ((dp[j] + dist[i - 1] - 1) / speed + 1) * speed);
                if (j != 0) cur[j] = Math.min(cur[j], dp[j - 1] + dist[i - 1]);
            }
            dp = cur;
        }
        for (int j = 0; j <= n; j++) if (dp[j] <= 1L * hoursBefore * speed) return j;
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minSkips(self, dist: List[int], speed: int, hoursBefore: int) -> int:
        n = len(dist)
        dp = [0] + [inf] * (n := len(dist))
        for i in range(1, n + 1):
            cur = [inf] * (n + 1)
            for j in range(i + 1):
                if j != i:
                    cur[j] = min(cur[j], ((dp[j] + dist[i - 1] - 1) // speed + 1) * speed)
                if j:
                    cur[j] = min(cur[j], dp[j - 1] + dist[i - 1])
            dp = cur
        for j in range(n + 1):
            if dp[j] <= hoursBefore * speed:
                return j
        return -1
```
