# 2203 Minimum Weighted Subgraph With the Required Paths 得到要求路径的最小带权子图

__Description:__

You are given an integer `n` denoting the number of nodes of a __weighted directed__ graph. The nodes are numbered from `0` to `n - 1`.

You are also given a 2D integer array `edges` where `edges[i] = [fromi, toi, weighti]` denotes that there exists a __directed__ edge from `fromi` to `toi` with weight `weighti`.

Lastly, you are given three __distinct__ integers `src1`, `src2`, and `dest` denoting three distinct nodes of the graph.

Return _the __minimum weight__ of a subgraph of the graph such that it is __possible__ to reach_ `dest` _from both_ `src1` _and_ `src2` _via a set of edges of this subgraph_. In case such a subgraph does not exist, return `-1`.

A subgraph is a graph whose vertices and edges are subsets of the original graph. The weight of a subgraph is the sum of weights of its constituent edges.

__Example:__

Example 1:

![2203-1](https://assets.leetcode.com/uploads/2022/02/17/example1drawio.png)

```text
Input: n = 6, edges = [[0,2,2],[0,5,6],[1,0,3],[1,4,5],[2,1,1],[2,3,3],[2,3,4],[3,4,2],[4,5,1]], src1 = 0, src2 = 1, dest = 5
Output: 9
Explanation:
The above figure represents the input graph.
The blue edges represent one of the subgraphs that yield the optimal answer.
Note that the subgraph [[1,0,3],[0,5,6]] also yields the optimal answer. It is not possible to get a subgraph with less weight satisfying all the constraints.
```

Example 2:

![2203-2](https://assets.leetcode.com/uploads/2022/02/17/example2-1drawio.png)

```text
Input: n = 3, edges = [[0,1,1],[2,1,1]], src1 = 0, src2 = 1, dest = 2
Output: -1
Explanation:
The above figure represents the input graph.
It can be seen that there does not exist any path from node 1 to node 2, hence there are no subgraphs satisfying all the constraints.
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i].length == 3`
- `0 <= fromi, toi, src1, src2, dest <= n - 1`
- `fromi != toi`
- `src1`, `src2`, and `dest` are pairwise distinct.
- `1 <= weight[i] <= 10 ^ 5`

__题目描述:__

给你一个整数 `n` ，它表示一个 __带权有向__ 图的节点数，节点编号为 `0` 到 `n - 1` 。

同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [fromi, toi, weighti]` ，表示从 `fromi` 到 `toi` 有一条边权为 `weighti` 的 __有向__ 边。

最后，给你三个 __互不相同__ 的整数 `src1` ， `src2` 和 `dest` ，表示图中三个不同的点。

请你从图中选出一个 _边权和最小_ 的子图，使得从 `src1` 和 `src2` 出发，在这个子图中，都 __可以__ 到达 `dest` 。如果这样的子图不存在，请返回 `-1` 。

子图 中的点和边都应该属于原图的一部分。子图的边权和定义为它所包含的所有边的权值之和。

__示例:__

示例 1：

![2203-3](https://assets.leetcode.com/uploads/2022/02/17/example1drawio.png)

```text
输入：n = 6, edges = [[0,2,2],[0,5,6],[1,0,3],[1,4,5],[2,1,1],[2,3,3],[2,3,4],[3,4,2],[4,5,1]], src1 = 0, src2 = 1, dest = 5
输出：9
解释：
上图为输入的图。
蓝色边为最优子图之一。
注意，子图 [[1,0,3],[0,5,6]] 也能得到最优解，但无法在满足所有限制的前提下，得到更优解。
```

示例 2：

![2203-4](https://assets.leetcode.com/uploads/2022/02/17/example2-1drawio.png)

```text
输入：n = 3, edges = [[0,1,1],[2,1,1]], src1 = 0, src2 = 1, dest = 2
输出：-1
解释：
上图为输入的图。
可以看到，不存在从节点 1 到节点 2 的路径，所以不存在任何子图满足所有限制。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i].length == 3`
- `0 <= fromi, toi, src1, src2, dest <= n - 1`
- `fromi != toi`
- `src1` ， `src2` 和 `dest` 两两不同。
- `1 <= weight[i] <= 10 ^ 5`

__思路:__

```text
枚举最短路径
如果只有 src 到 dest 的最短路径, 我们可以使用 Dijkstra 算法求出
使用 Dijkstra 算法求出 src1, src2, dest 到所有点的最短路径, 从中选择最优解
其中 dest 到任意点的路径可由反图求出
时间复杂度为 O(N + MlogM), 空间复杂度为 O(M), M 为边的数量
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    long long minimumWeight(int n, vector<vector<int>>& edges, int src1, int src2, int dest) 
    {
        vector<vector<pair<int, int>>> graph(n), reverse_graph(n);
        for (const auto& e : edges)
        {
            int u = e[0], v = e[1], w = e[2];
            graph[u].emplace_back(v, w);
            reverse_graph[v].emplace_back(u, w);
        }
        auto d1 = dijkstra(graph, src1), d2 = dijkstra(graph, src2), d3 = dijkstra(reverse_graph, dest);
        long long result = LLONG_MAX / 3;
        for (int i = 0; i < n; i++) result = min(result, d1[i] + d2[i] + d3[i]);
        return result < LLONG_MAX / 3 ? result : -1;
    }
private:
    vector<long long> dijkstra(vector<vector<pair<int, int>>>& graph, int start)
    {
        vector<long long> dis(graph.size(), LLONG_MAX / 3);
        dis[start] = 0;
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
        pq.emplace(0LL, start);
        while (!pq.empty())
        {
            auto [d, u] = pq.top();
            pq.pop();
            if (d <= dis[u])
            {
                for (const auto& [v, w] : graph[u])
                {
                    long long new_d = w + dis[u];
                    if (new_d < dis[v])
                    {
                        dis[v] = new_d;
                        pq.emplace(new_d, v);
                    }
                }
            }
        }
        return dis;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumWeight(int n, int[][] edges, int src1, int src2, int dest) {
        List<Pair<Integer, Integer>>[] graph = new ArrayList[n], reverseGraph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        Arrays.setAll(reverseGraph, e -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1], w = e[2];
            graph[u].add(new Pair<>(v, w));
            reverseGraph[v].add(new Pair<>(u, w));
        }

        long d1[] = dijkstra(graph, src1), d2[] = dijkstra(graph, src2), d3[] = dijkstra(reverseGraph, dest), result = Long.MAX_VALUE / 3;
        for (int i = 0; i < n; i++) result = Math.min(result, d1[i] + d2[i] + d3[i]);
        return result < Long.MAX_VALUE / 3 ? result : -1;
    }

    private long[] dijkstra(List<Pair<Integer, Integer>>[] graph, int start) {
        var dis = new long[graph.length];
        Arrays.fill(dis, Long.MAX_VALUE / 3);
        dis[start] = 0;
        var pq = new PriorityQueue<Pair<Integer, Long>>(Comparator.comparingLong(Pair::getValue));
        pq.offer(new Pair<>(start, 0L));
        while (!pq.isEmpty()) {
            var u = pq.peek().getKey();
            var w = pq.poll().getValue();
            if (w <= dis[u]) {
                for (var e : graph[u]) {
                    var v = e.getKey();
                    var newD = w + e.getValue();
                    if (newD < dis[v]) {
                        dis[v] = newD;
                        pq.offer(new Pair<>(v, newD));
                    }
                }
            }
        }
        return dis;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumWeight(self, n: int, edges: List[List[int]], src1: int, src2: int, dest: int) -> int:
        def dijkstra(graph: List[List[tuple]], start: int) -> List[int]:
            dis, pq = [inf] * n, [(0, start)]
            dis[start] = 0
            while pq:
                d, u = heappop(pq)
                if d <= dis[u]:
                    for v, w in graph[u]:
                        if (new_d := dis[u] + w) < dis[v]:
                            dis[v] = new_d
                            heappush(pq, (new_d, v))
            return dis

        graph, reverse_graph = [[] for _ in range(n)], [[] for _ in range(n)]
        for u, v, w in edges:
            graph[u].append((v, w))
            reverse_graph[v].append((u, w))
        return result if (result := min(sum(d) for d in zip(dijkstra(graph, src1), dijkstra(graph, src2), dijkstra(reverse_graph, dest)))) < inf else -1
```
