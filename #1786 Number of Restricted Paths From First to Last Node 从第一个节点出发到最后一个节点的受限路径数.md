# 1786 Number of Restricted Paths From First to Last Node 从第一个节点出发到最后一个节点的受限路径数

__Description:__

There is an undirected weighted connected graph. You are given a positive integer `n` which denotes that the graph has `n` nodes labeled from `1` to `n`, and an array `edges` where each `edges[i] = [ui, vi, weighti]` denotes that there is an edge between nodes `ui` and `vi` with weight equal to `weighti`.

A path from node `start` to node `end` is a sequence of nodes `[z0, z1, z2, ..., zk]` such that `z0 = start` and `zk = end` and there is an edge between `zi` and `zi+1` where `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let `distanceToLastNode(x)` denote the shortest distance of a path between node `n` and node `x`. A __restricted path__ is a path that also satisfies that `distanceToLastNode(zi) > distanceToLastNode(zi+1)` where `0 <= i <= k-1`.

Return _the number of restricted paths from node_ `1` _to node_ `n`. Since that number may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1786-1](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png)

```text
Input:  n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
Output:  3
Explanation:  Each circle contains the node number in black and its `distanceToLastNode value in blue.` The three restricted paths are:
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5
```

Example 2:

![1786-2](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png)

```text
Input:  n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
Output:  1
Explanation:  Each circle contains the node number in black and its `distanceToLastNode value in blue.` The only restricted path is 1 --> 3 --> 7.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 4`
- `n - 1 <= edges.length <= 4 * 10 ^ 4`
- `edges[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= weighti <= 10 ^ 5`
- There is at most one edge between any two nodes.
- There is at least one path between any two nodes.

__题目描述:__

现有一个加权无向连通图。给你一个正整数 `n` ，表示图中有 `n` 个节点，并按从 `1` 到 `n` 给节点编号；另给你一个数组 `edges` ，其中每个 `edges[i] = [ui, vi, weighti]` 表示存在一条位于节点 `ui` 和 `vi` 之间的边，这条边的权重为 `weighti` 。

从节点 `start` 出发到节点 `end` 的路径是一个形如 `[z0, z1, z2, ..., zk]` 的节点序列，满足 `z0 = start` 、 `zk = end` 且在所有符合 `0 <= i <= k-1` 的节点 `zi` 和 `zi+1` 之间存在一条边。

路径的距离定义为这条路径上所有边的权重总和。用 `distanceToLastNode(x)` 表示节点 `n` 和 `x` 之间路径的最短距离。__受限路径__ 为满足 `distanceToLastNode(zi) > distanceToLastNode(zi+1)` 的一条路径，其中 `0 <= i <= k-1` 。

返回从节点 `1` 出发到节点 `n` 的 __受限路径数__ 。由于数字可能很大，请返回对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

![1786-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex1.png)

```text
输入：n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
输出：3
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。三条受限路径分别是：
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5
```

示例 2：

![1786-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex22.png)

```text
输入：n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
输出：1
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。唯一一条受限路径是：1 --> 3 --> 7 。
```

__提示：__

- `1 <= n <= 2 * 10 ^ 4`
- `n - 1 <= edges.length <= 4 * 10 ^ 4`
- `edges[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= weighti <= 10 ^ 5`
- 任意两个节点之间至多存在一条边
- 任意两个节点之间至少存在一条路径

__思路:__

```text
Dijkstra ➕ 动态规划
题意是从 1 出发找到所有路径使得路径上的点到点 n 的最短路径依次递减
将点 n 到点 n 的距离视为 0
反向查询各点到点 n 的最短路径依次递增的点直到点 1
使用堆优化的 Dijkstra 算法得到每个点到点 n 的最短距离 dist 数组
设 dp[i] 表示从 1 出发到 i 的路径的数量
dp[i] = dp[i] + dp[j], if dist[i] > dist[j]
将下标按照 dist 对应的下标大小进行排序
时间复杂度为 O(MlogN), 空间复杂度为 O(N + M), 其中 M 为 edges 数组的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countRestrictedPaths(int n, vector<vector<int>>& edges) 
    {
        unordered_map<int, unordered_map<int, int>> graph;
        for (const auto& edge : edges)
        {
            int u = edge[0], v = edge[1], w = edge[2];
            graph[u - 1][v - 1] = w;
            graph[v - 1][u - 1] = w;
        }
        vector<int> dist(n, INT_MAX), dp(n), arr(n);
        int cur = 0, MOD = 1e9 + 7;
        iota(arr.begin(), arr.end(), 0);
        dist.back() = 0;
        dp.back() = 1;
        priority_queue<pair<int, int>> heap;
        heap.push({0, n - 1});
        while (!heap.empty()) 
        {
            int d = -heap.top().first, x = heap.top().second;
            heap.pop();
            if (d > dist[x]) continue;
            for (const auto& [y, w] : graph[x])
            {
                if ((cur = dist[x] + w) < dist[y]) 
                {
                    dist[y] = cur;
                    heap.push({-cur, y});
                }
            }
        }
        sort(arr.begin(), arr.end(), [&](auto a, auto b) { return dist[a] < dist[b]; });
        for (int i = 0; i < n; i++) 
        {
            for (const auto& [y, w] : graph[arr[i]]) if (dist[arr[i]] > dist[y]) dp[arr[i]] = (dp[arr[i]] + dp[y]) % MOD;
            if (!arr[i]) break;
        }
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int countRestrictedPaths(int n, int[][] edges) {
        Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
        int dist[] = new int[n], d = 0, x = 0, cur = 0, arr[][] = new int[n][2], dp[] = new int[n], MOD = 1_000_000_007;
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            graph.putIfAbsent(u - 1, new HashMap<>());
            graph.putIfAbsent(v - 1, new HashMap<>());
            graph.get(u - 1).put(v - 1, w);
            graph.get(v - 1).put(u - 1, w);
        }
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[n - 1] = 0;
        dp[n - 1] = 1;
        Queue<int[]> heap = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        heap.offer(new int[]{0, n - 1});
        while (!heap.isEmpty()) {
            int[] edge = heap.poll();
            if ((d = edge[0]) > dist[x = edge[1]]) continue;
            for (int y : graph.get(x).keySet()) {
                if ((cur = dist[x] + graph.get(x).get(y)) < dist[y]) {
                    dist[y] = cur;
                    heap.offer(new int[]{cur, y});
                }
            }
        }
        for (int i = 0; i < n; i++) arr[i] = new int[]{i, dist[i]};
        Arrays.sort(arr, (a, b) -> a[1] - b[1]);
        for (int i = 0; i < n; i++) {
            if (!graph.containsKey(x = arr[i][0])) continue;
            for (int y : graph.get(x).keySet()) if (arr[i][1] > dist[y]) dp[x] = (dp[x] + dp[y]) % MOD;
            if (x == 0) break;
        }
        return dp[0];
    }
}
```

__Python__:

```Python

class Solution:
    def countRestrictedPaths(self, n: int, edges: List[List[int]]) -> int:
        graph, MOD = defaultdict(list), 10 ** 9 + 7
        for u, v, w in edges:
            graph[u - 1].append((v - 1, w))
            graph[v - 1].append((u - 1, w))
        dist, heap = [inf] * (n - 1) + [0], [(0, n - 1)] 
        while heap:
            d, x = heappop(heap)
            if d > dist[x]:
                continue
            for y, w in graph[x]:
                if (cur := dist[x] + w) < dist[y]:
                    dist[y] = cur
                    heappush(heap, (cur, y))

        @lru_cache(None)
        def dfs(x: int) -> int:
            if not x:
                return 1
            else:
                result = 0
                for y, w in graph[x]:
                    if dist[x] < dist[y]:
                        result = (result + dfs(y)) % MOD
                return result
        return dfs(n - 1)
```
