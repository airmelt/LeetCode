# 3108 Minimum Cost Walk in Weighted Graph 带权图里旅途的最小代价

__Description:__

There is an undirected weighted graph with `n` vertices labeled from `0` to `n - 1`.

You are given the integer `n` and an array `edges`, where `edges[i] = [ui, vi, wi]` indicates that there is an edge between vertices `ui` and `vi` with a weight of `wi`.

A walk on a graph is a sequence of vertices and edges. The walk starts and ends with a vertex, and each edge connects the vertex that comes before it and the vertex that comes after it. It's important to note that a walk may visit the same edge or vertex more than once.

The __cost__ of a walk starting at node `u` and ending at node `v` is defined as the bitwise `AND` of the weights of the edges traversed during the walk. In other words, if the sequence of edge weights encountered during the walk is `w0, w1, w2, ..., wk`, then the cost is calculated as `w0 & w1 & w2 & ... & wk`, where `&` denotes the bitwise `AND` operator.

You are also given a 2D array `query`, where `query[i] = [si, ti]`. For each query, you need to find the minimum cost of the walk starting at vertex `si` and ending at vertex `ti`. If there exists no such walk, the answer is `-1`.

Return _the array_ `answer`_, where_ `answer[i]` _denotes the __minimum__ cost of a walk for query_ `i`.

__Example:__

Example 1:

```text
Input: n = 5, edges = [[0,1,7],[1,3,7],[1,2,1]], query = [[0,3],[3,4]]

Output: [1,-1]

Explanation:
```

![3108-1](https://assets.leetcode.com/uploads/2024/01/31/q4_example1-1.png)

To achieve the cost of 1 in the first query, we need to move on the following edges: `0->1` (weight 7), `1->2` (weight 1), `2->1` (weight 1), `1->3` (weight 7).

In the second query, there is no walk between nodes 3 and 4, so the answer is -1.

Example 2:

```text
Input: n = 3, edges = [[0,2,7],[0,1,15],[1,2,6],[1,2,1]], query = [[1,2]]

Output: [0]

Explanation:
```

![3108-2](https://assets.leetcode.com/uploads/2024/01/31/q4_example2e.png)

To achieve the cost of 0 in the first query, we need to move on the following edges: `1->2` (weight 1), `2->1` (weight 6), `1->2` (weight 1).

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= wi <= 10 ^ 5`
- `1 <= query.length <= 10 ^ 5`
- `query[i].length == 2`
- `0 <= si, ti <= n - 1`
- `si != ti`

__题目描述:__

给你一个 `n` 个节点的带权无向图，节点编号为 `0` 到 `n - 1` 。

给你一个整数 `n` 和一个数组 `edges` ，其中 `edges[i] = [ui, vi, wi]` 表示节点 `ui` 和 `vi` 之间有一条权值为 `wi` 的无向边。

在图中，一趟旅途包含一系列节点和边。旅途开始和结束点都是图中的节点，且图中存在连接旅途中相邻节点的边。注意，一趟旅途可能访问同一条边或者同一个节点多次。

如果旅途开始于节点 `u` ，结束于节点 `v` ，我们定义这一趟旅途的 __代价__ 是经过的边权按位与 `AND` 的结果。换句话说，如果经过的边对应的边权为 `w0, w1, w2, ..., wk` ，那么代价为 `w0 & w1 & w2 & ... & wk` ，其中 `&` 表示按位与 `AND` 操作。

给你一个二维数组 `query` ，其中 `query[i] = [si, ti]` 。对于每一个查询，你需要找出从节点开始 `si` ，在节点 `ti` 处结束的旅途的最小代价。如果不存在这样的旅途，答案为 `-1` 。

返回数组 `answer` ，其中 `answer[i]` 表示对于查询 `i` 的 __最小__ 旅途代价。

__示例:__

示例 1：

```text
输入：n = 5, edges = [[0,1,7],[1,3,7],[1,2,1]], query = [[0,3],[3,4]]

输出：[1,-1]

解释：
```

![3108-3](https://assets.leetcode.com/uploads/2024/01/31/q4_example1-1.png)

第一个查询想要得到代价为 1 的旅途，我们依次访问: `0->1`（边权为 7 ） `1->2` （边权为 1 ） `2->1`（边权为 1 ） `1->3` （边权为 7 ）。

第二个查询中，无法从节点 3 到节点 4 ，所以答案为 -1 。

示例 2：

```text
输入：n = 3, edges = [[0,2,7],[0,1,15],[1,2,6],[1,2,1]], query = [[1,2]]

输出：[0]

解释：
```

![3108-4](https://assets.leetcode.com/uploads/2024/01/31/q4_example2e.png)

第一个查询想要得到代价为 0 的旅途，我们依次访问: `1->2`（边权为 1 ）， `2->1`（边权 为 6 ）， `1->2`（边权为 1 ）。

__提示：__

- `1 <= n <= 10 ^ 5`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= wi <= 10 ^ 5`
- `1 <= query.length <= 10 ^ 5`
- `query[i].length == 2`
- `0 <= si, ti <= n - 1`

__思路:__

```text
并查集
总可以把连在一起的节点走完所有路径而取到最小值
用并查集, 初始化节点为 -1
连接之后取与运算
最后如果连通, 返回父节点的值否则返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minimumCost(int n, vector<vector<int>>& edges, vector<vector<int>>& query) 
    {
        vector<int> val(n, -1), result(query.size(), -1), parent(n);
        iota(parent.begin(), parent.end(), 0);
        auto find = [&](this auto&& find, int p) -> int
        {
            return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
        };
        for (const auto& edge : edges) 
        {
            int u = find(edge[0]), v = find(edge[1]);
            val[v] &= edge[2];
            if (u != v) 
            {
                parent[u] = v;
                val[v] &= val[u];
            }
        }
        for (int i = 0, m = query.size(), u = 0; i < m; i++) if ((u = find(query[i][0])) == find(query[i][1])) result[i] = val[u];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;

    public int[] minimumCost(int n, int[][] edges, int[][] query) {
        int[] val = new int[n], parent = new int[n], result = new int[query.length];
        Arrays.fill(val, -1);
        Arrays.fill(result, -1);
        for (int i = 0; i < n; i++) parent[i] = i;
        for (var edge : edges) {
            int u = find(edge[0], parent), v = find(edge[1], parent);
            val[v] &= edge[2];
            if (u != v) {
                parent[u] = v;
                val[v] &= val[u];
            }
        }
        for (int i = 0, m = query.length, u = 0; i < m; i++) if ((u = find(query[i][0], parent)) == find(query[i][1], parent)) result[i] = val[u];
        return result;
    }

    private int find(int p, int[] parent) {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p], parent));
    }
}
```

__Python__:

```Python
class Solution:
    def minimumCost(self, n: int, edges: List[List[int]], query: List[List[int]]) -> List[int]:
        parent, val = list(range(n)), [-1] * n

        def find(p: int) -> int:
            if parent[p] != p:
                parent[p] = find(parent[p])
            return parent[p]
        for u, v, w in edges:
            u, v = find(u), find(v)
            val[v] &= w
            if u != v:
                val[v] &= val[u]
                parent[u] = v
        return [-1 if (p := find(u)) != find(v) else val[p] for u, v in query]
```
