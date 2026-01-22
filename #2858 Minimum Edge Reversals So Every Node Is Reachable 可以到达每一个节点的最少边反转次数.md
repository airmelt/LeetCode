# 2858 Minimum Edge Reversals So Every Node Is Reachable 可以到达每一个节点的最少边反转次数

__Description:__

There is a __simple directed graph__ with `n` nodes labeled from `0` to `n - 1`. The graph would form a __tree__ if its edges were bi-directional.

You are given an integer `n` and a __2D__ integer array `edges`, where `edges[i] = [ui, vi]` represents a __directed edge__ going from node `ui` to node `vi`.

An __edge reversal__ changes the direction of an edge, i.e., a directed edge going from node `ui` to node `vi` becomes a directed edge going from node `vi` to node `ui`.

For every node `i` in the range `[0, n - 1]`, your task is to __independently__ calculate the __minimum__ number of __edge reversals__ required so it is possible to reach any other node starting from node `i` through a __sequence__ of __directed edges__.

Return _an integer array_ `answer`_, where_ `answer[i]` _is the_ ___minimum__ number of __edge reversals__ required so it is possible to reach any other node starting from node_ `i` _through a __sequence__ of __directed edges__._

__Example:__

Example 1:

![2858-1](https://assets.leetcode.com/uploads/2023/08/26/image-20230826221104-3.png)

```text
Input: n = 4, edges = [[2,0],[2,1],[1,3]]
Output: [1,1,0,2]
Explanation: The image above shows the graph formed by the edges.
For node 0: after reversing the edge [2,0], it is possible to reach any other node starting from node 0.
So, answer[0] = 1.
For node 1: after reversing the edge [2,1], it is possible to reach any other node starting from node 1.
So, answer[1] = 1.
For node 2: it is already possible to reach any other node starting from node 2.
So, answer[2] = 0.
For node 3: after reversing the edges [1,3] and [2,1], it is possible to reach any other node starting from node 3.
So, answer[3] = 2.
```

Example 2:

![2858-2](https://assets.leetcode.com/uploads/2023/08/26/image-20230826225541-2.png)

```text
Input: n = 3, edges = [[1,2],[2,0]]
Output: [2,0,1]
Explanation: The image above shows the graph formed by the edges.
For node 0: after reversing the edges [2,0] and [1,2], it is possible to reach any other node starting from node 0.
So, answer[0] = 2.
For node 1: it is already possible to reach any other node starting from node 1.
So, answer[1] = 0.
For node 2: after reversing the edge [1, 2], it is possible to reach any other node starting from node 2.
So, answer[2] = 1.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ui == edges[i][0] < n`
- `0 <= vi == edges[i][1] < n`
- `ui != vi`
- The input is generated such that if the edges were bi-directional, the graph would be a tree.

__题目描述:__

给你一个 `n` 个点的 __简单有向图__ （没有重复边的有向图），节点编号为 `0` 到 `n - 1` 。如果这些边是双向边，那么这个图形成一棵 __树__ 。

给你一个整数 `n` 和一个 __二维__ 整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示从节点 `ui` 到节点 `vi` 有一条 __有向边__ 。

__边反转__ 指的是将一条边的方向反转，也就是说一条从节点 `ui` 到节点 `vi` 的边会变为一条从节点 `vi` 到节点 `ui` 的边。

对于范围 `[0, n - 1]` 中的每一个节点 `i` ，你的任务是分别 __独立__ 计算 __最少__ 需要多少次 __边反转__ ，从节点 `i` 出发经过 __一系列有向边__ ，可以到达所有的节点。

请你返回一个长度为 `n` 的整数数组 `answer` ，其中 `answer[i]`表示从节点 `i` 出发，可以到达所有节点的 __最少边反转__ 次数。

__示例:__

示例 1：

![2858-3](https://assets.leetcode.com/uploads/2023/08/26/image-20230826221104-3.png)

```text
输入：n = 4, edges = [[2,0],[2,1],[1,3]]
输出：[1,1,0,2]
解释：上图表示了与输入对应的简单有向图。
对于节点 0 ：反转 [2,0] ，从节点 0 出发可以到达所有节点。
所以 answer[0] = 1 。
对于节点 1 ：反转 [2,1] ，从节点 1 出发可以到达所有节点。
所以 answer[1] = 1 。
对于节点 2 ：不需要反转就可以从节点 2 出发到达所有节点。
所以 answer[2] = 0 。
对于节点 3 ：反转 [1,3] 和 [2,1] ，从节点 3 出发可以到达所有节点。
所以 answer[3] = 2 。
```

示例 2：

![2858-4](https://assets.leetcode.com/uploads/2023/08/26/image-20230826225541-2.png)

```text
输入：n = 3, edges = [[1,2],[2,0]]
输出：[2,0,1]
解释：上图表示了与输入对应的简单有向图。
对于节点 0 ：反转 [2,0] 和 [1,2] ，从节点 0 出发可以到达所有节点。
所以 answer[0] = 2 。
对于节点 1 ：不需要反转就可以从节点 2 出发到达所有节点。
所以 answer[1] = 0 。
对于节点 2 ：反转 [1,2] ，从节点 2 出发可以到达所有节点。
所以 answer[2] = 1 。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ui == edges[i][0] < n`
- `0 <= vi == edges[i][1] < n`
- `ui != vi`
- 输入保证如果边是双向边，可以得到一棵树。

__思路:__

```text
换根 DP
将边数组转换为邻接表
将正边记录为 1, 反向边记录为 -1
先跑一遍 DFS 确定 0 需要反转的次数, 路过负边时就需要反转
然后换根将结果更新为路过的反向边之和
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> minEdgeReversals(int n, vector<vector<int>>& edges) 
    {
        unordered_map<int, vector<pair<int, int>>> graph;
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(make_pair(edge.back(), 1));
            graph[edge.back()].emplace_back(make_pair(edge.front(), -1));
        }
        vector<int> result(n);
        auto dfs = [&](this auto&& dfs, int u, int fa) -> void
        {
            for (const auto& edge : graph[u])
            {
                if (edge.first != fa)
                {
                    result.front() += edge.second < 0;
                    dfs(edge.first, u);
                }
            }
        };
        dfs(0, -1);
        auto reroot = [&](this auto&& reroot, int u, int fa) -> void
        {
            for (const auto& edge : graph[u])
            {
                if (edge.first != fa)
                {
                    result[edge.first] = result[u] + edge.second;
                    reroot(edge.first, u);
                }
            }
        };
        reroot(0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] minEdgeReversals(int n, int[][] edges) {
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) 
        {
            graph[edge[0]].add(new int[]{ edge[1], 1 });
            graph[edge[1]].add(new int[]{ edge[0], -1 });
        }
        var result = new int[n];
        dfs(0, -1, graph, result);
        reroot(0, -1, graph, result);
        return result;
    }

    private void dfs(int u, int fa, List<int[]>[] graph, int[] result) {
        for (var edge : graph[u]) {
            if (edge[0] != fa) {
                if (edge[1] < 0) ++result[0];
                dfs(edge[0], u, graph, result);
            }
        }
    }

    private void reroot(int u, int fa, List<int[]>[] graph, int[] result) {
        for (var edge : graph[u]) {
             if (edge[0] != fa) {
                result[edge[0]] = result[u] + edge[1];
                reroot(edge[0], u, graph, result);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def minEdgeReversals(self, n: int, edges: List[List[int]]) -> List[int]:
        graph, result = defaultdict(list), [0] * n
        for u, v in edges:
            graph[u].append((v, 1))
            graph[v].append((u, -1))
            
        def dfs(u: int, fa: int) -> None:
            for v, d in graph[u]:
                if v != fa:
                    result[0] += d < 0
                    dfs(v, u)
        dfs(0, -1)

        def reroot(u: int, fa: int) -> None:
            for v, d in graph[u]:
                if v != fa:
                    result[v] = result[u] + d  
                    reroot(v, u)
        reroot(0, -1)
        return result
```
