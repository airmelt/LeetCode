# 1976 Number of Ways to Arrive at Destination 到达目的地的方案数

__Description:__

You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with __bi-directional__ roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where `roads[i] = [ui, vi, timei]` means that there is a road between intersections `ui` and `vi` that takes `timei` minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the __shortest amount of time__.

Return _the __number of ways__ you can arrive at your destination in the __shortest amount of time___. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1976-1](https://assets.leetcode.com/uploads/2021/07/17/graph2.png)

```text
Input: n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
Output: 4
Explanation: The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes.
The four ways to get there in 7 minutes are:
```

- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

Example 2:

```text
Input: n = 2, roads = [[1,0,10]]
Output: 1
Explanation: There is only one way to go from intersection 0 to intersection 1, and it takes 10 minutes.
```

__Constraints:__

- `1 <= n <= 200`
- `n - 1 <= roads.length <= n * (n - 1) / 2`
- `roads[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `1 <= timei <= 10 ^ 9`
- `ui != vi`
- There is at most one road connecting any two intersections.
- You can reach any intersection from any other intersection.

__题目描述:__

你在一个城市里，城市由 `n` 个路口组成，路口编号为 `0` 到 `n - 1` ，某些路口之间有 __双向__ 道路。输入保证你可以从任意路口出发到达其他任意路口，且任意两个路口之间最多有一条路。

给你一个整数 `n` 和二维整数数组 `roads` ，其中 `roads[i] = [ui, vi, timei]` 表示在路口 `ui` 和 `vi` 之间有一条需要花费 `timei` 时间才能通过的道路。你想知道花费 __最少时间__ 从路口 `0` 出发到达路口 `n - 1` 的方案数。

请返回花费 __最少时间__ 到达目的地的 __路径数目__ 。由于答案可能很大，将结果对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

![1976-2](https://assets.leetcode.com/uploads/2021/07/17/graph2.png)

```text
输入：n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
输出：4
解释：从路口 0 出发到路口 6 花费的最少时间是 7 分钟。
四条花费 7 分钟的路径分别为：
```

- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

示例 2：

```text
输入：n = 2, roads = [[1,0,10]]
输出：1
解释：只有一条从路口 0 到路口 1 的路，花费 10 分钟。
```

__提示：__

- `1 <= n <= 200`
- `n - 1 <= roads.length <= n * (n - 1) / 2`
- `roads[i].length == 3`
- `0 <= ui, vi <= n - 1`
- `1 <= timei <= 10 ^ 9`
- `ui != vi`
- 任意两个路口之间至多有一条路。
- 从任意路口出发，你能够到达其他任意路口。

__思路:__

```text
动态规划 ➕ Dijklstra
使用 Dijklstra 算法求出 0 到其他所有点的最短路径
这样计算得到 0 到 n - 1 的最短路径
接着将 0 到 n - 1 的在最短路径上的点加入图中
使用带记忆化的 DFS 求出 0 到 n - 1 的方案数
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int countPaths(int n, vector<vector<int>>& roads) {
        int mod = 1e9 + 7;
        vector<vector<long long>> dist(n, vector<long long>(n, LLONG_MAX >> 1));
        for (int i = 0; i < n; i++) dist[i][i] = 0;
        for (const auto& road : roads) dist[road[0]][road[1]] = dist[road[1]][road[0]] = road[2];
        vector<int> visited(n), dp(n, -1);
        for (int i = 0; i < n; i++) 
        {
            int u = -1;
            for (int v = 0; v < n; v++) if (!visited[v] and (u == -1 or dist[0][v] < dist[0][u])) u = v;
            visited[u] = true;
            for (int v = 0; v < n; v++) dist[0][v] = min(dist[0][v], dist[0][u] + dist[u][v]);
        }
        vector<vector<int>> graph(n);
        for (const auto& road : roads)
        {
            if (dist[0][road[1]] - dist[0][road[0]] == road[2]) graph[road[0]].emplace_back(road[1]);
            else if (dist[0][road[0]] - dist[0][road[1]] == road[2]) graph[road[1]].emplace_back(road[0]);
        }
        function<int(int)> dfs = [&](int u) {
            if (u == n - 1) return 1;
            if (dp[u] != -1) return dp[u];
            dp[u] = 0;
            for (const auto& v: graph[u]) 
            {
                dp[u] += dfs(v);
                if (dp[u] >= mod) dp[u] -= mod;
            }
            return dp[u];
        };
        return dfs(0);
    }
};
```

__Java__:

```Java
class Solution {
    static class Edge {
        int to;
        long weight;
        Edge(int to, long weight) {
            this.to = to;
            this.weight = weight;
        }
    }

    List<Edge>[] graph;
    final long INF = (Long.MAX_VALUE >> 1) - (long)1e5;
    final int MOD = (int)1e9 + 7;

    private int dijkstra(int ed) {
        PriorityQueue<Edge> pq = new PriorityQueue<>((a, b) -> Long.compare(a.weight, b.weight));
        boolean[] vis = new boolean[ed];
        long[] dist = new long[ed];
        int[] cur = new int[ed];
        pq.offer(new Edge(0, 0));
        Arrays.fill(dist, INF);
        dist[0] = 0;
        cur[0] = 1;
        while (!pq.isEmpty()) {
            var u = pq.poll().to;
            if (vis[u]) continue;
            vis[u] = true;
            for (var next : graph[u]) {
                int v = next.to;
                long w = next.weight;
                if (dist[v] > dist[u] + w) {
                    cur[v] = cur[u];
                    dist[v] = dist[u] + w;
                    pq.offer(new Edge(v, dist[v]));
                } else if (dist[v] == dist[u] + w) cur[v] = (cur[v] + cur[u]) % MOD;
            }
        }
        return cur[ed - 1];
    }

    public int countPaths(int n, int[][] roads) {
        graph = new ArrayList[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
        for (var road : roads) {
            int u = road[0], v = road[1], w = road[2];
            graph[u].add(new Edge(v, w));
            graph[v].add(new Edge(u, w));
        }
        return dijkstra(n);
    }
}
```

__Python__:

```Python
class Solution:
    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        MOD, dist, visited, graph = 10 ** 9 + 7, [[inf] * n for _ in range(n)], set(), defaultdict(list)
        for i in range(n):
            dist[i][i] = 0
        for x, y, z in roads:
            dist[x][y] = dist[y][x] = z
        for _ in range(n - 1):
            u = None
            for v in range(n):
                if v not in visited and (not u or dist[0][v] < dist[0][u]):
                    u = v
            visited.add(u)
            for v in range(n):
                dist[0][v] = min(dist[0][v], dist[0][u] + dist[u][v])
        for x, y, z in roads:
            if dist[0][y] - dist[0][x] == z:
                graph[x].append(y)
            elif dist[0][x] - dist[0][y] == z:
                graph[y].append(x)
            
        @lru_cache(None)
        def dfs(u: int) -> int:
            if u == n - 1:
                return 1
            result = 0
            for v in graph[u]:
                result += dfs(v)
            return result
        return dfs(0) % MOD
```
