# 2925 Maximum Score After Applying Operations on a Tree 在树上执行操作以后得到的最大分数

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`, and rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

You are also given a __0-indexed__ integer array `values` of length `n`, where `values[i]` is the __value__ associated with the `i ^ th` node.

You start with a score of `0`. In one operation, you can:

- Pick any node `i`.
- Add `values[i]` to your score.
- Set `values[i]` to `0`.

A tree is healthy if the sum of values on the path from the root to any leaf node is different than zero.

Return the maximum score you can obtain after performing these operations on the tree any number of times so that it remains healthy.

__Example:__

Example 1:

![2925-1](https://assets.leetcode.com/uploads/2023/10/11/graph-13-1.png)

```text
Input: edges = [[0,1],[0,2],[0,3],[2,4],[4,5]], values = [5,2,5,2,1,1]
Output: 11
Explanation: We can choose nodes 1, 2, 3, 4, and 5. The value of the root is non-zero. Hence, the sum of values on the path from the root to any leaf is different than zero. Therefore, the tree is healthy and the score is values[1] + values[2] + values[3] + values[4] + values[5] = 11.
It can be shown that 11 is the maximum score obtainable after any number of operations on the tree.
```

Example 2:

![2925-2](https://assets.leetcode.com/uploads/2023/10/11/graph-14-2.png)

```text
Input: edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [20,10,9,7,4,3,5]
Output: 40
Explanation: We can choose nodes 0, 2, 3, and 4.
```

- The sum of values on the path from 0 to 4 is equal to 10.
- The sum of values on the path from 0 to 3 is equal to 10.
- The sum of values on the path from 0 to 5 is equal to 3.
- The sum of values on the path from 0 to 6 is equal to 5.

Therefore, the tree is healthy and the score is values[0] + values[2] + values[3] + values[4] = 40.

It can be shown that 40 is the maximum score obtainable after any number of operations on the tree.

__Constraints:__

- `2 <= n <= 2 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `values.length == n`
- `1 <= values[i] <= 10 ^ 9`
- The input is generated such that `edges` represents a valid tree.

__题目描述:__

有一棵 `n` 个节点的无向树，节点编号为 `0` 到 `n - 1` ，根节点编号为 `0` 。给你一个长度为 `n - 1` 的二维整数数组 `edges` 表示这棵树，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 有一条边。

同时给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `values` ，其中 `values[i]` 表示第 `i` 个节点的值。

一开始你的分数为 `0` ，每次操作中，你将执行:

- 选择节点 `i` 。
- 将 `values[i]` 加入你的分数。
- 将 `values[i]` 变为 `0` 。

如果从根节点出发，到任意叶子节点经过的路径上的节点值之和都不等于 0 ，那么我们称这棵树是 健康的 。

你可以对这棵树执行任意次操作，但要求执行完所有操作以后树是 健康的 ，请你返回你可以获得的 最大分数 。

__示例:__

示例 1：

![2925-3](https://assets.leetcode.com/uploads/2023/10/11/graph-13-1.png)

```text
输入：edges = [[0,1],[0,2],[0,3],[2,4],[4,5]], values = [5,2,5,2,1,1]
输出：11
解释：我们可以选择节点 1 ，2 ，3 ，4 和 5 。根节点的值是非 0 的。所以从根出发到任意叶子节点路径上节点值之和都不为 0 。所以树是健康的。你的得分之和为 values[1] + values[2] + values[3] + values[4] + values[5] = 11 。
11 是你对树执行任意次操作以后可以获得的最大得分之和。
```

示例 2：

![2925-4](https://assets.leetcode.com/uploads/2023/10/11/graph-14-2.png)

```text
输入：edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [20,10,9,7,4,3,5]
输出：40
解释：我们选择节点 0 ，2 ，3 和 4 。
```

- 从 0 到 4 的节点值之和为 10 。
- 从 0 到 3 的节点值之和为 10 。
- 从 0 到 5 的节点值之和为 3 。
- 从 0 到 6 的节点值之和为 5 。

所以树是健康的。你的得分之和为 values[0] + values[2] + values[3] + values[4] = 40 。

40 是你对树执行任意次操作以后可以获得的最大得分之和。

__提示：__

- `2 <= n <= 2 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `values.length == n`
- `1 <= values[i] <= 10 ^ 9`
- 输入保证 `edges` 构成一棵合法的树。

__思路:__

```text
动态规划
能得到的最大分数
也就是在健康状态下所有分数减去遍历过程中的最小分数
当遍历到叶子节点时, 以叶子节点为根的树必须要健康, 所以不能选, 此时直接返回该叶子节点的值
否则加上路径上的所有点返回路径上所有值和当前根节点的较小值
表示要么去掉根节点要么去掉所有非根节点
最后返回所有分数减去遍历的结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumScoreAfterOperations(vector<vector<int>>& edges, vector<int>& values) 
    {
        int n = values.size();
        vector<vector<int>> graph(n);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        graph.front().emplace_back(-1);
        auto dfs = [&](this auto&& dfs, int u, int fa) -> long long
        {
            if (graph[u].size() == 1) return values[u];
            long long cur = 0LL;
            for (const auto& v : graph[u]) if (v != fa) cur += dfs(v, u);
            return min(cur, (long long)values[u]);
        };
        return accumulate(values.begin(), values.end(), 0LL) - dfs(0, -1);
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private int n;
    private long[] values;

    public long maximumScoreAfterOperations(int[][] edges, int[] values) {
        n = values.length;
        this.values = Arrays.stream(values).mapToLong(v -> v).toArray();
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        graph[0].add(-1);
        return Arrays.stream(this.values).sum() - dfs(0, -1);
    }

    private long dfs(int u, int fa) {
        if (graph[u].size() == 1) return values[u];
        long cur = 0;
        for (int v : graph[u]) if (v != fa) cur += dfs(v, u);
        return Math.min(values[u], cur);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumScoreAfterOperations(self, edges: List[List[int]], values: List[int]) -> int:
        graph = defaultdict(set)
        graph[0].add(-1)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(u: int, fa: int) -> int:
            if len(graph[u]) == 1:
                return values[u]
            cur = 0 
            for v in graph[u]:
                if v != fa:
                    cur += dfs(v, u)  
            return min(values[u], cur) 
        return sum(values) - dfs(0, -1)
```
