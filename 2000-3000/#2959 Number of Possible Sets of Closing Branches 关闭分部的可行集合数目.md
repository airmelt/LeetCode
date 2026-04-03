# 2959 Number of Possible Sets of Closing Branches 关闭分部的可行集合数目

__Description:__

There is a company with `n` branches across the country, some of which are connected by roads. Initially, all branches are reachable from each other by traveling some roads.

The company has realized that they are spending an excessive amount of time traveling between their branches. As a result, they have decided to close down some of these branches (__possibly none__). However, they want to ensure that the remaining branches have a distance of at most `maxDistance` from each other.

The distance between two branches is the minimum total traveled length needed to reach one branch from another.

You are given integers `n`, `maxDistance`, and a __0-indexed__ 2D array `roads`, where `roads[i] = [ui, vi, wi]` represents the __undirected__ road between branches `ui` and `vi` with length `wi`.

Return _the number of possible sets of closing branches, so that any branch has a distance of at most_ `maxDistance` _from any other_.

Note that, after closing a branch, the company will no longer have access to any roads connected to it.

Note that, multiple roads are allowed.

__Example:__

Example 1:

![2959-1](https://assets.leetcode.com/uploads/2023/11/08/example11.png)

```text
Input: n = 3, maxDistance = 5, roads = [[0,1,2],[1,2,10],[0,2,10]]
Output: 5
Explanation: The possible sets of closing branches are:
```

- The set [2], after closing, active branches are [0,1] and they are reachable to each other within distance 2.
- The set [0,1], after closing, the active branch is [2].
- The set [1,2], after closing, the active branch is [0].
- The set [0,2], after closing, the active branch is [1].
- The set [0,1,2], after closing, there are no active branches.

It can be proven, that there are only 5 possible sets of closing branches.

Example 2:

![2959-2](https://assets.leetcode.com/uploads/2023/11/08/example22.png)

```text
Input: n = 3, maxDistance = 5, roads = [[0,1,20],[0,1,10],[1,2,2],[0,2,2]]
Output: 7
Explanation: The possible sets of closing branches are:
```

- The set [], after closing, active branches are [0,1,2] and they are reachable to each other within distance 4.
- The set [0], after closing, active branches are [1,2] and they are reachable to each other within distance 2.
- The set [1], after closing, active branches are [0,2] and they are reachable to each other within distance 2.
- The set [0,1], after closing, the active branch is [2].
- The set [1,2], after closing, the active branch is [0].
- The set [0,2], after closing, the active branch is [1].
- The set [0,1,2], after closing, there are no active branches.

It can be proven, that there are only 7 possible sets of closing branches.

Example 3:

```text
Input: n = 1, maxDistance = 10, roads = []
Output: 2
Explanation: The possible sets of closing branches are:
```

- The set [], after closing, the active branch is [0].
- The set [0], after closing, there are no active branches.

It can be proven, that there are only 2 possible sets of closing branches.

__Constraints:__

- `1 <= n <= 10`
- `1 <= maxDistance <= 10 ^ 5`
- `0 <= roads.length <= 1000`
- `roads[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `1 <= wi <= 1000`
- All branches are reachable from each other by traveling some roads.

__题目描述:__

一个公司在全国有 `n` 个分部，它们之间有的有道路连接。一开始，所有分部通过这些道路两两之间互相可以到达。

公司意识到在分部之间旅行花费了太多时间，所以它们决定关闭一些分部（ _也可能不关闭任何分部_ ），同时保证剩下的分部之间两两互相可以到达且最远距离不超过 `maxDistance` 。

两个分部之间的 距离 是通过道路长度之和的 最小值 。

给你整数 `n` ， `maxDistance` 和下标从 __0__ 开始的二维整数数组 `roads` ，其中 `roads[i] = [ui, vi, wi]` 表示一条从 `ui` 到 `vi` 长度为 `wi`的 __无向__ 道路。

请你返回关闭分部的可行方案数目，满足每个方案里剩余分部之间的最远距离不超过 `maxDistance`。

注意，关闭一个分部后，与之相连的所有道路不可通行。

注意，两个分部之间可能会有多条道路。

__示例:__

示例 1：

![2959-3](https://assets.leetcode.com/uploads/2023/11/08/example11.png)

```text
输入：n = 3, maxDistance = 5, roads = [[0,1,2],[1,2,10],[0,2,10]]
输出：5
解释：可行的关闭分部方案有：
```

- 关闭分部集合 [2] ，剩余分部为 [0,1] ，它们之间的距离为 2 。
- 关闭分部集合 [0,1] ，剩余分部为 [2] 。
- 关闭分部集合 [1,2] ，剩余分部为 [0] 。
- 关闭分部集合 [0,2] ，剩余分部为 [1] 。
- 关闭分部集合 [0,1,2] ，关闭后没有剩余分部。

总共有 5 种可行的关闭方案。

示例 2：

![2959-4](https://assets.leetcode.com/uploads/2023/11/08/example22.png)

```text
输入：n = 3, maxDistance = 5, roads = [[0,1,20],[0,1,10],[1,2,2],[0,2,2]]
输出：7
解释：可行的关闭分部方案有：
```

- 关闭分部集合 [] ，剩余分部为 [0,1,2] ，它们之间的最远距离为 4 。
- 关闭分部集合 [0] ，剩余分部为 [1,2] ，它们之间的距离为 2 。
- 关闭分部集合 [1] ，剩余分部为 [0,2] ，它们之间的距离为 2 。
- 关闭分部集合 [0,1] ，剩余分部为 [2] 。
- 关闭分部集合 [1,2] ，剩余分部为 [0] 。
- 关闭分部集合 [0,2] ，剩余分部为 [1] 。
- 关闭分部集合 [0,1,2] ，关闭后没有剩余分部。

总共有 7 种可行的关闭方案。

示例 3：

```text
输入：n = 1, maxDistance = 10, roads = []
输出：2
解释：可行的关闭分部方案有：
```

- 关闭分部集合 [] ，剩余分部为 [0] 。
- 关闭分部集合 [0] ，关闭后没有剩余分部。

总共有 2 种可行的关闭方案。

__提示：__

- `1 <= n <= 10`
- `1 <= maxDistance <= 10 ^ 5`
- `0 <= roads.length <= 1000`
- `roads[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `1 <= wi <= 1000`
- 一开始所有分部之间通过道路互相可以到达。

__思路:__

```text
由于 n 只有 10
可以用邻接矩阵将所有点的距离都记录下来
1. 状态压缩 ➕ 动态规划
遍历所有状态
如果当前状态包含 i 就把 i 对应的距离加入当前矩阵
遍历中间边 k 如果 k 属于状态 cur
则遍历边 i 如果 i 也属于状态 cur
就把所有边 j 都更新为 min(f[i][j], f[i][k] + f[k][j])
最后检查属于状态 cur 的边 i, j
如果 f[i][j] 比 maxDistance 大
说明不能加入结果集合
否则结果加 1
时间复杂度为 O(M + N ^ 3 * 2 ^ N), 空间复杂度为 O(N ^ 2), M 为边数组的长度
2. 状态压缩 ➕ Floyd
设 dp[cur][i][j] 表示从 i 到 j 的最短路并且所有 i 到 j 的中间节点都在 cur 这个集合中
比如 cur = 0b1101 表示集合中含有 0, 2, 3 三个节点
对于下一个节点 1
如果不加入节点 1 那么 dp[0b1111][i][j] = dp[0b1101][i][j]
如果加入节点 1 那么 dp[0b1111][i][j] = dp[0b1101][i][1] + dp[0b1101][1][j]
只需要求上述两种情况的较小值即可
令 dp[0] = graph
也就是不删除任何节点的时候的状态
result 从 1 开始
因为空集一定满足题意
时间复杂度为 O(M + N ^ 2 * 2 ^ N), 空间复杂度为 O(N ^ 2 * 2 ^ N), M 为边数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfSets(int n, int maxDistance, vector<vector<int>>& roads) 
    {
        int inf = 0x3f3f3f3f, total = 1 << n, result = 0;
        vector<vector<int>> graph(n, vector<int>(n, inf));
        for (const auto& road : roads) graph[road[0]][road[1]] = graph[road[1]][road[0]] = min(graph[road[0]][road[1]], road[2]);
        vector<vector<int>> f(n);
        auto check = [&](const auto& cur) -> bool
        {
            for (int i = 0; i < n; i++) if (cur >> i & 1) f[i] = graph[i];
            for (int k = 0; k < n; k++) if (cur >> k & 1) for (int i = 0; i < n; i++) if (cur >> i & 1) for (int j = 0; j < n; j++) f[i][j] = min(f[i][j], f[i][k] + f[k][j]);
            for (int i = 0; i < n; i++) if (cur >> i & 1) for (int j = 0; j < i; j++) if (cur >> j & 1 and f[i][j] > maxDistance) return false;
            return true;
        };
        for (int i = 0; i < total; i++) result += check(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSets(int n, int maxDistance, int[][] roads) {
        int result = 1, total = 1 << n, graph[][] = new int[n][n], inf = 0x3f3f3f3f, dp[][][] = new int[total][n][n];
        for (var row : graph) Arrays.fill(row, inf);
        for (var road : roads) graph[road[0]][road[1]] = graph[road[1]][road[0]] = Math.min(graph[road[0]][road[1]], road[2]);
        for (var matrix : dp) for (var row : matrix) Arrays.fill(row, inf);
        dp[0] = graph;
        for (int cur = 1, k = 0, t = 0, flag = 1; cur < total; cur++, k = Integer.numberOfTrailingZeros(cur), t = cur ^ (1 << k), flag = 1) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    dp[cur][i][j] = Math.min(dp[t][i][j], dp[t][i][k] + dp[t][k][j]);
                    if (flag == 1 && j < i && (cur >> i & 1) != 0 && (cur >> j & 1) != 0 && dp[cur][i][j] > maxDistance) flag = 0;
                }
            }
            result += flag;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSets(self, n: int, maxDistance: int, roads: List[List[int]]) -> int:
        graph, total, result, dp = [[inf] * n for _ in range(n)], 1 << n, 1, [[[inf] * n for _ in range(n)] for _ in range(1 << n)]
        dp[0] = graph
        for u, v, w in roads:
            graph[u][v] = graph[v][u] = min(graph[u][v], w)
        for cur in range(1, total):
            flag, t = True, cur ^ (1 << (k := cur.bit_length() - 1))
            for i in range(n):
                for j in range(n):
                    dp[cur][i][j] = min(dp[t][i][j], dp[t][i][k] + dp[t][k][j])
                    if flag and j < i and cur >> i & 1 and cur >> j & 1 and dp[cur][i][j] > maxDistance:
                        flag = False
            result += flag
        return result
```
