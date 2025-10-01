# 2685 Count the Number of Complete Components 统计完全连通分量的数量

__Description:__

You are given an integer `n`. There is an __undirected__ graph with `n` vertices, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an __undirected__ edge connecting vertices `ai` and `bi`.

Return the number of complete connected components of the graph.

A connected component is a subgraph of a graph in which there exists a path between any two vertices, and no vertex of the subgraph shares an edge with a vertex outside of the subgraph.

A connected component is said to be complete if there exists an edge between every pair of its vertices.

__Example:__

Example 1:

![2685-1](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)

```text
Input: n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]
Output: 3
Explanation: From the picture above, one can see that all of the components of this graph are complete.
```

Example 2:

![2685-2](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)

```text
Input: n = 6, edges = [[0,1],[0,2],[1,2],[3,4],[3,5]]
Output: 1
Explanation: The component containing vertices 0, 1, and 2 is complete since there is an edge between every pair of two vertices. On the other hand, the component containing vertices 3, 4, and 5 is not complete since there is no edge between vertices 4 and 5. Thus, the number of complete components in this graph is 1.
```

__Constraints:__

- `1 <= n <= 50`
- `0 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no repeated edges.

__题目描述:__

给你一个整数 `n` 。现有一个包含 `n` 个顶点的 __无向__ 图，顶点按从 `0` 到 `n - 1` 编号。给你一个二维整数数组 `edges` 其中 `edges[i] = [ai, bi]` 表示顶点 `ai` 和 `bi` 之间存在一条 __无向__ 边。

返回图中 完全连通分量 的数量。

如果在子图中任意两个顶点之间都存在路径，并且子图中没有任何一个顶点与子图外部的顶点共享边，则称其为 连通分量 。

如果连通分量中每对节点之间都存在一条边，则称其为 完全连通分量 。

__示例:__

示例 1：

![2685-3](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-31-23.png)

```text
输入：n = 6, edges = [[0,1],[0,2],[1,2],[3,4]]
输出：3
解释：如上图所示，可以看到此图所有分量都是完全连通分量。
```

示例 2：

![2685-4](https://assets.leetcode.com/uploads/2023/04/11/screenshot-from-2023-04-11-23-32-00.png)

```text
输入：n = 6, edges = [[0,1],[0,2],[1,2],[3,4],[3,5]]
输出：1
解释：包含节点 0、1 和 2 的分量是完全连通分量，因为每对节点之间都存在一条边。
包含节点 3 、4 和 5 的分量不是完全连通分量，因为节点 4 和 5 之间不存在边。
因此，在图中完全连接分量的数量是 1 。
```

__提示：__

- `1 <= n <= 50`
- `0 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- 不存在重复的边

__思路:__

```text
DFS
在 DFS 节点的同时记录下节点的个数和边的数量
如果是完全联通分量则每对节点都存在一条边
那么节点数 v 和边数 e 对应关系为
v * (v - 1) >> 1 = e
因为每个节点都连接其他边, 即 C(v, 2) = e
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), 其中 n 为节点数, m 为边数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) 
    {
        vector<vector<int>> graph(n);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        vector<bool> visited(n);
        int result = 0, v = 0, e = 0;
        auto dfs = [&](auto&& dfs, int x) -> void
        {
            visited[x] = true;
            ++v;
            e += graph[x].size();
            for (const auto& y : graph[x]) if (!visited[y]) dfs(dfs, y);
        };
        for (int i = 0; i < n; i++) 
        {
            if (!visited[i]) 
            {
                v = e = 0;
                dfs(dfs, i);
                result += e == v * (v - 1);
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
    private boolean visited[];
    private int v, e, result;

    public int countCompleteComponents(int n, int[][] edges) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        visited = new boolean[n];
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                v = e = 0;
                dfs(i);
                if (e == v * (v - 1)) ++result;
            }
        }
        return result;
    }

    private void dfs(int x) {
        visited[x] = true;
        ++v;
        e += graph[x].size();
        for (int y : graph[x]) if (!visited[y]) dfs(y);
    }
}
```

__Python__:

```Python
class Solution:
    def countCompleteComponents(self, n: int, edges: List[List[int]]) -> int:
        graph, result, visited = defaultdict(set), 0, set()
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(x: int) -> None:
            visited.add(x)
            nonlocal v, e
            v += 1
            e += len(graph[x])
            for y in graph[x]:
                if y not in visited:
                    dfs(y)
        for i in range(n):
            if i not in visited:
                v = e = 0
                dfs(i)
                result += e == v * (v - 1)
        return result
```
