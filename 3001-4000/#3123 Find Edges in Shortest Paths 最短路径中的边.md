# 3123 Find Edges in Shortest Paths 最短路径中的边

__Description:__

You are given an undirected weighted graph of `n` nodes numbered from 0 to `n - 1`. The graph consists of `m` edges represented by a 2D array `edges`, where `edges[i] = [ai, bi, wi]` indicates that there is an edge between nodes `ai` and `bi` with weight `wi`.

Consider all the shortest paths from node 0 to node `n - 1` in the graph. You need to find a __boolean__ array `answer` where `answer[i]` is `true` if the edge `edges[i]` is part of __at least__ one shortest path. Otherwise, `answer[i]` is `false`.

Return the array `answer`.

Note that the graph may not be connected.

__Example:__

Example 1:

![3123-1](https://assets.leetcode.com/uploads/2024/03/05/graph35drawio-1.png)

```text
Input: n = 6, edges = [[0,1,4],[0,2,1],[1,3,2],[1,4,3],[1,5,1],[2,3,1],[3,5,3],[4,5,2]]

Output: [true,true,true,false,true,true,true,false]

Explanation:

The following are all the shortest paths between nodes 0 and 5:
```

- The path `0 -> 1 -> 5`: The sum of weights is `4 + 1 = 5`.
- The path `0 -> 2 -> 3 -> 5`: The sum of weights is `1 + 1 + 3 = 5`.
- The path `0 -> 2 -> 3 -> 1 -> 5`: The sum of weights is `1 + 1 + 2 + 1 = 5`.

Example 2:

![3123-2](https://assets.leetcode.com/uploads/2024/03/05/graphhhh.png)

```text
Input: n = 4, edges = [[2,0,1],[0,1,1],[0,3,4],[3,2,2]]

Output: [true,false,false,true]

Explanation:
```

There is one shortest path between nodes 0 and 3, which is the path `0 -> 2 -> 3` with the sum of weights `1 + 2 = 3`.

__Constraints:__

- `2 <= n <= 5 * 10 ^ 4`
- `m == edges.length`
- `1 <= m <= min(5 * 10 ^ 4, n * (n - 1) / 2)`
- `0 <= ai, bi < n`
- `ai != bi`
- `1 <= wi <= 10 ^ 5`
- There are no repeated edges.

__题目描述:__

给你一个 `n` 个节点的无向带权图，节点编号为 `0` 到 `n - 1` 。图中总共有 `m` 条边，用二维数组 `edges` 表示，其中 `edges[i] = [ai, bi, wi]` 表示节点 `ai` 和 `bi` 之间有一条边权为 `wi` 的边。

对于节点 `0` 为出发点，节点 `n - 1` 为结束点的所有最短路，你需要返回一个长度为 `m` 的 __boolean__ 数组 `answer` ，如果 `edges[i]` __至少__ 在其中一条最短路上，那么 `answer[i]` 为 `true` ，否则 `answer[i]` 为 `false` 。

请你返回数组 `answer` 。

注意，图可能不连通。

__示例:__

示例 1：

![3123-3](https://assets.leetcode.com/uploads/2024/03/05/graph35drawio-1.png)

```text
输入：n = 6, edges = [[0,1,4],[0,2,1],[1,3,2],[1,4,3],[1,5,1],[2,3,1],[3,5,3],[4,5,2]]

输出：[true,true,true,false,true,true,true,false]

解释：

以下为节点 0 出发到达节点 5 的 所有 最短路：
```

- 路径 `0 -> 1 -> 5` :边权和为 `4 + 1 = 5` 。
- 路径 `0 -> 2 -> 3 -> 5` :边权和为 `1 + 1 + 3 = 5` 。
- 路径 `0 -> 2 -> 3 -> 1 -> 5` :边权和为 `1 + 1 + 2 + 1 = 5` 。

示例 2：

![3123-4](https://assets.leetcode.com/uploads/2024/03/05/graphhhh.png)

```text
输入：n = 4, edges = [[2,0,1],[0,1,1],[0,3,4],[3,2,2]]

输出：[true,false,false,true]

解释：
```

只有一条从节点 0 出发到达节点 3 的最短路 `0 -> 2 -> 3` ，边权和为 `1 + 2 = 3` 。

__提示：__

- `2 <= n <= 5 * 10 ^ 4`
- `m == edges.length`
- `1 <= m <= min(5 * 10 ^ 4, n * (n - 1) / 2)`
- `0 <= ai, bi < n`
- `ai != bi`
- `1 <= wi <= 10 ^ 5`
- 图中没有重边。

__思路:__

```text
Dijkstra
使用堆优化的 Dijkstra 计算 0 到其他点的最短距离
然后从 n - 1 开始往 0 遍历
如果当前的距离满足
就把对应边设置为 true
时间复杂度为 O(N + MlogM), 空间复杂度为 O(N + M)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<bool> findAnswer(int n, vector<vector<int>>& edges) 
    {
        vector<vector<tuple<int, int, int>>> graph(n);
        for (int i = 0, m = edges.size(); i < m; i++)
        {
            graph[edges[i][0]].emplace_back(edges[i][1], edges[i][2], i);
            graph[edges[i][1]].emplace_back(edges[i][0], edges[i][2], i);
        }
        vector<long long> dis(n, LLONG_MAX);
        dis.front() = 0;
        priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<>> pq;
        pq.emplace(0, 0);
        while (!pq.empty()) 
        {
            auto [du, u] = pq.top();
            pq.pop();
            if (du <= dis[u]) 
            {
                for (auto [v, w, _] : graph[u]) 
                {
                    if (du + w < dis[v])
                    {
                        dis[v] = du + w;
                        pq.emplace(du + w, v);
                    }
                }
            }
        }
        vector<bool> result(edges.size()), visited(n);
        if (dis[n - 1] == LLONG_MAX) return result;
        auto dfs = [&](this auto&& dfs, int u) -> void
        {
            visited[u] = true;
            for (auto [v, w, i] : graph[u]) 
            {
                if (dis[v] + w == dis[u]) 
                {
                    result[i] = true;
                    if (!visited[v]) dfs(v);
                }
            }
        };
        dfs(n - 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean[] findAnswer(int n, int[][] edges) {
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, i -> new ArrayList<>());
        for (int i = 0, m = edges.length; i < m; i++) {
            graph[edges[i][0]].add(new int[]{ edges[i][1], edges[i][2], i });
            graph[edges[i][1]].add(new int[]{ edges[i][0], edges[i][2], i });
        }
        long dis[] = new long[n], nxt = 0L;
        Arrays.fill(dis, Long.MAX_VALUE);
        dis[0] = 0;
        var pq = new PriorityQueue<long[]>((a, b) -> Long.compare(a[0], b[0]));
        pq.offer(new long[]{ 0, 0 });
        while (!pq.isEmpty()) {
            long cur[] = pq.poll(), du = cur[0], u = cur[1];
            if (du <= dis[(int)u]) {
                for (var t : graph[(int)u]) {
                    if ((nxt = du + t[1]) < dis[t[0]]) {
                        dis[t[0]] = nxt;
                        pq.offer(new long[]{ nxt, t[0] });
                    }
                }
            }
        }
        boolean result[] = new boolean[edges.length], visited[] = new boolean[n];
        if (dis[n - 1] == Long.MAX_VALUE) return result;
        dfs(n - 1, graph, dis, result, visited);
        return result;
    }

    private void dfs(int u, List<int[]>[] graph, long[] dis, boolean[] result, boolean[] visited) {
        visited[u] = true;
        for (var t : graph[u]) {
            if (dis[t[0]] + t[1] == dis[u]) {
                result[t[2]] = true;
                if (!visited[t[0]]) dfs(t[0], graph, dis, result, visited);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def findAnswer(self, n: int, edges: List[List[int]]) -> List[bool]:
        graph, dis, heap, result, visited = defaultdict(list), [0] + [inf] * (n - 1), [(0, 0)], [False] * len(edges), [False] * n
        for i, (u, v, w) in enumerate(edges):
            graph[u].append((v, w, i))
            graph[v].append((u, w, i))
        while heap:
            du, u = heappop(heap)
            if du <= dis[u]:
                for v, w, _ in graph[u]:
                    if (nxt := du + w) < dis[v]:
                        dis[v] = nxt
                        heappush(heap, (nxt, v))
        if dis[-1] == inf:
            return result

        def dfs(u: int) -> None:
            visited[u] = True
            for v, w, i in graph[u]:
                if dis[v] + w == dis[u]:
                    result[i] = True
                    if not visited[v]:
                        dfs(v)
        dfs(n - 1)
        return result
```
