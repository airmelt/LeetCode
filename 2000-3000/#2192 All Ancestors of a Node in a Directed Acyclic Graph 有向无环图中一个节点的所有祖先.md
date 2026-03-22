# 2192 All Ancestors of a Node in a Directed Acyclic Graph 有向无环图中一个节点的所有祖先

__Description:__

You are given a positive integer `n` representing the number of nodes of a __Directed Acyclic Graph__ (DAG). The nodes are numbered from `0` to `n - 1` (__inclusive__).

You are also given a 2D integer array `edges`, where `edges[i] = [fromi, toi]` denotes that there is a __unidirectional__ edge from `fromi` to `toi` in the graph.

Return _a list_ `answer`_, where_ `answer[i]` _is the __list of ancestors__ of the_ `i ^ th` _node, sorted in __ascending order___.

A node `u` is an __ancestor__ of another node `v` if `u` can reach `v` via a set of edges.

__Example:__

Example 1:

![2192-1](https://assets.leetcode.com/uploads/2019/12/12/e1.png)

```text
Input: n = 8, edgeList = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]
Output: [[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]
Explanation:
The above diagram represents the input graph.
```

- Nodes 0, 1, and 2 do not have any ancestors.
- Node 3 has two ancestors 0 and 1.
- Node 4 has two ancestors 0 and 2.
- Node 5 has three ancestors 0, 1, and 3.
- Node 6 has five ancestors 0, 1, 2, 3, and 4.
- Node 7 has four ancestors 0, 1, 2, and 3.

Example 2:

![2192-2](https://assets.leetcode.com/uploads/2019/12/12/e2.png)

```text
Input: n = 5, edgeList = [[0,1],[0,2],[0,3],[0,4],[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Output: [[],[0],[0,1],[0,1,2],[0,1,2,3]]
Explanation:
The above diagram represents the input graph.
```

- Node 0 does not have any ancestor.
- Node 1 has one ancestor 0.
- Node 2 has two ancestors 0 and 1.
- Node 3 has three ancestors 0, 1, and 2.
- Node 4 has four ancestors 0, 1, 2, and 3.

__Constraints:__

- `1 <= n <= 1000`
- `0 <= edges.length <= min(2000, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi <= n - 1`
- `fromi != toi`
- There are no duplicate edges.
- The graph is __directed__ and __acyclic__.

__题目描述:__

给你一个正整数 `n` ，它表示一个 __有向无环图__ 中节点的数目，节点编号为 `0` 到 `n - 1` （包括两者）。

给你一个二维整数数组 `edges` ，其中 `edges[i] = [fromi, toi]` 表示图中一条从 `fromi` 到 `toi` 的单向边。

请你返回一个数组 `answer`，其中 `answer[i]`是第 `i` 个节点的所有 __祖先__ ，这些祖先节点 __升序__ 排序。

如果 `u` 通过一系列边，能够到达 `v` ，那么我们称节点 `u` 是节点 `v` 的 __祖先__ 节点。

__示例:__

示例 1：

![2192-3](https://assets.leetcode.com/uploads/2019/12/12/e1.png)

```text
输入：n = 8, edgeList = [[0,3],[0,4],[1,3],[2,4],[2,7],[3,5],[3,6],[3,7],[4,6]]
输出：[[],[],[],[0,1],[0,2],[0,1,3],[0,1,2,3,4],[0,1,2,3]]
解释：
上图为输入所对应的图。
```

- 节点 0 ，1 和 2 没有任何祖先。
- 节点 3 有 2 个祖先 0 和 1 。
- 节点 4 有 2 个祖先 0 和 2 。
- 节点 5 有 3 个祖先 0 ，1 和 3 。
- 节点 6 有 5 个祖先 0 ，1 ，2 ，3 和 4 。
- 节点 7 有 4 个祖先 0 ，1 ，2 和 3 。

示例 2：

![2192-4](https://assets.leetcode.com/uploads/2019/12/12/e2.png)

```text
输入：n = 5, edgeList = [[0,1],[0,2],[0,3],[0,4],[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
输出：[[],[0],[0,1],[0,1,2],[0,1,2,3]]
解释：
上图为输入所对应的图。
```

- 节点 0 没有任何祖先。
- 节点 1 有 1 个祖先 0 。
- 节点 2 有 2 个祖先 0 和 1 。
- 节点 3 有 3 个祖先 0 ，1 和 2 。
- 节点 4 有 4 个祖先 0 ，1 ，2 和 3 。

__提示：__

- `1 <= n <= 1000`
- `0 <= edges.length <= min(2000, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi <= n - 1`
- `fromi != toi`
- 图中不会有重边。
- 图是 __有向__ 且 __无环__ 的。

__思路:__

```text
DFS
使用邻接表存储图
对于每个节点, 使用 visited 数组记录是否访问过
初始化 visited 为 -1
对于每个节点, 从该节点开始 DFS, 记录所有访问过的节点
访问时 visited[u] = start, 表示从 start 节点开始访问
如果访问到的节点 visited[v] != start, 则 v 是 u 的祖先节点
时间复杂度为 O(N(N + M)), 空间复杂度为 O(N + M), M 为 edges 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> getAncestors(int n, vector<vector<int>>& edges) 
    {
        vector<vector<int>> graph(n), result(n);
        for (const auto& edge : edges) graph[edge.front()].emplace_back(edge.back());
        vector<int> visited(n, -1);
        for (int start = 0; start < n; start++) dfs(start, start, graph, result, visited);
        return result;
    }
private:
    void dfs(int u, int start, vector<vector<int>>& graph, vector<vector<int>>& result, vector<int>& visited) 
    {
        visited[u] = start;
        for (const auto& v : graph[u]) 
        {
            if (visited[v] != start) 
            {
                result[v].emplace_back(start);
                dfs(v, start, graph, result, visited);
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> getAncestors(int n, int[][] edges) {
        List<Integer> graph[] = new ArrayList[n], result[] = new ArrayList[n];
        Arrays.setAll(graph, i -> new ArrayList<>());
        Arrays.setAll(result, i -> new ArrayList<>());
        for (var edge : edges) graph[edge[0]].add(edge[1]);
        int[] visited = new int[n];
        Arrays.fill(visited, -1);
        for (int start = 0; start < n; start++) dfs(start, start, graph, result, visited);
        return Arrays.asList(result);
    }

    private void dfs(int u, int start, List<Integer>[] graph, List<Integer>[] result, int[] visited) {
        visited[u] = start;
        for (int v : graph[u]) {
            if (visited[v] != start) {
                result[v].add(start);
                dfs(v, start, graph, result, visited);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def getAncestors(self, n: int, edges: List[List[int]]) -> List[List[int]]:
        graph = defaultdict(set)
        for e in edges:
            graph[e[0]].add(e[1])
        result = [set() for _ in range(n)]
        for i in range(n):
            queue, visited = deque(graph[i]), {i}
            while queue:
                cur = queue.popleft()
                result[cur].add(i)
                for v in graph[cur]:
                    if v not in visited:
                        queue.append(v)
                        visited.add(v)
        return [sorted(i) for i in result]
```
