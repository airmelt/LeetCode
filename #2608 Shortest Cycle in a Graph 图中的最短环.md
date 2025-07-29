# 2608 Shortest Cycle in a Graph 图中的最短环

__Description:__

There is a __bi-directional__ graph with `n` vertices, where each vertex is labeled from `0` to `n - 1`. The edges in the graph are represented by a given 2D integer array `edges`, where `edges[i] = [ui, vi]` denotes an edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

Return _the length of the __shortest__ cycle in the graph_. If no cycle exists, return `-1`.

A cycle is a path that starts and ends at the same node, and each edge in the path is used only once.

__Example:__

Example 1:

![2608-1](https://assets.leetcode.com/uploads/2023/01/04/cropped.png)

```text
Input: n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]
Output: 3
Explanation: The cycle with the smallest length is : 0 -> 1 -> 2 -> 0
```

Example 2:

![2608-2](https://assets.leetcode.com/uploads/2023/01/04/croppedagin.png)

```text
Input: n = 4, edges = [[0,1],[0,2]]
Output: -1
Explanation: There are no cycles in this graph.
```

__Constraints:__

- `2 <= n <= 1000`
- `1 <= edges.length <= 1000`
- `edges[i].length == 2`
- `0 <= ui, vi < n`
- `ui != vi`
- There are no repeated edges.

__题目描述:__

现有一个含 `n` 个顶点的 __双向__ 图，每个顶点按从 `0` 到 `n - 1` 标记。图中的边由二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和 `vi` 之间存在一条边。每对顶点最多通过一条边连接，并且不存在与自身相连的顶点。

返回图中 __最短__ 环的长度。如果不存在环，则返回 `-1` 。

环 是指以同一节点开始和结束，并且路径中的每条边仅使用一次。

__示例:__

示例 1：

![2608-3](https://assets.leetcode.com/uploads/2023/01/04/cropped.png)

```text
输入：n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]
输出：3
解释：长度最小的循环是：0 -> 1 -> 2 -> 0
```

示例 2：

![2608-4](https://assets.leetcode.com/uploads/2023/01/04/croppedagin.png)

```text
输入：n = 4, edges = [[0,1],[0,2]]
输出：-1
解释：图中不存在循环
```

__提示：__

- `2 <= n <= 1000`
- `1 <= edges.length <= 1000`
- `edges[i].length == 2`
- `0 <= ui, vi < n`
- `ui != vi`
- 不存在重复的边

__思路:__

```text
BFS
从每个节点开始 BFS 遍历, 记录每个节点的距离和父节点
记录每个点到出发点的最短路 dis
其中 u 是当前节点, v 是遍历到的节点
如果环的长度小于当前最短环长度, 则更新最短环长度
如果是第一次遇到该节点, 则将其加入队列继续 BFS, dis[v] = dis[u] + 1
如果是第二次遇到该节点且不是父节点, 则说明存在环,  计算环的长度
dis[u] + dis[v] + 1, 并更新最短环长度
最后返回最短环长度, 如果没有环则返回 -1
时间复杂度为 O(MN), 空间复杂度为 O(M + N), 其中 M 为边数, N 为节点数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findShortestCycle(int n, vector<vector<int>>& edges) 
    {
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        auto bfs = [&](int start) -> int
        {
            vector<int> dis(n, -1);
            dis[start] = 0;
            int cur = INT_MAX;
            queue<pair<int, int>> q;
            q.emplace(start, -1);
            while (!q.empty())
            {
                auto node = q.front();
                q.pop();
                for (const auto& v : graph[node.first])
                {
                    if (dis[v] < 0)
                    {
                        dis[v] = dis[node.first] + 1;
                        q.emplace(v, node.first);
                    } else if (v != node.second) cur = min(cur, dis[node.first] + dis[v] + 1);
                }
            }
            return cur;
        };
        int result = INT_MAX;
        for (int i = 0; i < n; i++) result = min(result, bfs(i));
        return result < INT_MAX ? result : -1;
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private int[] dis;

    public int findShortestCycle(int n, int[][] edges) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        dis = new int[n];
        int result = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) result = Math.min(result, bfs(i));
        return result < Integer.MAX_VALUE ? result : -1;
    }

    private int bfs(int start) {
        int cur = Integer.MAX_VALUE;
        Arrays.fill(dis, -1);
        dis[start] = 0;
        var q = new ArrayDeque<int[]>();
        q.add(new int[]{ start, -1 });
        while (!q.isEmpty()) {
            int node[] = q.poll(), u = node[0], fa = node[1];
            for (int v : graph[u]) {
                if (dis[v] < 0) {
                    dis[v] = dis[u] + 1;
                    q.add(new int[]{ v, u });
                } else if (v != fa) cur = Math.min(cur, dis[u] + dis[v] + 1);
            }
        }
        return cur;
    }
}
```

__Python__:

```Python
class Solution:
    def findShortestCycle(self, n: int, edges: List[List[int]]) -> int:
        graph = defaultdict(set)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def bfs(start: int) -> int:
            cur, dis = inf, [-1] * n
            dis[start], q = 0, deque([(start, -1)])
            while q:
                u, fa = q.popleft()
                for v in graph[u]:
                    if dis[v] < 0:
                        dis[v] = dis[u] + 1
                        q.append((v, u))
                    elif v != fa:
                        cur = min(cur, dis[u] + dis[v] + 1)
            return cur
        return result if (result := min(bfs(i) for i in range(n))) < inf else -1
```
