# 2065 Maximum Path Quality of a Graph 最大化一张图中的路径价值

__Description:__

There is an __undirected__ graph with `n` nodes numbered from `0` to `n - 1` (__inclusive__). You are given a __0-indexed__ integer array `values` where `values[i]` is the __value__ of the `i ^ th` node. You are also given a __0-indexed__ 2D integer array `edges`, where each `edges[j] = [uj, vj, timej]` indicates that there is an undirected edge between the nodes `uj` and `vj`, and it takes `timej` seconds to travel between the two nodes. Finally, you are given an integer `maxTime`.

A __valid__ __path__ in the graph is any path that starts at node `0`, ends at node `0`, and takes __at most__ `maxTime` seconds to complete. You may visit the same node multiple times. The __quality__ of a valid path is the __sum__ of the values of the __unique nodes__ visited in the path (each node's value is added __at most once__ to the sum).

Return the maximum quality of a valid path.

Note: There are at most four edges connected to each node.

__Example:__

Example 1:

![2065-1](https://assets.leetcode.com/uploads/2021/10/19/ex1drawio.png)

```text
Input: values = [0,32,10,43], edges = [[0,1,10],[1,2,15],[0,3,10]], maxTime = 49
Output: 75
Explanation:
One possible path is 0 -> 1 -> 0 -> 3 -> 0. The total time taken is 10 + 10 + 10 + 10 = 40 <= 49.
The nodes visited are 0, 1, and 3, giving a maximal path quality of 0 + 32 + 43 = 75.
```

Example 2:

![2065-2](https://assets.leetcode.com/uploads/2021/10/19/ex2drawio.png)

```text
Input: values = [5,10,15,20], edges = [[0,1,10],[1,2,10],[0,3,10]], maxTime = 30
Output: 25
Explanation:
One possible path is 0 -> 3 -> 0. The total time taken is 10 + 10 = 20 <= 30.
The nodes visited are 0 and 3, giving a maximal path quality of 5 + 20 = 25.
```

Example 3:

![2065-3](https://assets.leetcode.com/uploads/2021/10/19/ex31drawio.png)

```text
Input: values = [1,2,3,4], edges = [[0,1,10],[1,2,11],[2,3,12],[1,3,13]], maxTime = 50
Output: 7
Explanation:
One possible path is 0 -> 1 -> 3 -> 1 -> 0. The total time taken is 10 + 13 + 13 + 10 = 46 <= 50.
The nodes visited are 0, 1, and 3, giving a maximal path quality of 1 + 2 + 4 = 7.
```

__Constraints:__

- `n == values.length`
- `1 <= n <= 1000`
- `0 <= values[i] <= 10 ^ 8`
- `0 <= edges.length <= 2000`
- `edges[j].length == 3`
- `0 <= uj < vj <= n - 1`
- `10 <= timej, maxTime <= 100`
- All the pairs `[uj, vj]` are __unique__.
- There are __at most four__ edges connected to each node.
- The graph may not be connected.

__题目描述:__

给你一张 __无向__ 图，图中有 `n` 个节点，节点编号从 `0` 到 `n - 1` （__都包括__）。同时给你一个下标从 __0__ 开始的整数数组 `values` ，其中 `values[i]` 是第 `i` 个节点的 __价值__ 。同时给你一个下标从 __0__ 开始的二维整数数组 `edges` ，其中 `edges[j] = [uj, vj, timej]` 表示节点 `uj` 和 `vj` 之间有一条需要 `timej` 秒才能通过的无向边。最后，给你一个整数 `maxTime` 。

__合法路径__ 指的是图中任意一条从节点 `0` 开始，最终回到节点 `0` ，且花费的总时间 __不超过__ `maxTime` 秒的一条路径。你可以访问一个节点任意次。一条合法路径的 _价值_ 定义为路径中 __不同节点__ 的价值 __之和__ （每个节点的价值 __至多__ 算入价值总和中一次）。

请你返回一条合法路径的 最大 价值。

注意：每个节点 至多 有 四条 边与之相连。

__示例:__

示例 1：

![2065-4](https://assets.leetcode.com/uploads/2021/10/19/ex1drawio.png)

```text
输入：values = [0,32,10,43], edges = [[0,1,10],[1,2,15],[0,3,10]], maxTime = 49
输出：75
解释：
一条可能的路径为：0 -> 1 -> 0 -> 3 -> 0 。总花费时间为 10 + 10 + 10 + 10 = 40 <= 49 。
访问过的节点为 0 ，1 和 3 ，最大路径价值为 0 + 32 + 43 = 75 。
```

示例 2：

![2065-5](https://assets.leetcode.com/uploads/2021/10/19/ex2drawio.png)

```text
输入：values = [5,10,15,20], edges = [[0,1,10],[1,2,10],[0,3,10]], maxTime = 30
输出：25
解释：
一条可能的路径为：0 -> 3 -> 0 。总花费时间为 10 + 10 = 20 <= 30 。
访问过的节点为 0 和 3 ，最大路径价值为 5 + 20 = 25 。
```

示例 3：

![2065-6](https://assets.leetcode.com/uploads/2021/10/19/ex31drawio.png)

```text
输入：values = [1,2,3,4], edges = [[0,1,10],[1,2,11],[2,3,12],[1,3,13]], maxTime = 50
输出：7
解释：
一条可能的路径为：0 -> 1 -> 3 -> 1 -> 0 。总花费时间为 10 + 13 + 13 + 10 = 46 <= 50 。
访问过的节点为 0 ，1 和 3 ，最大路径价值为 1 + 2 + 4 = 7 。
```

示例 4：

![2065-7](https://assets.leetcode.com/uploads/2021/10/21/ex4drawio.png)

```text
输入：values = [0,1,2], edges = [[1,2,10]], maxTime = 10
输出：0
解释：
唯一一条路径为 0 。总花费时间为 0 。
唯一访问过的节点为 0 ，最大路径价值为 0 。
```

__提示：__

- `n == values.length`
- `1 <= n <= 1000`
- `0 <= values[i] <= 10 ^ 8`
- `0 <= edges.length <= 2000`
- `edges[j].length == 3`
- `0 <= uj < vj <= n - 1`
- `10 <= timej, maxTime <= 100`
- `[uj, vj]` 所有节点对 __互不相同__ 。
- 每个节点 __至多有四条__ 边。
- 图可能不连通。

__思路:__

```text
回溯
用邻接表记录图的边和权值
先用 dijkstra 算法求出 0 到其他节点的最短距离
然后用回溯算法遍历所有合法路径
如果不能返回 0 节点，提前剪枝
用 visited 记录访问过的节点
如果当前节点已经访问过，不用加上节点的价值
否则加上节点的价值
最后返回最大路径价值
时间复杂度为 O(N + M + D ^ K), 空间复杂度为 O(N + M + K), 其中 N 为节点的数量, M 为边的数量即 edges 数组的长度, D 为点的最大度数, K 搜索的最大边数, 这里 K 最大为 maxTime / time <= 10
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximalPathQuality(vector<int>& values, vector<vector<int>>& edges, int maxTime) 
    {
        int n = values.size();
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& edge: edges) 
        {
            graph[edge[0]].emplace_back(edge[1], edge[2]);
            graph[edge[1]].emplace_back(edge[0], edge[2]);
        }
        vector<bool> visited(n);
        visited.front() = true;
        int result = 0;
        function<void(int, int, int)> dfs = [&](int u, int cur, int value) 
        {
            if (!u) result = max(result, value);
            for (const auto& [v, w]: graph[u]) 
            {
                if (cur + w <= maxTime) 
                {
                    if (!visited[v]) 
                    {
                        visited[v] = true;
                        dfs(v, cur + w, value + values[v]);
                        visited[v] = false;
                    }
                    else dfs(v, cur + w, value);
                }
            }
        };
        dfs(0, 0, values.front());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0;
    private int maxTime;
    private int[] values;
    private boolean[] visited;
    private Map<Integer, List<int[]>> graph = new HashMap<>();

    public int maximalPathQuality(int[] values, int[][] edges, int maxTime) {
        int n = values.length;
        this.values = values;
        this.maxTime = maxTime;
        visited = new boolean[n];
        visited[0] = true;
        for (int i = 0; i < n; i++) graph.put(i, new ArrayList<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(new int[]{edge[1], edge[2]});
            graph.get(edge[1]).add(new int[]{edge[0], edge[2]});
        }
        dfs(0, 0, values[0]);
        return result;
    }

    private void dfs(int u, int cur, int value) {
        if (u == 0) result = Math.max(result, value);
        for (int[] next : graph.get(u)) {
            if (cur + next[1] <= maxTime) {
                if (!visited[next[0]]) {
                    visited[next[0]] = true;
                    dfs(next[0], cur + next[1], value + values[next[0]]);
                    visited[next[0]] = false;
                } else dfs(next[0], cur + next[1], value);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maximalPathQuality(self, values: List[int], edges: List[List[int]], maxTime: int) -> int:
        graph, dis, heap, visited, result = defaultdict(dict), [0] + [inf] * ((n := len(values)) - 1), [(0, 0)], [True] + [False] * (n - 1), 0
        for u, v, w in edges:
            graph[u][v] = graph[v][u] = w
        while heap:
            u, w = heappop(heap)
            if dis[u] >= w:
                for v, t in graph[u].items():
                    if dis[u] + t < dis[v]:
                        dis[v] = dis[u] + t
                        heappush(heap, (v, dis[v]))
        def dfs(u: int, cur: int, value: int) -> NoReturn:
            if not u:
                nonlocal result
                result = max(result, value) 
            for v, w in graph[u].items():
                if cur + w + dis[v] <= maxTime:
                    if not visited[v]:
                        visited[v] = True
                        dfs(v, cur + w, value + values[v])
                        visited[v] = False
                    else:
                        dfs(v, cur + w, value)
        dfs(0, 0, values[0])
        return result
```
