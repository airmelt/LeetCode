# 1761 Minimum Degree of a Connected Trio in a Graph 一个图中连通三元组的最小度数

__Description:__

You are given an undirected graph. You are given an integer `n` which is the number of nodes in the graph and an array `edges`, where each `edges[i] = [ui, vi]` indicates that there is an undirected edge between `ui` and `vi`.

A connected trio is a set of three nodes where there is an edge between every pair of them.

The degree of a connected trio is the number of edges where one endpoint is in the trio, and the other is not.

Return _the __minimum__ degree of a connected trio in the graph, or_ `-1` _if the graph has no connected trios._

__Example:__

Example 1:

![1761-1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/14/trios1.png)

```text
Input: n = 6, edges = [[1,2],[1,3],[3,2],[4,1],[5,2],[3,6]]
Output: 3
Explanation: There is exactly one trio, which is [1,2,3]. The edges that form its degree are bolded in the figure above.
```

Example 2:

![1761-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/14/trios2.png)

```text
Input: n = 7, edges = [[1,3],[4,1],[4,3],[2,5],[5,6],[6,7],[7,5],[2,6]]
Output: 0
Explanation: There are exactly three trios:
1) [1,4,3] with degree 0.
2) [2,5,6] with degree 2.
3) [5,6,7] with degree 2.
```

__Constraints:__

- `2 <= n <= 400`
- `edges[i].length == 2`
- `1 <= edges.length <= n * (n-1) / 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- There are no repeated edges.

__题目描述:__

给你一个无向图，整数 `n` 表示图中节点的数目， `edges` 数组表示图中的边，其中 `edges[i] = [ui, vi]` ，表示 `ui` 和 `vi` 之间有一条无向边。

一个 连通三元组 指的是 三个 节点组成的集合且这三个点之间 两两 有边。

连通三元组的度数 是所有满足此条件的边的数目：一个顶点在这个三元组内，而另一个顶点不在这个三元组内。

请你返回所有连通三元组中度数的 __最小值__ ，如果图中没有连通三元组，那么返回 `-1` 。

__示例:__

示例 1：

![1761-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/14/trios1.png)

```text
输入：n = 6, edges = [[1,2],[1,3],[3,2],[4,1],[5,2],[3,6]]
输出：3
解释：只有一个三元组 [1,2,3] 。构成度数的边在上图中已被加粗。
```

示例 2：

![1761-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/02/14/trios2.png)

```text
输入：n = 7, edges = [[1,3],[4,1],[4,3],[2,5],[5,6],[6,7],[7,5],[2,6]]
输出：0
解释：有 3 个三元组：
1) [1,4,3]，度数为 0 。
2) [2,5,6]，度数为 2 。
3) [5,6,7]，度数为 2 。
```

__提示：__

- `2 <= n <= 400`
- `edges[i].length == 2`
- `1 <= edges.length <= n * (n-1) / 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- 图中没有重复的边。

__思路:__

```text
1. 暴力法
注意到 N 的范围为 [0, 400], 可以使用 N ^ 3 级别的算法
使用邻接矩阵记录所有相邻的结点
记录每个结点的度
遍历所有结点对, 只要是三元组就统计三元组的度的和与 6 的差
并记录最小值
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 2)
2. 有向图
给无向图添加方向
先统计每个结点的度
这里使用较小的度的结点向较大的度的结点
如果度相同, 标号小的指向标号大的
枚举每个结点及其邻接结点
如果是三元组就更新结果
时间复杂度为 O(M ^ 3 / 2), 空间复杂度为 O(M + N), 其中 M 为 edges 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minTrioDegree(int n, vector<vector<int>>& edges) 
    {
        vector<unordered_set<int>> degree(n);
        for (const auto& edge : edges)
        {
            degree[edge.front() - 1].insert(edge.back() - 1);
            degree[edge.back() - 1].insert(edge.front() - 1);
        }
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            int u = edge.front() - 1, v = edge.back() - 1;
            if (degree[u].size() < degree[v].size() or (degree[u].size() == degree[v].size() and u < v)) graph[u].emplace_back(v);
            else graph[v].emplace_back(u);
        }
        int result = INT_MAX;
        for (int u = 0; u < n; u++) for (const auto& v : graph[u]) for (const auto& w : graph[v]) if (degree[u].count(w)) result = min(result, (int)(degree[u].size() + degree[v].size() + degree[w].size() - 6));
        return result == INT_MAX ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minTrioDegree(int n, int[][] edges) {
        int result = Integer.MAX_VALUE, degree[] = new int[n], graph[][] = new int[n][n];
        for (int[] edge : edges) {
            graph[edge[0] - 1][edge[1] - 1] = graph[edge[1] - 1][edge[0] - 1] = 1;
            ++degree[edge[0] - 1];
            ++degree[edge[1] - 1];
        }
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (graph[i][j] == 1) for (int k = j + 1; k < n; k++) if (graph[i][k] == 1 && graph[j][k] == 1) result = Math.min(result, degree[i] + degree[j] + degree[k] - 6);
        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def minTrioDegree(self, n: int, edges: List[List[int]]) -> int:
        graph, degree, result = [[False] * n for _ in range(n)], [0] * n, float('inf')
        for u, v in edges:
            graph[u - 1][v - 1] = graph[v - 1][u - 1] = True
            degree[u - 1] += 1
            degree[v - 1] += 1
        for i in range(n):
            for j in range(i + 1, n):
                if graph[i][j]:
                    result = min(result, min((degree[i] + degree[j] + degree[k] - 6 for k in range(j + 1, n) if graph[i][k] and graph[j][k]), default=float('inf')))
        return result if result < float('inf') else -1
```
