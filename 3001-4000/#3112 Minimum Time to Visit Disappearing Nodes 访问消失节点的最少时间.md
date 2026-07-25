# 3112 Minimum Time to Visit Disappearing Nodes 访问消失节点的最少时间

__Description:__

There is an undirected graph of `n` nodes. You are given a 2D array `edges`, where `edges[i] = [ui, vi, length_i]` describes an edge between node `ui` and node `vi` with a traversal time of `length_i` units.

Additionally, you are given an array `disappear`, where `disappear[i]` denotes the time when the node `i` disappears from the graph and you won't be able to visit it.

Note that the graph might be disconnected and might contain multiple edges.

Return the array `answer`, with `answer[i]` denoting the __minimum__ units of time required to reach node `i` from node 0. If node `i` is __unreachable__ from node 0 then `answer[i]` is `-1`.

__Example:__

Example 1:

```text
Input: n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,1,5]

Output: [0,-1,4]

Explanation:
```

![3112-1](https://assets.leetcode.com/uploads/2024/08/11/output-onlinepngtools.png)

We are starting our journey from node 0, and our goal is to find the minimum time required to reach each node before it disappears.

- For node 0, we don't need any time as it is our starting point.
- For node 1, we need at least 2 units of time to traverse `edges[0]`. Unfortunately, it disappears at that moment, so we won't be able to visit it.
- For node 2, we need at least 4 units of time to traverse `edges[2]`.

Example 2:

```text
Input: n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,3,5]

Output: [0,2,3]

Explanation:
```

![3112-2](https://assets.leetcode.com/uploads/2024/08/11/output-onlinepngtools-1.png)

We are starting our journey from node 0, and our goal is to find the minimum time required to reach each node before it disappears.

- For node 0, we don't need any time as it is the starting point.
- For node 1, we need at least 2 units of time to traverse `edges[0]`.
- For node 2, we need at least 3 units of time to traverse `edges[0]` and `edges[1]`.

Example 3:

```text
Input: n = 2, edges = [[0,1,1]], disappear = [1,1]

Output: [0,-1]

Explanation:

Exactly when we reach node 1, it disappears.
```

__Constraints:__

- `1 <= n <= 5 * 10 ^ 4`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i] == [ui, vi, length_i]`
- `0 <= ui, vi <= n - 1`
- `1 <= length_i <= 10 ^ 5`
- `disappear.length == n`
- `1 <= disappear[i] <= 10 ^ 5`

__题目描述:__

给你一个二维数组 `edges` 表示一个 `n` 个点的无向图，其中 `edges[i] = [ui, vi, length_i]` 表示节点 `ui` 和节点 `vi` 之间有一条需要 `length_i` 单位时间通过的无向边。

同时给你一个数组 `disappear` ，其中 `disappear[i]` 表示节点 `i` 从图中消失的时间点，在那一刻及以后，你无法再访问这个节点。

注意，图有可能一开始是不连通的，两个节点之间也可能有多条边。

请你返回数组 `answer` ， `answer[i]` 表示从节点 `0` 到节点 `i` 需要的 __最少__ 单位时间。如果从节点 `0` 出发 __无法__ 到达节点 `i` ，那么 `answer[i]` 为 `-1` 。

__示例:__

示例 1：

![3112-3](https://assets.leetcode.com/uploads/2024/03/09/example1.png)

```text
输入：n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,1,5]
```

输出：[0,-1,4]

解释：

我们从节点 0 出发，目的是用最少的时间在其他节点消失之前到达它们。

- 对于节点 0 ，我们不需要任何时间，因为它就是我们的起点。
- 对于节点 1 ，我们需要至少 2 单位时间，通过 `edges[0]` 到达。但当我们到达的时候，它已经消失了，所以我们无法到达它。
- 对于节点 2 ，我们需要至少 4 单位时间，通过 `edges[2]` 到达。

示例 2：

![3112-4](https://assets.leetcode.com/uploads/2024/03/09/example2.png)

```text
输入：n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,3,5]
```

输出：[0,2,3]

解释：

我们从节点 0 出发，目的是用最少的时间在其他节点消失之前到达它们。

- 对于节点 0 ，我们不需要任何时间，因为它就是我们的起点。
- 对于节点 1 ，我们需要至少 2 单位时间，通过 `edges[0]` 到达。
- 对于节点 2 ，我们需要至少 3 单位时间，通过 `edges[0]` 和 `edges[1]` 到达。

示例 3：

```text
输入：n = 2, edges = [[0,1,1]], disappear = [1,1]
```

输出：[0,-1]

解释：

当我们到达节点 1 的时候，它恰好消失，所以我们无法到达节点 1 。

__提示：__

- `1 <= n <= 5 * 10 ^ 4`
- `0 <= edges.length <= 10 ^ 5`
- `edges[i] == [ui, vi, length_i]`
- `0 <= ui, vi <= n - 1`
- `1 <= length_i <= 10 ^ 5`
- `disappear.length == n`
- `1 <= disappear[i] <= 10 ^ 5`

__思路:__

```text
dijkstra
用一个优先队列记录当前的最短时间
如果可以更新, 即当前的队首的时间比结果时间更短
那么检查当前节点是否会消失
如果不会消失就进行松弛操作
时间复杂度为 O(N + MlogM), 空间复杂度为 O(N + M), 其中 M 为 edges 数组大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minimumTime(int n, vector<vector<int>>& edges, vector<int>& disappear) 
    {
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge[0]].emplace_back(edge[1], edge[2]);
            graph[edge[1]].emplace_back(edge[0], edge[2]);
        }
        vector<int> result(n, -1);
        result.front() = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.emplace(0, 0);
        while (!pq.empty())
        {
            auto cur = pq.top();
            pq.pop();
            if (cur.first <= result[cur.second])
            {
                for (const auto& edge : graph[cur.second])
                {
                    if (cur.first + edge.second < disappear[edge.first] and (result[edge.first] < 0 or result[edge.first] > cur.first + edge.second))
                    {
                        result[edge.first] = cur.first + edge.second;
                        pq.emplace(cur.first + edge.second, edge.first);
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] minimumTime(int n, int[][] edges, int[] disappear) {
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, i -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(new int[]{ edge[1], edge[2] });
            graph[edge[1]].add(new int[]{ edge[0], edge[2] });
        }
        int[] result = new int[n];
        Arrays.fill(result, -1);
        result[0] = 0;
        var pq = new PriorityQueue<int[]>((a, b) -> (a[0] - b[0]));
        pq.offer(new int[]{ 0, 0 });
        while (!pq.isEmpty()) {
            int cur[] = pq.poll(), nxt = 0;
            if (cur[0] <= result[cur[1]]) {
                for (var edge : graph[cur[1]]) {
                    if ((nxt = cur[0] + edge[1]) < disappear[edge[0]] && (result[edge[0]] < 0 || result[edge[0]] > nxt)) {
                        result[edge[0]] = nxt;
                        pq.offer(new int[]{ nxt, edge[0] });
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTime(self, n: int, edges: List[List[int]], disappear: List[int]) -> List[int]:
        graph, result, heap = defaultdict(list), [0] + [-1] * (n - 1), [(0, 0)]
        for u, v, w in edges:
            graph[u].append((v, w))
            graph[v].append((u, w))
        while heap:
            dx, x = heappop(heap)
            if dx <= result[x]:
                for v, w in graph[x]:
                    if (nxt_dis := w + dx) < disappear[v] and (result[v] < 0 or nxt_dis < result[v]):
                        result[v] = nxt_dis
                        heappush(heap, (nxt_dis, v))
        return result
```
