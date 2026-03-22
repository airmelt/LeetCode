# 1473 Paint House III 粉刷房子III

__Description:__

There is a row of `m` houses in a small city, each house must be painted with one of the `n` colors (labeled from `1` to `n`), some houses that have been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color.

- For example: `houses = [1,2,2,3,3,2,1,1]` contains `5` neighborhoods `[{1}, {2,2}, {3,3}, {2}, {1,1}]`.

Given an array `houses`, an `m x n` matrix `cost` and an integer `target` where:

- `houses[i]`: is the color of the house `i`, and `0` if the house is not painted yet.
- `cost[i][j]`: is the cost of paint the house `i` with the color `j + 1`.

Return _the minimum cost of painting all the remaining houses in such a way that there are exactly_ `target` _neighborhoods_. If it is not possible, return `-1`.

__Example:__

Example 1:

```text
Input: houses = [0,0,0,0,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
Output: 9
Explanation: Paint houses of this way [1,2,2,1,1]
This array contains target = 3 neighborhoods, [{1}, {2,2}, {1,1}].
Cost of paint all houses (1 + 1 + 1 + 1 + 5) = 9.
```

Example 2:

```text
Input: houses = [0,2,1,2,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
Output: 11
Explanation: Some houses are already painted, Paint the houses of this way [2,2,1,2,2]
This array contains target = 3 neighborhoods, [{2,2}, {1}, {2,2}]. 
Cost of paint the first and last house (10 + 1) = 11.
```

Example 3:

```text
Input: houses = [3,1,2,3], cost = [[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3
Output: -1
Explanation: Houses are already painted with a total of 4 neighborhoods [{3},{1},{2},{3}] different of target = 3.
```

__Constraints:__

- `m == houses.length == cost.length`
- `n == cost[i].length`
- `1 <= m <= 100`
- `1 <= n <= 20`
- `1 <= target <= m`
- `0 <= houses[i] <= n`
- `1 <= cost[i][j] <= 10 ^ 4`

__题目描述:__

在一个小城市里，有 `m` 个房子排成一排，你需要给每个房子涂上 `n` 种颜色之一（颜色编号为 `1` 到 `n` ）。有的房子去年夏天已经涂过颜色了，所以这些房子不可以被重新涂色。

我们将连续相同颜色尽可能多的房子称为一个街区。（比方说 `houses = [1,2,2,3,3,2,1,1]` ，它包含 5 个街区 `[{1}, {2,2}, {3,3}, {2}, {1,1}]` 。）

给你一个数组 `houses` ，一个 `m * n` 的矩阵 `cost` 和一个整数 `target` ，其中：

- `houses[i]`：是第 `i` 个房子的颜色，__0__ 表示这个房子还没有被涂色。
- `cost[i][j]`：是将第 `i` 个房子涂成颜色 `j+1` 的花费。

请你返回房子涂色方案的最小总花费，使得每个房子都被涂色后，恰好组成 `target` 个街区。如果没有可用的涂色方案，请返回 __-1__ 。

__示例:__

示例 1：

```text
输入：houses = [0,0,0,0,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
输出：9
解释：房子涂色方案为 [1,2,2,1,1]
此方案包含 target = 3 个街区，分别是 [{1}, {2,2}, {1,1}]。
涂色的总花费为 (1 + 1 + 1 + 1 + 5) = 9。
```

示例 2：

```text
输入：houses = [0,2,1,2,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
输出：11
解释：有的房子已经被涂色了，在此基础上涂色方案为 [2,2,1,2,2]
此方案包含 target = 3 个街区，分别是 [{2,2}, {1}, {2,2}]。
给第一个和最后一个房子涂色的花费为 (10 + 1) = 11。
```

示例 3：

```text
输入：houses = [0,0,0,0,0], cost = [[1,10],[10,1],[1,10],[10,1],[1,10]], m = 5, n = 2, target = 5
输出：5
```

示例 4：

```text
输入：houses = [3,1,2,3], cost = [[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3
输出：-1
解释：房子已经被涂色并组成了 4 个街区，分别是 [{3},{1},{2},{3}] ，无法形成 target = 3 个街区。
```

__提示：__

- `m == houses.length == cost.length`
- `n == cost[i].length`
- `1 <= m <= 100`
- `1 <= n <= 20`
- `1 <= target <= m`
- `0 <= houses[i] <= n`
- `1 <= cost[i][j] <= 10 ^ 4`

__思路:__

```text
1. 动态规划
设 dp[i][j][k] 表示粉刷前 i 个房子, 且第 i 个房子被粉刷成颜色 j, 它属于第 k 个街区的最小代价
不妨设第 i - 1 个房子的颜色为 pre

1.1 如果第 i 个房子已经被涂上颜色 cur

1.1.1 那么当涂色与原来的颜色不相同时 dp[i][j][k] = INF, j != cur, 可以取 INF = 0x3f3f3f3f 表示一个极大值

1.1.2 当涂色与原来的颜色相同时 dp[i][cur][k] 取决于前一个房子

1.1.2.1 当涂色与上一个房子颜色相同时, 两个房子属于同一个街区, dp[i][cur][k] = dp[i - 1][pre][k], 其中 cur == pre

1.1.2.2 当涂色与上一个房子颜色不相同时, 两个房子属于不同的街区, dp[i][cur][k] = min(dp[i - 1][pre][k - 1]), 其中 cur != pre

1.2 如果第 i 个房子没有涂色

1.2.1 当涂色与上一个房子颜色相同时, 两个房子属于同一个街区, dp[i][pre][k] = dp[i - 1][pre][k] + cost[i][j]

1.2.2 当涂色与上一个房子颜色不相同时, 两个房子属于不同的街区, dp[i][cur][k] = min(dp[i - 1][pre][k - 1]) + cost[i][j], 其中 cur != pre

初始化可以将所有值先设为 INF, i == 0 && j == 0 的区域可以初始化为 0, 该区间无法被取到, 或者将 houses[i] == j 的区域初始化为 0
每次需要遍历一次所有颜色来决定第 i 个房子的颜色可以进行优化
记录前一个房子的最小值, 最小值对应的颜色下标, 以及次小值 (first, first_idx, second)
如果 cur 和 first_idx 相等那么就选择 second
否则选择 first
这样就完成了从 dp[i - 1][j][k - 1] 向 dp[i][j][k] 的转移(这里隐含条件为 cur != j, 两个房子不属于同一个街区)
另外注意到第 i 个房子只取决于第 i - 1 个房子的颜色, 可以用滚动数组优化空间复杂度
时间复杂度为 O(KMN), 空间复杂度为 O(KN), 其中 K 为 target
2. 记忆化搜索
搜索空间与动态规划相同, 记录当前房子的下标, 颜色以及分成的区域数
可以利用区域数剪枝
分成两类情况讨论
房子已经被涂色, 只需要判断是否需要分区域
房子未被涂色, 取所有颜色的最小值
时间复杂度为 O(KMN ^ 2), 空间复杂度为 O(KMN), 其中 K 为 target
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    static constexpr int INF = 0x3f3f3f3f;
    using TIII = tuple<int, int, int>;
public:
    int minCost(vector<int>& houses, vector<vector<int>>& cost, int m, int n, int target) 
    {
        for (auto& house: houses) --house;
        vector<vector<int>> dp(n, vector<int>(target, INF));
        vector<vector<TIII>> best(m, vector<TIII>(target, {INF, -1, INF}));
        for (int i = 0; i < m; i++) 
        {
            vector<vector<int>> cur(n, vector<int>(target, INF));
            for (int j = 0; j < n; j++) 
            {
                if (houses[i] != -1 && houses[i] != j) continue;
                for (int k = 0; k < target; k++) 
                {
                    if (!i) 
                    {
                        if (!k) cur[j][k] = 0;
                    }
                    else 
                    {
                        cur[j][k] = dp[j][k];
                        if (k > 0) 
                        {
                            auto&& [first, first_idx, second] = best[i - 1][k - 1];
                            cur[j][k] = min(cur[j][k], (j == first_idx ? second : first));
                        }
                    }
                    if (cur[j][k] != INF and houses[i] == -1) cur[j][k] += cost[i][j];
                    auto&& [first, first_idx, second] = best[i][k];
                    if (cur[j][k] < first) 
                    {
                        second = first;
                        first = cur[j][k];
                        first_idx = j;
                    }
                    else if (cur[j][k] < second) second = cur[j][k];
                }
            }
            dp = cur;
        }
        int result = INF;
        for (int j = 0; j < n; j++) result = min(result, dp[j][target - 1]);
        return result < INF ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        int dp[][][] = new int[m + 1][target + 1][n + 1], INF = 0x3f3f3f3f, result = INF;
        for (int i = 0; i <= m; i++) for (int j = 0; j <= target; j++) for (int k = 0; k <= n; k++) if (i != 0 || j != 0) dp[i][j][k] = INF;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= Math.min(i, target); j++) {
                for (int k = 1; k <= n; k++) {
                    if (houses[i - 1] == 0 || houses[i - 1] == k) {
                        dp[i][j][k] = dp[i - 1][j][k] + (houses[i - 1] == 0 ? cost[i - 1][k - 1] : 0);
                        for (int l = 1, c = (houses[i - 1] == 0 ? cost[i - 1][k - 1] : 0); l <= n; l++) if (l != k) dp[i][j][k] = Math.min(dp[i][j][k], dp[i - 1][j - 1][l] + c);
                    }
                    if (i == m && j == target) result = Math.min(result, dp[i][j][k]);
                }
            }
        }
        return result < INF ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, houses: List[int], cost: List[List[int]], m: int, n: int, target: int) -> int:
        @lru_cache(None)
        def dfs(i: int, pre: int, blocks: int) -> int:
            if blocks > target or m - i + blocks < target:
                return float("inf")
            if i == m:
                return float("inf") if blocks < target else 0
            result = float("inf")
            if houses[i]:
                result = result if result < (cur := dfs(i + 1, houses[i], blocks + int(pre != houses[i]))) else cur
            else:
                for j in range(n):
                    result = result if result < (cur := cost[i][j] + dfs(i + 1, j + 1, blocks + int(pre != j + 1))) else cur
            return result
        return result if (result := dfs(0, -1, 0)) < float("inf") else -1
```
