# 1928 Minimum Cost to Reach Destination in Time 规定时间内到达终点的最小花费

__Description:__

There is a country of `n` cities numbered from `0` to `n - 1` where __all the cities are connected__ by bi-directional roads. The roads are represented as a 2D integer array `edges` where `edges[i] = [xi, yi, timei]` denotes a road between cities `xi` and `yi` that takes `timei` minutes to travel. There may be multiple roads of differing travel times connecting the same two cities, but no road connects a city to itself.

Each time you pass through a city, you must pay a passing fee. This is represented as a __0-indexed__ integer array `passingFees` of length `n` where `passingFees[j]` is the amount of dollars you must pay when you pass through city `j`.

In the beginning, you are at city `0` and want to reach city `n - 1` in `maxTime` __minutes or less__. The __cost__ of your journey is the __summation of passing fees__ for each city that you passed through at some moment of your journey (__including__ the source and destination cities).

Given `maxTime`, `edges`, and `passingFees`, return _the __minimum cost__ to complete your journey, or_ `-1` _if you cannot complete it within_ `maxTime` _minutes_.

__Example:__

Example 1:

![1928-1](https://assets.leetcode.com/uploads/2021/06/04/leetgraph1-1.png)

```text
Input: maxTime = 30, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
Output: 11
Explanation: The path to take is 0 -> 1 -> 2 -> 5, which takes 30 minutes and has $11 worth of passing fees.
```

Example 2:

![1928-2](https://assets.leetcode.com/uploads/2021/06/04/copy-of-leetgraph1-1.png)

```text
Input: maxTime = 29, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
Output: 48
Explanation: The path to take is 0 -> 3 -> 4 -> 5, which takes 26 minutes and has $48 worth of passing fees.
You cannot take path 0 -> 1 -> 2 -> 5 since it would take too long.
```

Example 3:

```text
Input: maxTime = 25, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
Output: -1
Explanation: There is no way to reach city 5 from city 0 within 25 minutes.
```

__Constraints:__

- `1 <= maxTime <= 1000`
- `n == passingFees.length`
- `2 <= n <= 1000`
- `n - 1 <= edges.length <= 1000`
- `0 <= xi, yi <= n - 1`
- `1 <= timei <= 1000`
- `1 <= passingFees[j] <= 1000`
- The graph may contain multiple edges between two nodes.
- The graph does not contain self loops.

__题目描述:__

一个国家有 `n` 个城市，城市编号为 `0` 到 `n - 1` ，题目保证 __所有城市__ 都由双向道路 _连接在一起_ 。道路由二维整数数组 `edges` 表示，其中 `edges[i] = [xi, yi, timei]` 表示城市 `xi` 和 `yi` 之间有一条双向道路，耗费时间为 `timei` 分钟。两个城市之间可能会有多条耗费时间不同的道路，但是不会有道路两头连接着同一座城市。

每次经过一个城市时，你需要付通行费。通行费用一个长度为 `n` 且下标从 __0__ 开始的整数数组 `passingFees` 表示，其中 `passingFees[j]` 是你经过城市 `j` 需要支付的费用。

一开始，你在城市 `0` ，你想要在 `maxTime` __分钟以内__ （包含 `maxTime` 分钟）到达城市 `n - 1` 。旅行的 __费用__ 为你经过的所有城市 __通行费之和__ （__包括__ 起点和终点城市的通行费）。

给你 `maxTime`， `edges` 和 `passingFees` ，请你返回完成旅行的 __最小费用__ ，如果无法在 `maxTime` 分钟以内完成旅行，请你返回 `-1` 。

__示例:__

示例 1：

![1928-3](https://assets.leetcode.com/uploads/2021/06/04/leetgraph1-1.png)

```text
输入：maxTime = 30, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
输出：11
解释：最优路径为 0 -> 1 -> 2 -> 5 ，总共需要耗费 30 分钟，需要支付 11 的通行费。
```

示例 2：

![1928-4](https://assets.leetcode.com/uploads/2021/06/04/copy-of-leetgraph1-1.png)

```text
输入：maxTime = 29, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
输出：48
解释：最优路径为 0 -> 3 -> 4 -> 5 ，总共需要耗费 26 分钟，需要支付 48 的通行费。
你不能选择路径 0 -> 1 -> 2 -> 5 ，因为这条路径耗费的时间太长。
```

示例 3：

```text
输入：maxTime = 25, edges = [[0,1,10],[1,2,10],[2,5,10],[0,3,1],[3,4,10],[4,5,15]], passingFees = [5,1,2,20,20,3]
输出：-1
解释：无法在 25 分钟以内从城市 0 到达城市 5 。
```

__提示：__

- `1 <= maxTime <= 1000`
- `n == passingFees.length`
- `2 <= n <= 1000`
- `n - 1 <= edges.length <= 1000`
- `0 <= xi, yi <= n - 1`
- `1 <= timei <= 1000`
- `1 <= passingFees[j] <= 1000`
- 图中两个节点之间可能有多条路径。
- 图中不含有自环。

__思路:__

```text
动态规划
设 dp[t][i] 表示在恰好使用时间 t 到达城市 i 的最小花费
设最后一次从城市 u 到城市 i 的时间为 time, 则有 dp[t][i] = min(dp[t - time][u] + passingFees[i])
最终返回 min(dp[t][n - 1])，其中 1 <= t <= maxTime
初始状态为 dp[0][0] = passingFees[0] 表示在时间 0 到达城市 0 的最小花费为 passingFees[0]
时间复杂度为 O((N + M)K), 空间复杂度为 O(NK), 其中 N 为城市数量, M 为道路数量, K 为最大时间 maxTime
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minCost(int maxTime, vector<vector<int>>& edges, vector<int>& passingFees) 
    {
        int n = passingFees.size(), INF = 0x3f3f3f3f, result = INF;
        vector<vector<int>> dp(maxTime + 1, vector<int>(n, INF));
        dp.front().front() = passingFees.front();
        for (int t = 1; t <= maxTime; t++) 
        {
            for (const auto& e: edges) 
            {
                int u = e[0], v = e[1], time = e[2];
                if (time <= t) 
                {
                    dp[t][v] = min(dp[t][v], dp[t - time][u] + passingFees[v]);
                    dp[t][u] = min(dp[t][u], dp[t - time][v] + passingFees[u]);
                }
            }
        }
        for (int t = 1; t <= maxTime; i++) result = min(result, dp[t].back());
        return result == INF ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int maxTime, int[][] edges, int[] passingFees) {
        int n = passingFees.length, INF = 0x3f3f3f3f, dp[][] = new int[maxTime + 1][n], result = INF;
        for (int[] row : dp) Arrays.fill(row, INF);
        dp[0][0] = passingFees[0];
        for (int t = 1; t <= maxTime; t++) {
            for (int[] e: edges) {
                int u = e[0], v = e[1], time = e[2];
                if (time <= t) {
                    dp[t][v] = Math.min(dp[t][v], dp[t - time][u] + passingFees[v]);
                    dp[t][u] = Math.min(dp[t][u], dp[t - time][v] + passingFees[u]);
                }
            }
        }
        for (int t = 1; t <= maxTime; t++) result = Math.min(result, dp[t][n - 1]);
        return result == INF ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, maxTime: int, edges: List[List[int]], passingFees: List[int]) -> int:
        n = len(passingFees)
        dp = [[inf] * n for _ in range(maxTime + 1)]
        dp[0][0] = passingFees[0]
        for t in range(1, maxTime + 1):
            for u, v, time in edges:
                if time <= t:
                    dp[t][v], dp[t][u] = min(dp[t][v], dp[t - time][u] + passingFees[v]), min(dp[t][u], dp[t - time][v] + passingFees[u])
        return -1 if (result := min(dp[t][-1] for t in range(1, maxTime + 1))) == inf else result
```
