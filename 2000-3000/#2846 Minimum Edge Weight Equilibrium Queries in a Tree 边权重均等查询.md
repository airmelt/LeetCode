# 2846 Minimum Edge Weight Equilibrium Queries in a Tree 边权重均等查询

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi, wi]` indicates that there is an edge between nodes `ui` and `vi` with weight `wi` in the tree.

You are also given a 2D integer array `queries` of length `m`, where `queries[i] = [ai, bi]`. For each query, find the __minimum number of operations__ required to make the weight of every edge on the path from `ai` to `bi` equal. In one operation, you can choose any edge of the tree and change its weight to any value.

Note that:

- Queries are __independent__ of each other, meaning that the tree returns to its __initial state__ on each new query.
- The path from `ai` to `bi` is a sequence of __distinct__ nodes starting with node `ai` and ending with node `bi` such that every two adjacent nodes in the sequence share an edge in the tree.

Return _an array_ `answer` _of length_ `m` _where_ `answer[i]` _is the answer to the_ `i ^ th` _query._

__Example:__

Example 1:

![2846-1](https://assets.leetcode.com/uploads/2023/08/11/graph-6-1.png)

```text
Input: n = 7, edges = [[0,1,1],[1,2,1],[2,3,1],[3,4,2],[4,5,2],[5,6,2]], queries = [[0,3],[3,6],[2,6],[0,6]]
Output: [0,0,1,3]
Explanation: In the first query, all the edges in the path from 0 to 3 have a weight of 1. Hence, the answer is 0.
In the second query, all the edges in the path from 3 to 6 have a weight of 2. Hence, the answer is 0.
In the third query, we change the weight of edge [2,3] to 2. After this operation, all the edges in the path from 2 to 6 have a weight of 2. Hence, the answer is 1.
In the fourth query, we change the weights of edges [0,1], [1,2] and [2,3] to 2. After these operations, all the edges in the path from 0 to 6 have a weight of 2. Hence, the answer is 3.
For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from ai to bi.
```

Example 2:

![2846-2](https://assets.leetcode.com/uploads/2023/08/11/graph-9-1.png)

```text
Input: n = 8, edges = [[1,2,6],[1,3,4],[2,4,6],[2,5,3],[3,6,6],[3,0,8],[7,0,2]], queries = [[4,6],[0,4],[6,5],[7,4]]
Output: [1,2,2,3]
Explanation: In the first query, we change the weight of edge [1,3] to 6. After this operation, all the edges in the path from 4 to 6 have a weight of 6. Hence, the answer is 1.
In the second query, we change the weight of edges [0,3] and [3,1] to 6. After these operations, all the edges in the path from 0 to 4 have a weight of 6. Hence, the answer is 2.
In the third query, we change the weight of edges [1,3] and [5,2] to 6. After these operations, all the edges in the path from 6 to 5 have a weight of 6. Hence, the answer is 2.
In the fourth query, we change the weights of edges [0,7], [0,3] and [1,3] to 6. After these operations, all the edges in the path from 7 to 4 have a weight of 6. Hence, the answer is 3.
For each queries[i], it can be shown that answer[i] is the minimum number of operations needed to equalize all the edge weights in the path from ai to bi.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 3`
- `0 <= ui, vi < n`
- `1 <= wi <= 26`
- The input is generated such that `edges` represents a valid tree.
- `1 <= queries.length == m <= 2 * 10 ^ 4`
- `queries[i].length == 2`
- `0 <= ai, bi < n`

__题目描述:__

现有一棵由 `n` 个节点组成的无向树，节点按从 `0` 到 `n - 1` 编号。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ui, vi, wi]` 表示树中存在一条位于节点 `ui` 和节点 `vi` 之间、权重为 `wi` 的边。

另给你一个长度为 `m` 的二维整数数组 `queries` ，其中 `queries[i] = [ai, bi]` 。对于每条查询，请你找出使从 `ai` 到 `bi` 路径上每条边的权重相等所需的 __最小操作次数__ 。在一次操作中，你可以选择树上的任意一条边，并将其权重更改为任意值。

注意：

- 查询之间 __相互独立__ 的，这意味着每条新的查询时，树都会回到 __初始状态__ 。
- 从 `ai` 到 `bi`的路径是一个由 __不同__ 节点组成的序列，从节点 `ai` 开始，到节点 `bi` 结束，且序列中相邻的两个节点在树中共享一条边。

返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是第 `i` 条查询的答案。

__示例:__

示例 1：

![2846-3](https://assets.leetcode.com/uploads/2023/08/11/graph-6-1.png)

```text
输入：n = 7, edges = [[0,1,1],[1,2,1],[2,3,1],[3,4,2],[4,5,2],[5,6,2]], queries = [[0,3],[3,6],[2,6],[0,6]]
输出：[0,0,1,3]
解释：第 1 条查询，从节点 0 到节点 3 的路径中的所有边的权重都是 1 。因此，答案为 0 。
第 2 条查询，从节点 3 到节点 6 的路径中的所有边的权重都是 2 。因此，答案为 0 。
第 3 条查询，将边 [2,3] 的权重变更为 2 。在这次操作之后，从节点 2 到节点 6 的路径中的所有边的权重都是 2 。因此，答案为 1 。
第 4 条查询，将边 [0,1]、[1,2]、[2,3] 的权重变更为 2 。在这次操作之后，从节点 0 到节点 6 的路径中的所有边的权重都是 2 。因此，答案为 3 。
对于每条查询 queries[i] ，可以证明 answer[i] 是使从 ai 到 bi 的路径中的所有边的权重相等的最小操作次数。
```

示例 2：

![2846-4](https://assets.leetcode.com/uploads/2023/08/11/graph-9-1.png)

```text
输入：n = 8, edges = [[1,2,6],[1,3,4],[2,4,6],[2,5,3],[3,6,6],[3,0,8],[7,0,2]], queries = [[4,6],[0,4],[6,5],[7,4]]
输出：[1,2,2,3]
解释：第 1 条查询，将边 [1,3] 的权重变更为 6 。在这次操作之后，从节点 4 到节点 6 的路径中的所有边的权重都是 6 。因此，答案为 1 。
第 2 条查询，将边 [0,3]、[3,1] 的权重变更为 6 。在这次操作之后，从节点 0 到节点 4 的路径中的所有边的权重都是 6 。因此，答案为 2 。
第 3 条查询，将边 [1,3]、[5,2] 的权重变更为 6 。在这次操作之后，从节点 6 到节点 5 的路径中的所有边的权重都是 6 。因此，答案为 2 。
第 4 条查询，将边 [0,7]、[0,3]、[1,3] 的权重变更为 6 。在这次操作之后，从节点 7 到节点 4 的路径中的所有边的权重都是 6 。因此，答案为 3 。
对于每条查询 queries[i] ，可以证明 answer[i] 是使从 ai 到 bi 的路径中的所有边的权重相等的最小操作次数。
```

__提示：__

- `1 <= n <= 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 3`
- `0 <= ui, vi < n`
- `1 <= wi <= 26`
- 生成的输入满足 `edges` 表示一棵有效的树
- `1 <= queries.length == m <= 2 * 10 ^ 4`
- `queries[i].length == 2`
- `0 <= ai, bi < n`

__思路:__

```text
倍增 ➕ LCA
以 0 为根节点
记录每一个节点到其他节点的最大权出现次数以及深度
用倍增处理每一个节点并记录每个节点到倍增节点到权出现次数
在处理查询时
将深度大的节点放在后面
先将两个节点拉到同一深度
然后尝试往上跳到 LCA
找到 LCA 之后则查询结果为
depth[u] + depth[v] - (depth[lca] << 1) - max(cur)
cur 为当前路径上的边权计数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minOperationsQueries(int n, vector<vector<int>> &edges, vector<vector<int>> &queries) 
    {
        vector<vector<pair<int, int>>> graph(n);
        for (const auto &edge : edges) 
        {
            graph[edge[0]].emplace_back(edge[1], edge[2] - 1);
            graph[edge[1]].emplace_back(edge[0], edge[2] - 1);
        }
        int m = 32 - __builtin_clz(n);
        vector<vector<int>> pa(n, vector<int>(m, -1));
        vector<vector<array<int, 26>>> c(n, vector<array<int, 26>>(m));
        vector<int> depth(n);
        auto dfs = [&](this auto&& dfs, int u, int fa) -> void
        {
            pa[u][0] = fa;
            for (auto [v, w]: graph[u]) 
            {
                if (v != fa) 
                {
                    c[v][0][w] = 1;
                    depth[v] = depth[u] + 1;
                    dfs(v, u);
                }
            }
        };
        dfs(0, -1);
        for (int i = 0, p = 0; i < m - 1; i++) 
        {
            for (int u = 0; u < n; u++) 
            {
                if ((p = pa[u][i]) != -1) 
                {
                    pa[u][i + 1] = pa[p][i];
                    for (int j = 0; j < 26; j++) c[u][i + 1][j] = c[u][i][j] + c[p][i][j];
                }
            }
        }
        vector<int> result;
        for (const auto &q : queries) 
        {
            int u = q.front(), v = q.back(), l = depth[u] + depth[v], cur[26]{};
            if (depth[u] > depth[v]) swap(u, v);
            for (int k = depth[v] - depth[u]; k; k &= k - 1) 
            {
                int i = __builtin_ctz(k), p = pa[v][i];
                for (int j = 0; j < 26; j++) cur[j] += c[v][i][j];
                v = p;
            }
            if (u != v) 
            {
                for (int i = m - 1; i > -1; i--) 
                {
                    if (pa[u][i] != pa[v][i]) 
                    {
                        for (int j = 0; j < 26; j++) cur[j] += c[u][i][j] + c[v][i][j];
                        u = pa[u][i];
                        v = pa[v][i];
                    }
                }
                for (int j = 0; j < 26; j++) cur[j] += c[u][0][j] + c[v][0][j];
                u = pa[u][0];
            }
            result.emplace_back(l - (depth[u] << 1) - *max_element(cur, cur + 26));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    List<int[]>[] graph;

    public int[] minOperationsQueries(int n, int[][] edges, int[][] queries) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(new int[]{ edge[1], edge[2] - 1 });
            graph[edge[1]].add(new int[]{ edge[0], edge[2] - 1 });
        }
        int m = 32 - Integer.numberOfLeadingZeros(n), pa[][] = new int[n][m], c[][][] = new int[n][m][26], depth[] = new int[n], result[] = new int[queries.length];
        for (int i = 0; i < n; i++) Arrays.fill(pa[i], -1);
        dfs(0, -1, pa, c, depth);
        for (int i = 0, p; i < m - 1; i++) {
            for (int u = 0; u < n; u++) {
                if ((p = pa[u][i]) != -1) {
                    pa[u][i + 1] = pa[p][i];
                    for (int j = 0; j < 26; j++) c[u][i + 1][j] = c[u][i][j] + c[p][i][j];
                }
            }
        }
        for (int qi = 0; qi < queries.length; qi++) {
            int u = queries[qi][0], v = queries[qi][1], l = depth[u] + depth[v], cur[] = new int[26];
            if (depth[u] > depth[v]) {
                u ^= v;
                v ^= u;
                u ^= v;
            }
            for (int k = depth[v] - depth[u]; k > 0; k &= k - 1) {
                int i = Integer.numberOfTrailingZeros(k), p = pa[v][i];
                for (int j = 0; j < 26; j++) cur[j] += c[v][i][j];
                v = p;
            }
            if (u != v) {
                for (int i = m - 1; i > -1; i--) {
                    if (pa[u][i] != pa[v][i]) {
                        for (int j = 0; j < 26; j++) cur[j] += c[u][i][j] + c[v][i][j];
                        u = pa[u][i];
                        v = pa[v][i];
                    }
                }
                for (int j = 0; j < 26; j++) cur[j] += c[u][0][j] + c[v][0][j];
                u = pa[u][0];
            }
            result[qi] = l - (depth[u] << 1) - Arrays.stream(cur).max().getAsInt();
        }
        return result;
    }

    private void dfs(int u, int fa, int[][] pa, int[][][] c, int[] depth) {
        pa[u][0] = fa;
        for (var e : graph[u]) {
            int v = e[0], w = e[1];
            if (v != fa) {
                c[v][0][w] = 1;
                depth[v] = depth[u] + 1;
                dfs(v, u, pa, c, depth);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def minOperationsQueries(self, n: int, edges: List[List[int]], queries: List[List[int]]) -> List[int]:
        graph, m, result = defaultdict(list), n.bit_length(), []
        for u, v, w in edges:
            graph[u].append((v, w - 1))
            graph[v].append((u, w - 1))
        pa, c, depth = [[-1] * m for _ in range(n)], [[[0] * 26 for _ in range(m)] for _ in range(n)], [0] * n

        def dfs(u: int, fa: int) -> None:
            pa[u][0] = fa
            for v, w in graph[u]:
                if v != fa:
                    c[v][0][w], depth[v] = 1, depth[u] + 1
                    dfs(v, u)
        dfs(0, -1)
        for i in range(m - 1):
            for u in range(n):
                if pa[u][i] != -1:
                    pa[u][i + 1] = pa[pa[u][i]][i]
                    for j, (c1, c2) in enumerate(zip(c[u][i], c[pa[u][i]][i])):
                        c[u][i + 1][j] = c1 + c2
        for u, v in queries:
            if depth[u] > depth[v]:
                u, v = v, u
            k, l, cur = depth[v] - depth[u], depth[u] + depth[v], [0] * 26
            for i in range(k.bit_length()):
                if k & (1 << i):
                    for j, z in enumerate(c[v][i]):
                        cur[j] += z
                    v = pa[v][i]
            if u != v:
                for i in range(m - 1, -1, -1):
                    if pa[u][i] != pa[v][i]:
                        for j, (c1, c2) in enumerate(zip(c[u][i], c[v][i])):
                            cur[j] += c1 + c2
                        u, v = pa[u][i], pa[v][i]
                for j, (c1, c2) in enumerate(zip(c[u][0], c[v][0])):
                    cur[j] += c1 + c2
                u = pa[u][0]
            result.append(l - (depth[u] << 1) - max(cur))
        return result
```
