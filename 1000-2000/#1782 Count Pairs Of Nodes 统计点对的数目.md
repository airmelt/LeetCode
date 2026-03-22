# 1782 Count Pairs Of Nodes 统计点对的数目

__Description:__

You are given an undirected graph defined by an integer `n`, the number of nodes, and a 2D integer array `edges`, the edges in the graph, where `edges[i] = [ui, vi]` indicates that there is an __undirected__ edge between `ui` and `vi`. You are also given an integer array `queries`.

Let `incident(a, b)` be defined as the __number of edges__ that are connected to __either__ node `a` or `b`.

The answer to the `j ^ th` query is the __number of pairs__ of nodes `(a, b)` that satisfy __both__ of the following conditions:

- `a < b`
- `incident(a, b) > queries[j]`

Return _an array_ `answers` _such that_ `answers.length == queries.length` _and_ `answers[j]` _is the answer of the_ `j ^ th` _query_.

Note that there can be multiple edges between the same two nodes.

__Example:__

Example 1:

![1782-1](https://assets.leetcode.com/uploads/2021/06/08/winword_2021-06-08_00-58-39.png)

```text
Input: n = 4, edges = [[1,2],[2,4],[1,3],[2,3],[2,1]], queries = [2,3]
Output: [6,5]
Explanation: The calculations for incident(a, b) are shown in the table above.
The answers for each of the queries are as follows:
- answers[0] = 6. All the pairs have an incident(a, b) value greater than 2.
- answers[1] = 5. All the pairs except (3, 4) have an incident(a, b) value greater than 3.
```

Example 2:

```text
Input: n = 5, edges = [[1,5],[1,5],[3,4],[2,5],[1,3],[5,1],[2,3],[2,5]], queries = [1,2,3,4,5]
Output: [10,10,9,8,6]
```

__Constraints:__

- `2 <= n <= 2 * 10 ^ 4`
- `1 <= edges.length <= 10 ^ 5`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= queries.length <= 20`
- `0 <= queries[j] < edges.length`

__题目描述:__

给你一个无向图，无向图由整数 `n` ，表示图中节点的数目，和 `edges` 组成，其中 `edges[i] = [ui, vi]` 表示 `ui` 和 `vi` 之间有一条无向边。同时给你一个代表查询的整数数组 `queries` 。

第 `j` 个查询的答案是满足如下条件的点对 `(a, b)` 的数目:

- `a < b`
- `cnt` 是与 `a` __或者__ `b` 相连的边的数目，且 `cnt` __严格大于__ `queries[j]` 。

请你返回一个数组 `answers` ，其中 `answers.length == queries.length` 且 `answers[j]` 是第 `j` 个查询的答案。

请注意，图中可能会有 多重边 。

__示例:__

示例 1：

![1782-2](https://pic.leetcode.cn/1692844033-Kvxjvr-image.png)

```text
输入：n = 4, edges = [[1,2],[2,4],[1,3],[2,3],[2,1]], queries = [2,3]
输出：[6,5]
解释：每个点对中，与至少一个点相连的边的数目如上图所示。
answers[0] = 6。所有的点对(a, b)中边数和都大于2，故有6个；
answers[1] = 5。所有的点对(a, b)中除了(3,4)边数等于3，其它点对边数和都大于3，故有5个。
```

示例 2：

```text
输入：n = 5, edges = [[1,5],[1,5],[3,4],[2,5],[1,3],[5,1],[2,3],[2,5]], queries = [1,2,3,4,5]
输出：[10,10,9,8,6]
```

__提示：__

- `2 <= n <= 2 * 10 ^ 4`
- `1 <= edges.length <= 10 ^ 5`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= queries.length <= 20`
- `0 <= queries[j] < edges.length`

__思路:__

```text
1. 排序 ➕ 双指针
统计每个点的度数, 然后排序, 用双指针统计满足条件的点对数目
如果 degree[left] + degree[right] <= q, 则 ++left, 因为这时所有 [left, right] 区间内的点对都不满足条件
否则, result += right-- - left, 因为这时所有 [left, right - 1] 区间内的点对都满足条件
这里将会重复计算点对, 比如 [1, 2] 这条边在计算 (1, 2) 点对的时候会被计算两次, 因此需要减去重复计算的点对数目
用哈希表记录所有点对出现的次数, 由于题目要求 a < b, 记录的时候小的点放前面
如果 degree[a] + degree[b] > q && degree[a] + degree[b] <= q + count[a, b], 则 --result
时间复杂度为 O(NlogN + Q(N + M)), 空间复杂度为 O(N + M), 其中 M 为 edges 数组的长度, Q 为 queries 数组的长度
2. 后缀和
统计每个点的度数
统计每个度数出现的次数
利用后缀和统计每条边的出现次数
对于边
如果 d1 == d2, 则 result += c1 * (c1 - 1) / 2
如果 d1 < d2, 则 result += c1 * c2
即贡献
时间复杂度为 O(N + M + Q), 空间复杂度为 O(N + M), 其中 M 为 edges 数组的长度, Q 为 queries 数组的长度
```

__代码:__

__C++__:

```C++
class Solution {
public:
    vector<int> countPairs(int n, vector<vector<int>> &edges, vector<int> &queries) {
        vector<int> degree(n + 1), total;
        unordered_map<int, int> edge_count, degree_count;
        for (const auto& edge: edges) 
        {
            int x = min(edge.front(), edge.back()), y = max(edge.front(), edge.back());
            ++degree[x];
            ++degree[y];
            ++edge_count[x << 16 | y];
        }
        for (int i = 1; i <= n; i++) ++degree_count[degree[i]];
        int max_degree = *max_element(degree.begin() + 1, degree.end()), k = (max_degree << 1) + 2;
        total.resize(k);
        for (const auto& [d1, c1] : degree_count) for (const auto& [d2, c2] : degree_count) total[d1 + d2] += c1 * (c1 - 1) / 2 * (d1 == d2) + c1 * c2 * (d1 < d2);
        for (const auto& [k, v]: edge_count) 
        {
            --total[degree[k >> 16] + degree[k & 0xffff]];
            ++total[degree[k >> 16] + degree[k & 0xffff] - v];
        }
        for (int i = k - 1; i > 0; i--) total[i - 1] += total[i];
        for (int &q : queries) q = total[min(q + 1, k - 1)];
        return queries;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countPairs(int n, int[][] edges, int[] queries) {
        int degree[] = new int[n + 1], m = queries.length, result[] = new int[m];
        Map<Integer, Integer> map = new HashMap<>();
        for (int[] edge : edges) {
            int x = Math.min(edge[0], edge[1]), y = Math.max(edge[0], edge[1]);
            ++degree[x];
            ++degree[y];
            map.merge(x << 16 | y, 1, Integer::sum);
        }
        int[] sortedDegree = degree.clone();
        Arrays.sort(sortedDegree);
        for (int i = 0; i < m; i++) {
            int q = queries[i], left = 1, right = n;
            while (left < right) {
                if (sortedDegree[left] + sortedDegree[right] <= q) ++left;
                else result[i] += right-- - left;
            }
            for (var e : map.entrySet()) if (degree[e.getKey() >> 16] + degree[e.getKey() & 0xffff] > q && degree[e.getKey() >> 16] + degree[e.getKey() & 0xffff] <= q + e.getValue()) --result[i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairs(self, n: int, edges: List[List[int]], queries: List[int]) -> List[int]:
        degree, result, c = [0] * (n + 1), [0] * (m := len(queries)), Counter(tuple(sorted(e)) for e in edges)
        for x, y in edges:
            degree[x] += 1
            degree[y] += 1
        sorted_degree = sorted(degree)
        for i, q in enumerate(queries):
            left, right = 1, n
            while left < right:
                if sorted_degree[left] + sorted_degree[right] <= q:
                    left += 1
                else:
                    result[i] += right - left
                    right -= 1
            for (x, y), v in c.items():
                if 0 < degree[x] + degree[y] - q <= v:
                    result[i] -= 1
        return result
```
