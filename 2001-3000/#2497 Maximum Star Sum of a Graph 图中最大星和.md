# 2497 Maximum Star Sum of a Graph 图中最大星和

__Description:__

There is an undirected graph consisting of `n` nodes numbered from `0` to `n - 1`. You are given a __0-indexed__ integer array `vals` of length `n` where `vals[i]` denotes the value of the `i ^ th` node.

You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an __undirected__ edge connecting nodes `ai` and `bi.`

A __star graph__ is a subgraph of the given graph having a center node containing `0` or more neighbors. In other words, it is a subset of edges of the given graph such that there exists a common node for all edges.

The image below shows star graphs with `3` and `4` neighbors respectively, centered at the blue node.

![2497-1](https://assets.leetcode.com/uploads/2022/11/07/max-star-sum-descdrawio.png)

The star sum is the sum of the values of all the nodes present in the star graph.

Given an integer `k`, return _the __maximum star sum__ of a star graph containing __at most___ `k` _edges._

__Example:__

Example 1:

![2497-2](https://assets.leetcode.com/uploads/2022/11/07/max-star-sum-example1drawio.png)

```text
Input: vals = [1,2,3,4,10,-10,-20], edges = [[0,1],[1,2],[1,3],[3,4],[3,5],[3,6]], k = 2
Output: 16
Explanation: The above diagram represents the input graph.
The star graph with the maximum star sum is denoted by blue. It is centered at 3 and includes its neighbors 1 and 4.
It can be shown it is not possible to get a star graph with a sum greater than 16.
```

Example 2:

```text
Input: vals = [-5], edges = [], k = 0
Output: -5
Explanation: There is only one possible star graph, which is node 0 itself.
Hence, we return -5.
```

__Constraints:__

- `n == vals.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 4 <= vals[i] <= 10 ^ 4`
- `0 <= edges.length <= min(n * (n - 1) / 2` `, 10 ^ 5)`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- `0 <= k <= n - 1`

__题目描述:__

给你一个 `n` 个点的无向图，节点从 `0` 到 `n - 1` 编号。给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `vals` ，其中 `vals[i]` 表示第 `i` 个节点的值。

同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 和 `bi` 之间有一条双向边。

__星图__ 是给定图中的一个子图，它包含一个中心节点和 `0` 个或更多个邻居。换言之，星图是给定图中一个边的子集，且这些边都有一个公共节点。

下图分别展示了有 `3` 个和 `4` 个邻居的星图，蓝色节点为中心节点。

![2497-3](https://assets.leetcode.com/uploads/2022/11/07/max-star-sum-descdrawio.png)

星和 定义为星图中所有节点值的和。

给你一个整数 `k` ，请你返回 __至多__ 包含 `k` 条边的星图中的 __最大星和__ 。

__示例:__

示例 1：

![2497-4](https://assets.leetcode.com/uploads/2022/11/07/max-star-sum-example1drawio.png)

输入：vals = [1,2,3,4,10,-10,-20], edges = [[0,1],[1,2],[1,3],[3,4],[3,5],[3,6]], k = 2
输出：16
解释：上图展示了输入示例。
最大星和对应的星图在上图中用蓝色标出。中心节点是 3 ，星图中还包含邻居 1 和 4 。
无法得到一个和大于 16 且边数不超过 2 的星图。

示例 2：

```text
输入：vals = [-5], edges = [], k = 0
输出：-5
解释：只有一个星图，就是节点 0 自己。
所以我们返回 -5 。
```

__提示：__

- `n == vals.length`
- `1 <= n <= 10 ^ 5`
- `-10 ^ 4 <= vals[i] <= 10 ^ 4`
- `0 <= edges.length <= min(n * (n - 1) / 2` `, 10 ^ 5)`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- `0 <= k <= n - 1`

__思路:__

```text
排序
使用优先队列或者排序
如果大于 0 则记录节点的邻居节点
遍历所有节点, 找到邻居节点的和的最大值
如果超过 k 则取前 k 个
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), 其中 M 为边数, N 为节点数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxStarSum(vector<int>& vals, vector<vector<int>>& edges, int k) 
    {
        int result = *max_element(vals.begin(), vals.end()), n = vals.size();
        vector<priority_queue<int>> graph(n);
        for (const auto& edge : edges)
        {
            if (vals[edge.front()] > 0) graph[edge.back()].push(vals[edge.front()]);
            if (vals[edge.back()] > 0) graph[edge.front()].push(vals[edge.back()]);
        }
        for (int i = 0; i < n; i++)
        {
            int cur = vals[i];
            for (int j = 0; j < k and !graph[i].empty(); j++)
            {
                if (graph[i].top() <= 0) break;
                cur += graph[i].top();
                graph[i].pop();
            }
            result = max(result, cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxStarSum(int[] vals, int[][] edges, int k) {
        var map = new TreeMap<Integer, List<Integer>>();
        for (var edge : edges) {
            if (vals[edge[1]] > 0) map.computeIfAbsent(edge[0], v -> new ArrayList<>()).add(vals[edge[1]]);
            if (vals[edge[0]] > 0) map.computeIfAbsent(edge[1], v -> new ArrayList<>()).add(vals[edge[0]]);
        }
        int result = Integer.MIN_VALUE, n = vals.length;
        for (int i = 0; i < n; i++) result = Math.max(result, vals[i] + map.getOrDefault(i, new ArrayList<>()).stream().sorted((a, b) -> b - a).toList().stream().limit(k).mapToInt(Integer::intValue).sum());
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxStarSum(self, vals: List[int], edges: List[List[int]], k: int) -> int:
        graph = [[] for _ in vals]
        for x, y in edges:
            if vals[y] > 0: 
                graph[x].append(vals[y])
            if vals[x] > 0: 
                graph[y].append(vals[x])
        return max(v + sum(nlargest(k, a)) for v, a in zip(vals, graph))
```
