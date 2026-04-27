# 2642 Design Graph With Shortest Path Calculator 设计可以求最短路径的图类

__Description:__

There is a __directed weighted__ graph that consists of `n` nodes numbered from `0` to `n - 1`. The edges of the graph are initially represented by the given array `edges` where `edges[i] = [from_i, to_i, edgeCost_i]` meaning that there is an edge from `from_i` to `to_i` with the cost `edgeCost_i`.

Implement the `Graph` class:

- `Graph(int n, int[][] edges)` initializes the object with `n` nodes and the given edges.
- `addEdge(int[] edge)` adds an edge to the list of edges where `edge = [from, to, edgeCost]`. It is guaranteed that there is no edge between the two nodes before adding this one.
- `int shortestPath(int node1, int node2)` returns the __minimum__ cost of a path from `node1` to `node2`. If no path exists, return `-1`. The cost of a path is the sum of the costs of the edges in the path.

__Example:__

Example 1:

![2642-1](https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png)

```text
Input
["Graph", "shortestPath", "shortestPath", "addEdge", "shortestPath"]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
Output
[null, 6, -1, null, 6]

Explanation
```

```Java
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // return 6. The shortest path from 3 to 2 in the first diagram above is 3 -> 0 -> 1 -> 2 with a total cost of 3 + 2 + 1 = 6.
g.shortestPath(0, 3); // return -1. There is no path from 0 to 3.
g.addEdge([1, 3, 4]); // We add an edge from node 1 to node 3, and we get the second diagram above.
g.shortestPath(0, 3); // return 6. The shortest path from 0 to 3 now is 0 -> 1 -> 3 with a total cost of 2 + 4 = 6.
```

__Constraints:__

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1)`
- `edges[i].length == edge.length == 3`
- `0 <= from_i, to_i, from, to, node1, node2 <= n - 1`
- `1 <= edgeCost_i, edgeCost <= 10 ^ 6`
- There are no repeated edges and no self-loops in the graph at any point.
- At most `100` calls will be made for `addEdge`.
- At most `100` calls will be made for `shortestPath`.

__题目描述:__

给你一个有 `n` 个节点的 __有向带权__ 图，节点编号为 `0` 到 `n - 1` 。图中的初始边用数组 `edges` 表示，其中 `edges[i] = [from_i, to_i, edgeCost_i]` 表示从 `from_i` 到 `to_i` 有一条代价为 `edgeCost_i` 的边。

请你实现一个 `Graph` 类:

- `Graph(int n, int[][] edges)` 初始化图有 `n` 个节点，并输入初始边。
- `addEdge(int[] edge)` 向边集中添加一条边，其中 `edge = [from, to, edgeCost]` 。数据保证添加这条边之前对应的两个节点之间没有有向边。
- `int shortestPath(int node1, int node2)` 返回从节点 `node1` 到 `node2` 的路径 __最小__ 代价。如果路径不存在，返回 `-1` 。一条路径的代价是路径中所有边代价之和。

__示例:__

示例 1：

![2642-2](https://assets.leetcode.com/uploads/2023/01/11/graph3drawio-2.png)

```text
输入：
["Graph", "shortestPath", "shortestPath", "addEdge", "shortestPath"]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
输出：
[null, 6, -1, null, 6]

解释：
```

```Java
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // 返回 6 。从 3 到 2 的最短路径如第一幅图所示：3 -> 0 -> 1 -> 2 ，总代价为 3 + 2 + 1 = 6 。
g.shortestPath(0, 3); // 返回 -1 。没有从 0 到 3 的路径。
g.addEdge([1, 3, 4]); // 添加一条节点 1 到节点 3 的边，得到第二幅图。
g.shortestPath(0, 3); // 返回 6 。从 0 到 3 的最短路径为 0 -> 1 -> 3 ，总代价为 2 + 4 = 6 。
```

__提示：__

- `1 <= n <= 100`
- `0 <= edges.length <= n * (n - 1)`
- `edges[i].length == edge.length == 3`
- `0 <= from_i, to_i, from, to, node1, node2 <= n - 1`
- `1 <= edgeCost_i, edgeCost <= 10 ^ 6`
- 图中任何时候都不会有重边和自环。
- 调用 `addEdge` 至多 `100` 次。
- 调用 `shortestPath` 至多 `100` 次。

__思路:__

```text
Dijkstra ➕ 优先队列
每次加入边时直接加入到邻接矩阵中
计算最短路径时使用优先队列, 记录最短距离和当前位置
遍历所有邻接点, 如果该邻接点恰好是终点, 直接返回距离
否则对邻接点进行松弛操作
只有当 d <= dist[u] 才需要更新, 因为此前已经更新过 u
初始化时间复杂度为 O(M + N), addEdge 时间复杂度为 O(1), shortestPath 时间复杂度为 O(MlogM + N), 若图为稠密图, shortestPath 时间复杂度为 O(N ^ 2logN), 空间复杂度为 O(M + N), M 为边的数量, 即 edges 加上 addEdge 的调用次数
```

__代码:__

__C++__:

```C++
class Graph 
{
public:
    Graph(int n, vector<vector<int>>& edges) : graph(n) 
    {
        for (const auto& edge : edges) addEdge(edge);    
    }
    
    void addEdge(vector<int> edge) 
    {
        graph[edge[0]].emplace_back(edge[1], edge[2]);
    }
    
    int shortestPath(int node1, int node2) 
    {
        vector<int> dist(graph.size(), INT_MAX);
        dist[node1] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.push({ 0, node1 });
        while (!pq.empty()) 
        {
            auto [d, u] = pq.top();
            pq.pop();
            if (u == node2) return d;
            for (const auto& [v, w] : graph[u]) 
            {
                if (d + w < dist[v]) 
                {
                    dist[v] = d + w;
                    pq.push({ dist[v], v });
                }
            }
        }
        return -1;
    }
private:
    vector<vector<pair<int, int>>> graph;
};

/**
 * Your Graph object will be instantiated and called as such:
 * Graph* obj = new Graph(n, edges);
 * obj->addEdge(edge);
 * int param_2 = obj->shortestPath(node1,node2);
 */
```

__Java__:

```Java
class Graph {
    private final List<int[]>[] graph;

    public Graph(int n, int[][] edges) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, k -> new ArrayList<>());
        for (int[] edge : edges) addEdge(edge);
    }
    
    public void addEdge(int[] edge) {
        graph[edge[0]].add(new int[]{ edge[1], edge[2] });
    }
    
    public int shortestPath(int node1, int node2) {
        int[] dist = new int[graph.length];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[node1] = 0;
        var pq = new PriorityQueue<int[]>((a, b) -> (a[0] - b[0]));
        pq.offer(new int[]{ 0, node1 });
        while (!pq.isEmpty()) {
            int edge[] = pq.poll(), d = edge[0], u = edge[1];
            if (u == node2) return d;
            for (int[] neighbor : graph[u]) {
                if (d + neighbor[1] < dist[neighbor[0]]) {
                    dist[neighbor[0]] = d + neighbor[1];
                    pq.offer(new int[]{ dist[neighbor[0]], neighbor[0] });
                }
            }
        }
        return -1;
    }
}

/**
 * Your Graph object will be instantiated and called as such:
 * Graph obj = new Graph(n, edges);
 * obj.addEdge(edge);
 * int param_2 = obj.shortestPath(node1,node2);
 */
```

__Python__:

```Python
class Graph:

    def __init__(self, n: int, edges: List[List[int]]):
        self.graph = [[] for _ in range(n)]
        for u, v, w in edges:
            self.graph[u].append([v, w])
        

    def addEdge(self, edge: List[int]) -> None:
        self.graph[edge[0]].append([edge[1], edge[2]])
        

    def shortestPath(self, node1: int, node2: int) -> int:
        dist, heap = [inf] * node1 + [0] + [inf] * (len(self.graph) - node1 - 1), [[0, node1]]
        while heap:
            d, u = heappop(heap)
            if u == node2:
                return d
            for v, w in self.graph[u]:
                if d + w < dist[v]:
                    dist[v] = d + w
                    heappush(heap, (dist[v], v))
        return -1



# Your Graph object will be instantiated and called as such:
# obj = Graph(n, edges)
# obj.addEdge(edge)
# param_2 = obj.shortestPath(node1,node2)
```
