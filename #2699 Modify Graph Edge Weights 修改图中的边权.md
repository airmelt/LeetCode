# 2699 Modify Graph Edge Weights 修改图中的边权

__Description:__

You are given an __undirected weighted__ __connected__ graph containing `n` nodes labeled from `0` to `n - 1`, and an integer array `edges` where `edges[i] = [ai, bi, wi]` indicates that there is an edge between nodes `ai` and `bi` with weight `wi`.

Some edges have a weight of `-1` ( `wi = -1`), while others have a __positive__ weight ( `wi > 0`).

Your task is to modify __all edges__ with a weight of `-1` by assigning them __positive integer values__ in the range `[1, 2 * 10 ^ 9]` so that the __shortest distance__ between the nodes `source` and `destination` becomes equal to an integer `target`. If there are __multiple__ __modifications__ that make the shortest distance between `source` and `destination` equal to `target`, any of them will be considered correct.

Return _an array containing all edges (even unmodified ones) in any order if it is possible to make the shortest distance from_ `source` _to_ `destination` _equal to_ `target`_, or an __empty array__ if it's impossible._

Note: You are not allowed to modify the weights of edges with initial positive weights.

__Example:__

Example 1:

![2699-1](https://assets.leetcode.com/uploads/2023/04/18/graph.png)

```text
Input: n = 5, edges = [[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]], source = 0, destination = 1, target = 5
Output: [[4,1,1],[2,0,1],[0,3,3],[4,3,1]]
Explanation: The graph above shows a possible modification to the edges, making the distance from 0 to 1 equal to 5.
```

Example 2:

![2699-2](https://assets.leetcode.com/uploads/2023/04/18/graph-2.png)

```text
Input: n = 3, edges = [[0,1,-1],[0,2,5]], source = 0, destination = 2, target = 6
Output: []
Explanation: The graph above contains the initial edges. It is not possible to make the distance from 0 to 2 equal to 6 by modifying the edge with weight -1. So, an empty array is returned.
```

Example 3:

![2699-3](https://assets.leetcode.com/uploads/2023/04/19/graph-3.png)

```text
Input: n = 4, edges = [[1,0,4],[1,2,3],[2,3,5],[0,3,-1]], source = 0, destination = 2, target = 6
Output: [[1,0,4],[1,2,3],[2,3,5],[0,3,1]]
Explanation: The graph above shows a modified graph having the shortest distance from 0 to 2 as 6.
```

__Constraints:__

- `1 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `wi = -1`or `1 <= wi <= 10 ^ 7`
- `ai != bi`
- `0 <= source, destination < n`
- `source != destination`
- `1 <= target <= 10 ^ 9`
- The graph is connected, and there are no self-loops or repeated edges

__题目描述:__

给你一个 `n` 个节点的 __无向带权连通__ 图，节点编号为 `0` 到 `n - 1` ，再给你一个整数数组 `edges` ，其中 `edges[i] = [ai, bi, wi]` 表示节点 `ai` 和 `bi` 之间有一条边权为 `wi` 的边。

部分边的边权为 `-1`（ `wi = -1`），其他边的边权都为 __正__ 数（ `wi > 0`）。

你需要将所有边权为 `-1` 的边都修改为范围 `[1, 2 * 10 ^ 9]` 中的 __正整数__ ，使得从节点 `source` 到节点 `destination` 的 __最短距离__ 为整数 `target` 。如果有 __多种__ 修改方案可以使 `source` 和 `destination` 之间的最短距离等于 `target` ，你可以返回任意一种方案。

如果存在使 `source` 到 `destination` 最短距离为 `target` 的方案，请你按任意顺序返回包含所有边的数组（包括未修改边权的边）。如果不存在这样的方案，请你返回一个 __空数组__ 。

注意：你不能修改一开始边权为正数的边。

__示例:__

示例 1：

![2699-4](https://assets.leetcode.com/uploads/2023/04/18/graph.png)

```text
输入：n = 5, edges = [[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]], source = 0, destination = 1, target = 5
输出：[[4,1,1],[2,0,1],[0,3,3],[4,3,1]]
解释：上图展示了一个满足题意的修改方案，从 0 到 1 的最短距离为 5 。
```

示例 2：

![2699-5](https://assets.leetcode.com/uploads/2023/04/18/graph-2.png)

```text
输入：n = 3, edges = [[0,1,-1],[0,2,5]], source = 0, destination = 2, target = 6
输出：[]
解释：上图是一开始的图。没有办法通过修改边权为 -1 的边，使得 0 到 2 的最短距离等于 6 ，所以返回一个空数组。
```

示例 3：

![2699-6](https://assets.leetcode.com/uploads/2023/04/19/graph-3.png)

```text
输入：n = 4, edges = [[1,0,4],[1,2,3],[2,3,5],[0,3,-1]], source = 0, destination = 2, target = 6
输出：[[1,0,4],[1,2,3],[2,3,5],[0,3,1]]
解释：上图展示了一个满足题意的修改方案，从 0 到 2 的最短距离为 6 。
```

__提示：__

- `1 <= n <= 100`
- `1 <= edges.length <= n * (n - 1) / 2`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `wi = -1` 或者 `1 <= wi <= 10 ^ <span>7`
- `ai != bi`
- `0 <= source, destination < n`
- `source != destination`
- `1 <= target <= 10 ^ 9`
- 输入的图是连通图，且没有自环和重边。

__思路:__

```text
dijkstra
设到 source 的距离为 dis, dis[source] = 0
初始化其他的 dis 为 inf
如果把所有 -1 都用 1 替换
能得到 source 到 destination 的最短路径
如果这个路径都比 target 要大, 那么一定不可达, 最短路径都比 target
大, 直接返回空数组
否则记录下 delta = target - dis[destination][0], 0 表示第一次 dijkstra 的最短路径
对于第一次 dijkstra 还没确定的值可以进行修改, 即 edges[i][2] = -1 的部分
现在需要修改 source - u - v - destination 的 u 到 v 的路径的值
source 到 u 的路径由第二次 dijkstra 得到, 即 dis[u][1]
u 到 v 即 w 为一个变量, 未修改时为 -1
v 到 destination 还未更新, 故可由第一次 dijkstra 得到, 即 dis[destination][0] - dis[v][0]
上面的路径和即 target
所以有 dis[u][1] + w + dis[destination][0] - dis[v][0] = target
w = target - dis[destination][0] + dis[v][0] - dis[u][1] = delta + dis[v][0] - dis[u][1]
如果跑完第二次 dijkstra, 得到的最短路径比 target 还小, 说明最短路径不能增加到 target, 也不可达, 返回空数组
否则将其他未修改的值改为 1 即可, 返回 edges
时间复杂度为 O(N ^ 2), 空间复杂度为 O(M + N), M 为 edges 的数量, 即边数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges, int source, int destination, int target) 
    {
        vector<pair<int, int>> graph[n];
        for (int i = 0, m = edges.size(); i < m; i++) 
        {
            graph[edges[i][0]].emplace_back(edges[i][1], i);
            graph[edges[i][1]].emplace_back(edges[i][0], i);
        }
        int dis[n][2], delta, visited[n], inf = 0x3f3f3f3f;
        memset(dis, inf, sizeof(dis));
        dis[source][0] = dis[source][1] = 0;
        auto dijkstra = [&](int k)
        {
            memset(visited, 0, sizeof(visited));
            while (true)
            {
                int u = -1, wt, w;
                for (int v = 0; v < n; v++) if (!visited[v] and (u < 0 or dis[v][k] < dis[u][k])) u = v;
                if (u == destination) return;
                visited[u] = 1;
                for (auto& [v, i] : graph[u])
                {
                    if ((wt = edges[i].back()) == -1) wt = 1;
                    if (k and edges[i].back() == -1) if ((w = delta + dis[v][0] - dis[u][1]) > wt) edges[i][2] = wt = w;
                    dis[v][k] = min(dis[v][k], dis[u][k] + wt);
                }
            }
        };
        dijkstra(0);
        if ((delta = target - dis[destination][0]) < 0) return {};
        dijkstra(1);
        if (dis[destination][1] < target) return {};
        for (auto& edge : edges) if (edge.back() == -1) edge.back() = 1;
        return edges;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] modifiedGraphEdges(int n, int[][] edges, int source, int destination, int target) {
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int i = 0, m = edges.length; i < m; i++) {
            graph[edges[i][0]].add(new int[]{ edges[i][1], i });
            graph[edges[i][1]].add(new int[]{ edges[i][0], i });
        }
        int dis[][] = new int[n][2], inf = 0x3f3f3f3f, delta = 0;
        for (int i = 0; i < n; i++) if (i != source) dis[i][0] = dis[i][1] = inf;
        dijkstra(graph, edges, destination, dis, 0, 0);
        if ((delta = target - dis[destination][0]) < 0) return new int[][]{};
        dijkstra(graph, edges, destination, dis, delta, 1);
        if (dis[destination][1] < target) return new int[][]{};
        for (int[] edge : edges) if (edge[2] == -1) edge[2] = 1;
        return edges;
    }

    private void dijkstra(List<int[]>[] graph, int[][] edges, int destination, int[][] dis, int delta, int k) {
        int n = graph.length;
        boolean[] visited = new boolean[n];
        while (true) {
            int u = -1;
            for (int v = 0; v < n; v++) if (!visited[v] && (u < 0 || dis[v][k] < dis[u][k])) u = v;
            if (u == destination) return;
            visited[u] = true;
            for (int[] edge : graph[u]) {
                int v = edge[0], i = edge[1], wt = edges[i][2];
                if (wt == -1) wt = 1;
                if (k == 1 && edges[i][2] == -1) if (delta + dis[v][0] - dis[u][1] > wt) edges[i][2] = wt = delta + dis[v][0] - dis[u][1];
                dis[v][k] = Math.min(dis[v][k], dis[u][k] + wt);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def modifiedGraphEdges(self, n: int, edges: List[List[int]], source: int, destination: int, target: int) -> List[List[int]]:
        graph, dis = defaultdict(list), [[inf, inf] for _ in range(n)]
        for i, (u, v, _) in enumerate(edges):
            graph[u].append((v, i))
            graph[v].append((u, i))
        dis[source] = [0, 0]

        def dijkstra(flag: bool) -> None:
            visited = [False] * n
            while True:
                u = -1
                for v, (f, d) in enumerate(zip(visited, dis)):
                    if not f and (u < 0 or d[flag] < dis[u][flag]):
                        u = v
                if u == destination:
                    return
                visited[u] = True
                for v, i in graph[u]:
                    if (wt := edges[i][2]) == -1:
                        wt = 1
                    if flag and edges[i][2] == -1:
                        if (w := delta + dis[v][0] - dis[u][1]) > wt:
                            edges[i][2] = wt = w
                    dis[v][flag] = min(dis[v][flag], dis[u][flag] + wt)
        dijkstra(0)
        if (delta := target - dis[destination][0]) < 0:
            return []
        dijkstra(1)
        if dis[destination][1] < target:
            return []
        return [[e[0], e[1], 1] if e[2] == -1 else e for e in edges]
```
