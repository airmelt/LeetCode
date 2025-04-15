# 2493 Divide Nodes Into the Maximum Number of Groups 将节点分成尽可能多的组

__Description:__

You are given a positive integer `n` representing the number of nodes in an __undirected__ graph. The nodes are labeled from `1` to `n`.

You are also given a 2D integer array `edges`, where `edges[i] = [ai, bi]` indicates that there is a __bidirectional__ edge between nodes `ai` and `bi`. __Notice__ that the given graph may be disconnected.

Divide the nodes of the graph into `m` groups (__1-indexed__) such that:

- Each node in the graph belongs to exactly one group.
- For every pair of nodes in the graph that are connected by an edge `[ai, bi]`, if `ai` belongs to the group with index `x`, and `bi` belongs to the group with index `y`, then `|y - x| = 1`.

Return _the maximum number of groups (i.e., maximum_ `m`_) into which you can divide the nodes_. Return `-1` _if it is impossible to group the nodes with the given conditions_.

__Example:__

Example 1:

![2493-1](https://assets.leetcode.com/uploads/2022/10/13/example1.png)

```text
Input: n = 6, edges = [[1,2],[1,4],[1,5],[2,6],[2,3],[4,6]]
Output: 4
Explanation: As shown in the image we:
```

- Add node 5 to the first group.
- Add node 1 to the second group.
- Add nodes 2 and 4 to the third group.
- Add nodes 3 and 6 to the fourth group.

We can see that every edge is satisfied.

It can be shown that that if we create a fifth group and move any node from the third or fourth group to it, at least on of the edges will not be satisfied.

Example 2:

```text
Input: n = 3, edges = [[1,2],[2,3],[3,1]]
Output: -1
Explanation: If we add node 1 to the first group, node 2 to the second group, and node 3 to the third group to satisfy the first two edges, we can see that the third edge will not be satisfied.
It can be shown that no grouping is possible.
```

__Constraints:__

- `1 <= n <= 500`
- `1 <= edges.length <= 10 ^ 4`
- `edges[i].length == 2`
- `1 <= ai, bi <= n`
- `ai != bi`
- There is at most one edge between any pair of vertices.

__题目描述:__

给你一个正整数 `n` ，表示一个 __无向__ 图中的节点数目，节点编号从 `1` 到 `n` 。

同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 和 `bi` 之间有一条 __双向__ 边。注意给定的图可能是不连通的。

请你将图划分为 `m` 个组（编号从 __1__ 开始），满足以下要求:

- 图中每个节点都只属于一个组。
- 图中每条边连接的两个点 `[ai, bi]` ，如果 `ai` 属于编号为 `x` 的组， `bi` 属于编号为 `y` 的组，那么 `|y - x| = 1` 。

请你返回最多可以将节点分为多少个组（也就是最大的 `m` ）。如果没办法在给定条件下分组，请你返回 `-1` 。

__示例:__

示例 1：

![2493-2](https://assets.leetcode.com/uploads/2022/10/13/example1.png)

```text
输入：n = 6, edges = [[1,2],[1,4],[1,5],[2,6],[2,3],[4,6]]
输出：4
解释：如上图所示，
```

- 节点 5 在第一个组。
- 节点 1 在第二个组。
- 节点 2 和节点 4 在第三个组。
- 节点 3 和节点 6 在第四个组。

所有边都满足题目要求。

如果我们创建第五个组，将第三个组或者第四个组中任何一个节点放到第五个组，至少有一条边连接的两个节点所属的组编号不符合题目要求。

示例 2：

```text
输入：n = 3, edges = [[1,2],[2,3],[3,1]]
输出：-1
解释：如果我们将节点 1 放入第一个组，节点 2 放入第二个组，节点 3 放入第三个组，前两条边满足题目要求，但第三条边不满足题目要求。
没有任何符合题目要求的分组方式。
```

__提示：__

- `1 <= n <= 500`
- `1 <= edges.length <= 10 ^ 4`
- `edges[i].length == 2`
- `1 <= ai, bi <= n`
- `ai != bi`
- 两个点之间至多只有一条边。

__思路:__

```text
二分图
不同的组可以用不同的 BFS 起点表示
枚举起点进行 BFS
保证 BFS 的路径中都是二分图
即要么不同色要么还未染色且能被染色成不同色
否则返回 -1
每次 BFS 计算当前起点的最大深度并累加到结果中
时间复杂度为 O(MN), 空间复杂度为 O(M + N), 其中 M 为边数，N 为节点数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int magnificentSets(int n, vector<vector<int>>& edges) 
    {
        int result = 0, clock = 0;
        vector<vector<int>> graph(n);
        vector<int> nodes, time(n), color(n);
        for (const auto& edge : edges)
        {
            graph[edge.front() - 1].emplace_back(edge.back() - 1);
            graph[edge.back() - 1].emplace_back(edge.front() - 1);
        }
        auto is_bipartite = [&](auto&& is_bipartite, int u, int c) -> bool
        {
            nodes.emplace_back(u);
            color[u] = c;
            for (int v : graph[u]) if (color[v] == c or !color[v] and !is_bipartite(is_bipartite, v, -c)) return false;
            return true;
        };
        auto bfs = [&](int start) -> int
        {
            int depth = 0;
            time[start] = ++clock;
            vector<int> q = {start};
            while (!q.empty())
            {
                vector<int> nxt;
                for (int u : q)
                {
                    for (int v : graph[u])
                    {
                        if (time[v] != clock)
                        {
                            time[v] = clock;
                            nxt.emplace_back(v);
                        }
                    }
                }
                q = move(nxt);
                ++depth;
            }
            return depth;
        };
        for (int i = 0; i < n; i++)
        {
            if (!color[i])
            {
                nodes.clear();
                if (!is_bipartite(is_bipartite, i, 1)) return -1;
                int max_depth = 0;
                for (int u : nodes) max_depth = max(max_depth, bfs(u));
                result += max_depth;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private final List<Integer> nodes = new ArrayList<>();
    private int clock = 0;

    public int magnificentSets(int n, int[][] edges) {
        int result = 0, time[] = new int[n], color[] = new int[n];
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0] - 1].add(edge[1] - 1);
            graph[edge[1] - 1].add(edge[0] - 1);
        }
        for (int i = 0; i < n; i++) {
            if (color[i] == 0) {
                nodes.clear();
                if (!isBipartite(i, 1, color)) return -1;
                int maxDepth = 0;
                for (var u : nodes) maxDepth = Math.max(maxDepth, bfs(u, time));
                result += maxDepth;
            }
        }
        return result;
    }

    private boolean isBipartite(int u, int c, int[] color) {
        nodes.add(u);
        color[u] = c;
        for (int v : graph[u]) if (color[v] == c || color[v] == 0 && !isBipartite(v, -c, color)) return false;
        return true;
    }

    private int bfs(int start, int[] time) {
        int depth = 0;
        time[start] = ++clock;
        var q = new ArrayList<Integer>();
        q.add(start);
        while (!q.isEmpty()) {
            var cur = q;
            q = new ArrayList<>();
            for (var u : cur) {
                for (var v : graph[u]) {
                    if (time[v] != clock) {
                        time[v] = clock;
                        q.add(v);
                    }
                }
            }
            ++depth;
        }
        return depth;
    }
}
```

__Python__:

```Python
class Solution 
{
public:
    int magnificentSets(int n, vector<vector<int>>& edges) 
    {
        int result = 0, clock = 0;
        vector<vector<int>> graph(n);
        vector<int> nodes, time(n), color(n);
        for (const auto& edge : edges)
        {
            graph[edge.front() - 1].emplace_back(edge.back() - 1);
            graph[edge.back() - 1].emplace_back(edge.front() - 1);
        }
        auto is_bipartite = [&](auto&& is_bipartite, int u, int c) -> bool
        {
            nodes.emplace_back(u);
            color[u] = c;
            for (int v : graph[u]) if (color[v] == c or !color[v] and !is_bipartite(is_bipartite, v, -c)) return false;
            return true;
        };
        auto bfs = [&](int start) -> int
        {
            int depth = 0;
            time[start] = ++clock;
            vector<int> q = {start};
            while (!q.empty())
            {
                vector<int> nxt;
                for (int u : q)
                {
                    for (int v : graph[u])
                    {
                        if (time[v] != clock)
                        {
                            time[v] = clock;
                            nxt.emplace_back(v);
                        }
                    }
                }
                q = move(nxt);
                ++depth;
            }
            return depth;
        };
        for (int i = 0; i < n; i++)
        {
            if (!color[i])
            {
                nodes.clear();
                if (!is_bipartite(is_bipartite, i, 1)) return -1;
                int max_depth = 0;
                for (int u : nodes) max_depth = max(max_depth, bfs(u));
                result += max_depth;
            }
        }
        return result;
    }
};
```
