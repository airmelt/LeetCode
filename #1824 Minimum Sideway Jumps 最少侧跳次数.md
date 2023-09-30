# 1824 Minimum Sideway Jumps 最少侧跳次数

__Description:__

There is a __3 lane road__ of length `n` that consists of `n + 1` __points__ labeled from `0` to `n`. A frog __starts__ at point `0` in the __second__ lane and wants to jump to point `n`. However, there could be obstacles along the way.

You are given an array `obstacles` of length `n + 1` where each `obstacles[i]` (__ranging from 0 to 3__) describes an obstacle on the lane `obstacles[i]` at point `i`. If `obstacles[i] == 0`, there are no obstacles at point `i`. There will be __at most one__ obstacle in the 3 lanes at each point.

- For example, if `obstacles[2] == 1`, then there is an obstacle on lane 1 at point 2.

The frog can only travel from point `i` to point `i + 1` on the same lane if there is not an obstacle on the lane at point `i + 1`. To avoid obstacles, the frog can also perform a __side jump__ to jump to __another__ lane (even if they are not adjacent) at the __same__ point if there is no obstacle on the new lane.

- For example, the frog can jump from lane 3 at point 3 to lane 1 at point 3.

Return _the __minimum number of side jumps__ the frog needs to reach __any lane__ at point n starting from lane `2` at point 0._

__Note:__ There will be no obstacles on points `0` and `n`.

__Example:__

Example 1:

![1824-1](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex1.png)

```text
Input: obstacles = [0,1,2,3,0]
Output: 2 
Explanation: The optimal solution is shown by the arrows above. There are 2 side jumps (red arrows).
Note that the frog can jump over obstacles only when making side jumps (as shown at point 2).
```

Example 2:

![1824-2](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex2.png)

```text
Input: obstacles = [0,1,1,3,3,0]
Output: 0
Explanation: There are no obstacles on lane 2. No side jumps are required.
```

Example 3:

![1824-3](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex3.png)

```text
Input: obstacles = [0,2,1,0,3,0]
Output: 2
Explanation: The optimal solution is shown by the arrows above. There are 2 side jumps.
```

__Constraints:__

- `obstacles.length == n + 1`
- `1 <= n <= 5 * 10 ^ 5`
- `0 <= obstacles[i] <= 3`
- `obstacles[0] == obstacles[n] == 0`

__题目描述:__

给你一个长度为 `n` 的 __3 跑道道路__ ，它总共包含 `n + 1` 个 __点__ ，编号为 `0` 到 `n` 。一只青蛙从 `0` 号点第二条跑道 __出发__ ，它想要跳到点 `n` 处。然而道路上可能有一些障碍。

给你一个长度为 `n + 1` 的数组 `obstacles` ，其中 `obstacles[i]` （ _取值范围从 0 到 3_ ）表示在点 `i` 处的 `obstacles[i]` 跑道上有一个障碍。如果 `obstacles[i] == 0` ，那么点 `i` 处没有障碍。任何一个点的三条跑道中 __最多有一个__ 障碍。

- 比方说，如果 `obstacles[2] == 1` ，那么说明在点 2 处跑道 1 有障碍。

这只青蛙从点 `i` 跳到点 `i + 1` 且跑道不变的前提是点 `i + 1` 的同一跑道上没有障碍。为了躲避障碍，这只青蛙也可以在 __同一个__ 点处 __侧跳__ 到 __另外一条__ 跑道（这两条跑道可以不相邻），但前提是跳过去的跑道该点处没有障碍。

- 比方说，这只青蛙可以从点 3 处的跑道 3 跳到点 3 处的跑道 1 。

这只青蛙从点 0 处跑道 `2` 出发，并想到达点 `n` 处的 __任一跑道__ ，请你返回 __最少侧跳次数__ 。

__注意__:点 `0` 处和点 `n` 处的任一跑道都不会有障碍。

__示例:__

示例 1：

![1824-4](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex1.png)

```text
输入：obstacles = [0,1,2,3,0]
输出：2 
解释：最优方案如上图箭头所示。总共有 2 次侧跳（红色箭头）。
注意，这只青蛙只有当侧跳时才可以跳过障碍（如上图点 2 处所示）。
```

示例 2：

![1824-5](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex2.png)

```text
输入：obstacles = [0,1,1,3,3,0]
输出：0
解释：跑道 2 没有任何障碍，所以不需要任何侧跳。
```

示例 3：

![1824-6](https://assets.leetcode.com/uploads/2021/03/25/ic234-q3-ex3.png)

```text
输入：obstacles = [0,2,1,0,3,0]
输出：2
解释：最优方案如上图所示。总共有 2 次侧跳。
```

__提示：__

- `obstacles.length == n + 1`
- `1 <= n <= 5 * 10 ^ 5`
- `0 <= obstacles[i] <= 3`
- `obstacles[0] == obstacles[n] == 0`

__思路:__

```text
动态规划
设 dp[i][j] 表示第 i 个位置青蛙在第 j 条跑道的最少侧跳次数
初始化 dp[0][1] = 0, dp[0][0] = dp[0][2] = 1
如果 obstacles[i] != 0, dp[i][obstacles[i] - 1] = inf
dp[i][j] = min(dp[i - 1][j], min(dp[i - 1]) + 1)
最后返回 min(dp[n - 1])
注意到每次更新 dp[i][j] 只需要用到 dp[i - 1][j], dp[i - 1][j - 1], dp[i - 1][j + 1]
所以可以用滚动数组优化空间复杂度
dp = [1, 0, 1]
dp[j] = min(dp[j], min(dp) + 1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minSideJumps(vector<int>& obstacles) 
    {
        int n = obstacles.size();
        vector<int> dp(3, 1);
        dp[1] = 0;
        for (int i = 1; i < n; i++) 
        {
            if (obstacles[i]) dp[obstacles[i] - 1] = INT_MAX;
            int cur = *min_element(dp.begin(), dp.end());
            for (int j = 0; j < 3; j++) if (j != obstacles[i] - 1) dp[j] = min(dp[j], cur + 1);
        }
        return *min_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minSideJumps(int[] obstacles) {
        int n = obstacles.length, dp[] = new int[3];
        dp[0] = dp[2] = 1;
        for (int i = 1; i < n; i++) {
            if (obstacles[i] != 0) dp[obstacles[i] - 1] = Integer.MAX_VALUE;
            int cur = Math.min(dp[0], Math.min(dp[1], dp[2]));
            for (int j = 0; j < 3; j++) if (j != obstacles[i] - 1) dp[j] = Math.min(dp[j], cur + 1);
        }
        return Math.min(dp[0], Math.min(dp[1], dp[2]));
    }
}
```

__Python__:

```Python
class Solution:
    def minSideJumps(self, obstacles: List[int]) -> int:
        dp = [1, 0, 1]
        for o in obstacles[1:]:
            if o:
                dp[o - 1] = inf
            cur = min(dp)
            for j in range(3):
                if j != o - 1:
                    dp[j] = min(dp[j], cur + 1)
        return min(dp)

```
