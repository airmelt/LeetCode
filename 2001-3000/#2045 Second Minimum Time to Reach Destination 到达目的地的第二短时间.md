# 2045 Second Minimum Time to Reach Destination 到达目的地的第二短时间

__Description:__

A city is represented as a __bi-directional connected__ graph with `n` vertices where each vertex is labeled from `1` to `n` (__inclusive__). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by __at most one__ edge, and no vertex has an edge to itself. The time taken to traverse any edge is `time` minutes.

Each vertex has a traffic signal which changes its color from __green__ to __red__ and vice versa every `change` minutes. All signals change __at the same time__. You can enter a vertex at __any time__, but can leave a vertex __only when the signal is green__. You __cannot wait__ at a vertex if the signal is __green__.

The second minimum value is defined as the smallest value strictly larger than the minimum value.

- For example the second minimum value of `[2, 3, 4]` is `3`, and the second minimum value of `[2, 2, 4]` is `4`.

Given `n`, `edges`, `time`, and `change`, return _the __second minimum time__ it will take to go from vertex_ `1` _to vertex_ `n`.

Notes:

- You can go through any vertex __any__ number of times, __including__ `1` and `n`.
- You can assume that when the journey __starts__, all signals have just turned __green__.

__Example:__

Example 1:

![2045-1](https://assets.leetcode.com/uploads/2021/09/29/e1.png)

![2045-2](https://assets.leetcode.com/uploads/2021/09/29/e2.png)

```text
Input: n = 5, edges = [[1,2],[1,3],[1,4],[3,4],[4,5]], time = 3, change = 5
Output: 13
Explanation:
```

The figure on the left shows the given graph.
The blue path in the figure on the right is the minimum time path.
The time taken is:

- Start at 1, time elapsed=0
- 1 -> 4: 3 minutes, time elapsed=3
- 4 -> 5: 3 minutes, time elapsed=6

Hence the minimum time needed is 6 minutes.

The red path shows the path to get the second minimum time.

- Start at 1, time elapsed=0
- 1 -> 3: 3 minutes, time elapsed=3
- 3 -> 4: 3 minutes, time elapsed=6
- Wait at 4 for 4 minutes, time elapsed=10
- 4 -> 5: 3 minutes, time elapsed=13

Hence the second minimum time is 13 minutes.

Example 2:

![2045-3](https://assets.leetcode.com/uploads/2021/09/29/eg2.png)

```text
Input: n = 2, edges = [[1,2]], time = 3, change = 2
Output: 11
Explanation:
The minimum time path is 1 -> 2 with time = 3 minutes.
The second minimum time path is 1 -> 2 -> 1 -> 2 with time = 11 minutes.
```

__Constraints:__

- `2 <= n <= 10 ^ 4`
- `n - 1 <= edges.length <= min(2 * 10 ^ 4, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- There are no duplicate edges.
- Each vertex can be reached directly or indirectly from every other vertex.
- `1 <= time, change <= 10 ^ 3`

__题目描述:__

城市用一个 __双向连通__ 图表示，图中有 `n` 个节点，从 `1` 到 `n` 编号（包含 `1` 和 `n`）。图中的边用一个二维整数数组 `edges` 表示，其中每个 `edges[i] = [ui, vi]` 表示一条节点 `ui` 和节点 `vi` 之间的双向连通边。每组节点对由 __最多一条__ 边连通，顶点不存在连接到自身的边。穿过任意一条边的时间是 `time` 分钟。

每个节点都有一个交通信号灯，每 `change` 分钟改变一次，从绿色变成红色，再由红色变成绿色，循环往复。所有信号灯都 __同时__ 改变。你可以在 __任何时候__ 进入某个节点，但是 __只能__ 在节点 __信号灯是绿色时__ 才能离开。如果信号灯是 __绿色__ ，你 __不能__ 在节点等待，必须离开。

第二小的值 是 严格大于 最小值的所有值中最小的值。

- 例如， `[2, 3, 4]` 中第二小的值是 `3` ，而 `[2, 2, 4]` 中第二小的值是 `4` 。

给你 `n`、 `edges`、 `time` 和 `change` ，返回从节点 `1` 到节点 `n` 需要的 __第二短时间__ 。

注意：

- 你可以 __任意次__ 穿过任意顶点，__包括__ `1` 和 `n` 。
- 你可以假设在 __启程时__ ，所有信号灯刚刚变成 __绿色__ 。

__示例:__

示例 1：

![2045-4](https://assets.leetcode.com/uploads/2021/09/29/e1.png)

![2045-5](https://assets.leetcode.com/uploads/2021/09/29/e2.png)

```text
输入：n = 5, edges = [[1,2],[1,3],[1,4],[3,4],[4,5]], time = 3, change = 5
输出：13
解释：
```

上面的左图展现了给出的城市交通图。
右图中的蓝色路径是最短时间路径。
花费的时间是：

- 从节点 1 开始，总花费时间=0
- 1 -> 4：3 分钟，总花费时间=3
- 4 -> 5：3 分钟，总花费时间=6

因此需要的最小时间是 6 分钟。

右图中的红色路径是第二短时间路径。

- 从节点 1 开始，总花费时间=0
- 1 -> 3：3 分钟，总花费时间=3
- 3 -> 4：3 分钟，总花费时间=6
- 在节点 4 等待 4 分钟，总花费时间=10
- 4 -> 5：3 分钟，总花费时间=13

因此第二短时间是 13 分钟。

示例 2：

![2045-6](https://assets.leetcode.com/uploads/2021/09/29/eg2.png)

```text
输入：n = 2, edges = [[1,2]], time = 3, change = 2
输出：11
解释：
最短时间路径是 1 -> 2 ，总花费时间 = 3 分钟
第二短时间路径是 1 -> 2 -> 1 -> 2 ，总花费时间 = 11 分钟
```

__提示：__

- `2 <= n <= 10 ^ 4`
- `n - 1 <= edges.length <= min(2 * 10 ^ 4, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- 不含重复边
- 每个节点都可以从其他节点直接或者间接到达
- `1 <= time, change <= 10 ^ 3`

__思路:__

```text
DFS
注意到时间只和路径长度有关, 因此可以用 DFS 求最短路径
将所有路径的权重都设置为 1, 从 1 开始 DFS
记录队列的时候优先记录最短的路径
在求最短路径的过程中, 记录每个节点的最短时间和次短时间
每个节点最多经过 2 次
最后根据次短时间计算总时间
每次到达一个新的节点, 检查是否需要等待, 如果需要等待, 计算等待时间
如果 cur_time % (change << 1) >= change, 需要等待 (change << 1) - cur_time % (change << 1) 的时间
时间复杂度为 O(N + E), 空间复杂度为 O(N + E), 其中 E 为边数, 即 edges 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int secondMinimum(int n, vector<vector<int>>& edges, int time, int change) 
    {
        vector<vector<int>> graph(n + 1);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        int INF = 0x3f3f3f3f, result = 0;
        vector<int> dist1(n + 1, INF), dist2(n + 1, INF);
        dist1.front() = 0;
        queue<pair<int, int>> q;
        q.push(make_pair(1, 0));
        while (dist2.back() == INF) 
        {
            auto cur = q.front();
            q.pop();
            for (const auto& v : graph[cur.first])
            {
                if (cur.second + 1 < dist2[v]) 
                {
                    q.push(make_pair(v, cur.second + 1));
                    if (cur.second + 1 <= dist1[v]) dist1[v] = cur.second + 1;
                    else dist2[v] = cur.second + 1;
                }
            }
        }
        for (int i = 0; i < dist2.back(); i++) 
        {
            if ((result % (change << 1)) >= change) result += (change << 1) - (result % (change << 1));
            result += time;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int secondMinimum(int n, int[][] edges, int time, int change) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n + 1; i++) graph.add(new ArrayList<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        int dist1[] = new int[n + 1], dist2[] = new int[n + 1], result = 0, INF = 0x3f3f3f3f;
        Arrays.fill(dist1, INF);
        Arrays.fill(dist2, INF);
        dist1[0] = 0;
        Queue<int[]> queue = new ArrayDeque<>();
        queue.offer(new int[]{1, 0});
        while (dist2[n] == INF) {
            int[] cur = queue.poll();
            for (int v : graph.get(cur[0])) {
                if (cur[1] + 1 < dist2[v]) {
                    queue.offer(new int[]{v, cur[1] + 1});
                    if (cur[1] + 1 <= dist1[v]) dist1[v] = cur[1] + 1;
                    else dist2[v] = cur[1] + 1;
                }
            }
        }
        for (int i = 0; i < dist2[n]; i++) {
            if ((result % (change << 1)) >= change) result += (change << 1) - (result % (change << 1));
            result += time;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def secondMinimum(self, n: int, edges: List[List[int]], time: int, change: int) -> int:
        graph, dist1, dist2, q, result = defaultdict(set), [0] + [inf] * n, [inf] * (n + 1), deque([(1, 0)]), 0
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        while dist2[-1] == inf:
            u, dis = q.popleft()
            for v in graph[u]:
                if (d := dis + 1) < dist2[v]:
                    q.append((v, d))
                    if d <= dist1[v]:
                        dist1[v] = d
                    else:
                        dist2[v] = d
        for _ in range(dist2[-1]):
            if result % (change << 1) >= change:
                result += (change << 1) - result % (change << 1)
            result += time
        return result

```
