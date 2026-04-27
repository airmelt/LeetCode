# 2242 Maximum Score of a Node Sequence 节点序列的最大得分

__Description:__

There is an __undirected__ graph with `n` nodes, numbered from `0` to `n - 1`.

You are given a __0-indexed__ integer array `scores` of length `n` where `scores[i]` denotes the score of node `i`. You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an __undirected__ edge connecting nodes `ai` and `bi`.

A node sequence is valid if it meets the following conditions:

- There is an edge connecting every pair of __adjacent__ nodes in the sequence.
- No node appears more than once in the sequence.

The score of a node sequence is defined as the sum of the scores of the nodes in the sequence.

Return _the __maximum score__ of a valid node sequence with a length of_ `4`_._ If no such sequence exists, return `-1`.

__Example:__

Example 1:

![2242-1](https://assets.leetcode.com/uploads/2022/04/15/ex1new3.png)

```text
Input: scores = [5,2,9,8,4], edges = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
Output: 24
Explanation: The figure above shows the graph and the chosen node sequence [0,1,2,3].
The score of the node sequence is 5 + 2 + 9 + 8 = 24.
It can be shown that no other node sequence has a score of more than 24.
Note that the sequences [3,1,2,0] and [1,0,2,3] are also valid and have a score of 24.
The sequence [0,3,2,4] is not valid since no edge connects nodes 0 and 3.
```

Example 2:

![2242-2](https://assets.leetcode.com/uploads/2022/03/17/ex2.png)

```text
Input: scores = [9,20,6,4,11,12], edges = [[0,3],[5,3],[2,4],[1,3]]
Output: -1
Explanation: The figure above shows the graph.
There are no valid node sequences of length 4, so we return -1.
```

__Constraints:__

- `n == scores.length`
- `4 <= n <= 5 * 10 ^ 4`
- `1 <= scores[i] <= 10 ^ 8`
- `0 <= edges.length <= 5 * 10 ^ 4`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- There are no duplicate edges.

__题目描述:__

给你一个 `n` 个节点的 __无向图__ ，节点编号为 `0` 到 `n - 1` 。

给你一个下标从 __0__ 开始的整数数组 `scores` ，其中 `scores[i]` 是第 `i` 个节点的分数。同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` ，表示节点 `ai` 和 `bi` 之间有一条 __无向__ 边。

一个合法的节点序列如果满足以下条件，我们称它是 合法的 ：

- 序列中每 _相邻_ 节点之间有边相连。
- 序列中没有节点出现超过一次。

节点序列的分数定义为序列中节点分数之 和 。

请你返回一个长度为 `4` 的合法节点序列的最大分数。如果不存在这样的序列，请你返回 `-1` 。

__示例:__

示例 1：

![2242-3](https://assets.leetcode.com/uploads/2022/04/15/ex1new3.png)

```text
输入：scores = [5,2,9,8,4], edges = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
输出：24
解释：上图为输入的图，节点序列为 [0,1,2,3] 。
节点序列的分数为 5 + 2 + 9 + 8 = 24 。
观察可知，没有其他节点序列得分和超过 24 。
注意节点序列 [3,1,2,0] 和 [1,0,2,3] 也是合法的，且分数为 24 。
序列 [0,3,2,4] 不是合法的，因为没有边连接节点 0 和 3 。
```

示例 2：

![2242-4](https://assets.leetcode.com/uploads/2022/03/17/ex2.png)

```text
输入：scores = [9,20,6,4,11,12], edges = [[0,3],[5,3],[2,4],[1,3]]
输出：-1
解释：上图为输入的图。
没有长度为 4 的合法序列，所以我们返回 -1 。
```

__提示：__

- `n == scores.length`
- `4 <= n <= 5 * 10 ^ 4`
- `1 <= scores[i] <= 10 ^ 8`
- `0 <= edges.length <= 5 * 10 ^ 4`
- `edges[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- 不会有重边。

__思路:__

```text
模拟
枚举所有的边, 对于每一条边, 枚举其两端点的最大三个分数
比如 (x, y), 对于 x 的最大三个分数为 a, b, c, 对于 y 的最大三个分数为 d, e, f
需要保证 x, a, b, y 两两不相等
时间复杂度为 O(MlogN), 空间复杂度为 O(M), 其中 M 为边的数量, N 为节点的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumScore(vector<int>& scores, vector<vector<int>>& edges) 
    {
        int n = scores.size(), result = -1;
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& e : edges)
        {
            int u = e.front(), v = e.back();
            graph[u].emplace_back(-scores[v], v);
            graph[v].emplace_back(-scores[u], u);
        }
        for (int i = 0; i < n; i++) 
        {
            if (graph[i].size() > 3)
            {
                nth_element(graph[i].begin(), graph[i].begin() + 3, graph[i].end());
                graph[i].resize(3);
            }
        }
        for (const auto& e : edges)
        {
            int x = e.front(), y = e.back();
            for (const auto& [score_a, a] : graph[x]) for (const auto& [score_b, b] : graph[y]) if (a != y and b != x and a != b) result = max(result, -score_a + scores[x] + scores[y] - score_b);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumScore(int[] scores, int[][] edges) {
        int n = scores.length, result = -1;
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            graph[u].add(new int[]{scores[v], v});
            graph[v].add(new int[]{scores[u], u});
        }
        for (int i = 0; i < n; i++) {
            if (graph[i].size() > 3) {
                graph[i].sort((a, b) -> (b[0] - a[0]));
                graph[i] = new ArrayList<>(graph[i].subList(0, 3));
            }
        }
        for (var e : edges) {
            int x = e[0], y = e[1];
            for (var p : graph[x]) {
                int scoreA = p[0], a = p[1];
                for (var q : graph[y]) {
                    int scoreB = q[0], b = q[1];
                    if (a != y && b != x && a != b) result = Math.max(result, scoreA + scores[x] + scores[y] + scoreB);
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
    def maximumScore(self, scores: List[int], edges: List[List[int]]) -> int:
        graph = [[] for _ in range(len(scores))]
        for u, v in edges:
            graph[u].append((scores[v], v))
            graph[v].append((scores[u], u))
        for i, nei in enumerate(graph):
            if len(nei) > 3:
                graph[i] = nlargest(3, nei)
        return max([score_a + scores[x] + scores[y] + score_b for x, y in edges for score_a, a in graph[x] for score_b, b in graph[y] if y != a != b != x], default=-1)
```
