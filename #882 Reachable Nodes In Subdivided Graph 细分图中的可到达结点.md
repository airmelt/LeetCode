# 882 Reachable Nodes In Subdivided Graph 细分图中的可到达结点

__Description__:
You are given an undirected graph (the "original graph") with n nodes labeled from 0 to n - 1. You decide to subdivide each edge in the graph into a chain of nodes, with the number of new nodes varying between each edge.

The graph is given as a 2D array of edges where edges[i] = [ui, vi, cnti] indicates that there is an edge between nodes ui and vi in the original graph, and cnti is the total number of new nodes that you will subdivide the edge into. Note that cnti == 0 means you will not subdivide the edge.

To subdivide the edge [ui, vi], replace it with (cnti + 1) new edges and cnti new nodes. The new nodes are x1, x2, ..., xcnti, and the new edges are [ui, x1], [x1, x2], [x2, x3], ..., [xcnti-1, xcnti], [xcnti, vi].

In this new graph, you want to know how many nodes are reachable from the node 0, where a node is reachable if the distance is maxMoves or less.

Given the original graph and maxMoves, return the number of nodes that are reachable from node 0 in the new graph.

__Example:__

Example 1:

![graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/01/origfinal.png)

Input: edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
Output: 13
Explanation: The edge subdivisions are shown in the image above.
The nodes that are reachable are highlighted in yellow.

Example 2:

Input: edges = [[0,1,4],[1,2,6],[0,2,8],[1,3,1]], maxMoves = 10, n = 4
Output: 23

Example 3:

Input: edges = [[1,2,4],[1,4,5],[1,3,1],[2,3,4],[3,4,5]], maxMoves = 17, n = 5
Output: 1
Explanation: Node 0 is disconnected from the rest of the graph, so only node 0 is reachable.

__Constraints:__

0 <= edges.length <= min(n * (n - 1) / 2, 10^4)
edges[i].length == 3
0 <= ui < vi < n
There are no multiple edges in the graph.
0 <= cnti <= 10^4
0 <= maxMoves <= 10^9
1 <= n <= 3000

__题目描述__:
给你一个无向图（原始图），图中有 n 个节点，编号从 0 到 n - 1 。你决定将图中的每条边细分为一条节点链，每条边之间的新节点数各不相同。

图用由边组成的二维数组 edges 表示，其中 edges[i] = [ui, vi, cnti] 表示原始图中节点 ui 和 vi 之间存在一条边，cnti 是将边细分后的新节点总数。注意，cnti == 0 表示边不可细分。

要细分边 [ui, vi] ，需要将其替换为 (cnti + 1) 条新边，和 cnti 个新节点。新节点为 x1, x2, ..., xcnti ，新边为 [ui, x1], [x1, x2], [x2, x3], ..., [xcnti+1, xcnti], [xcnti, vi] 。

现在得到一个新的 细分图 ，请你计算从节点 0 出发，可以到达多少个节点？节点 是否可以到达的判断条件 为：如果节点间距离是 maxMoves 或更少，则视为可以到达；否则，不可到达。

给你原始图和 maxMoves ，返回新的细分图中从节点 0 出发 可到达的节点数 。

__示例 :__

示例 1：

![图](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/01/origfinal.png)

输入：edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
输出：13
解释：边的细分情况如上图所示。
可以到达的节点已经用黄色标注出来。

示例 2：

输入：edges = [[0,1,4],[1,2,6],[0,2,8],[1,3,1]], maxMoves = 10, n = 4
输出：23

示例 3：

输入：edges = [[1,2,4],[1,4,5],[1,3,1],[2,3,4],[3,4,5]], maxMoves = 17, n = 5
输出：1
解释：节点 0 与图的其余部分没有连通，所以只有节点 0 可以到达。

__提示:__

0 <= edges.length <= min(n * (n - 1) / 2, 10^4)
edges[i].length == 3
0 <= ui < vi < n
图中 不存在平行边
0 <= cnti <= 10^4
0 <= maxMoves <= 10^9
1 <= n <= 3000

__思路__:

Dijkstra算法 ➕ 小根堆
原图中的点记为大点, 加的新点记为小点
将插入的点看作边权
用 Dijkstra 先判断是否能到达大点
将当前结点加入小根堆
用一个哈希表记录各点到起点的距离, 更大的距离不需要记录, 剪枝
每次从小根堆中选择最小的, 就是离上一个点距离最近的, 记录下小点能达到的个数, 为了不重复计数, 需要记录 min(visited[(u, v)], visited[(v, u)]) 找两个大点出发最近的
时间复杂度为 O(elge + v), 空间复杂度为 O(v), e 为边数, v 为顶点数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int reachableNodes(vector<vector<int>>& edges, int maxMoves, int n) 
    {
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& e : edges)
        {
            graph[e[0]].emplace_back(make_pair(e[1], e[2]));
            graph[e[1]].emplace_back(make_pair(e[0], e[2]));
        }
        vector<int> dist(n, 0x3f3f3f3f), visited(n, 0);
        dist.front() = 0;
        priority_queue<pair<int, int>> pq;
        pq.push({ 0, 0 });
        while (!pq.empty())
        {
            auto t = pq.top();
            pq.pop();
            int cur = t.second, d = -t.first;
            if (visited[cur]) continue;
            visited[cur] = true;
            for (const auto& [nei, w] : graph[cur])
            {
                if (dist[nei] > d + 1 + w)
                {
                    dist[nei] = d + 1 + w;
                    pq.push({ -dist[nei], nei });
                }
            }
        }
        int result = 0;
        for (int i = 0; i < n; i++) if (dist[i] <= maxMoves) ++result;
        for (const auto& e : edges) result += min(e[2], max(0, maxMoves - dist[e[0]]) + max(0, maxMoves - dist[e[1]]));
        return result;
    }
};
```

__Java__:

```Java
class Node {
    int node, dist;
    Node(int n, int d) {
        node = n;
        dist = d;
    }
}
class Solution {
    public int reachableNodes(int[][] edges, int maxMoves, int n) {
        Map<Integer, Map<Integer, Integer>> graph = new HashMap();
        for (int[] edge: edges) {
            graph.computeIfAbsent(edge[0], x -> new HashMap()).put(edge[1], edge[2]);
            graph.computeIfAbsent(edge[1a], x -> new HashMap()).put(edge[0], edge[2]);
        }
        PriorityQueue<Node> pq = new PriorityQueue<Node>( (a, b) -> Integer.compare(a.dist, b.dist));
        pq.offer(new Node(0, 0));
        Map<Integer, Integer> dist = new HashMap(), visited = new HashMap();
        dist.put(0, 0);
        int result = 0;
        while (!pq.isEmpty()) {
            Node node = pq.poll();
            if (node.dist > dist.getOrDefault(node.node, 0)) continue;
            ++result;
            if (!graph.containsKey(node.node)) continue;
            for (int nei: graph.get(node.node).keySet()) {
                int v = Math.min(graph.get(node.node).get(nei), maxMoves - node.dist), d2 = node.dist + graph.get(node.node).get(nei) + 1;
                visited.put(n * node.node + nei, v);
                if (d2 < dist.getOrDefault(nei, maxMoves + 1)) {
                    pq.offer(new Node(nei, d2));
                    dist.put(nei, d2);
                }
            }
        }
        for (int[] edge: edges) result += Math.min(edge[2], visited.getOrDefault(edge[0] * n + edge[1], 0) + visited.getOrDefault(edge[1] * n + edge[0], 0) );
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def reachableNodes(self, edges: List[List[int]], maxMoves: int, n: int) -> int:
        result, graph, dist, visited, pq = 0, defaultdict(dict), { 0: 0 }, defaultdict(int), [(0, 0)]
        for u, v, w in edges:
            graph[u][v] = graph[v][u] = w
        while pq:
            d, node = heappop(pq)
            if d > dist[node]:
                continue
            result += 1
            for nei, w in graph[node].items():
                visited[(node, nei)] = min(w, maxMoves - d)
                if (d2 := d + w + 1) < dist.get(nei, maxMoves + 1):
                    heappush(pq, (d2, nei))
                    dist[nei] = d2
        for u, v, w in edges:
            result += min(w, visited[(u, v)] + visited[(v, u)])
        return result
```
