# 2872 Maximum Number of K-Divisible Components 可以被 K 整除连通块的最大数目

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

You are also given a __0-indexed__ integer array `values` of length `n`, where `values[i]` is the __value__ associated with the `i ^ th` node, and an integer `k`.

A __valid split__ of the tree is obtained by removing any set of edges, possibly empty, from the tree such that the resulting components all have values that are divisible by `k`, where the __value of a connected component__ is the sum of the values of its nodes.

Return the maximum number of components in any valid split.

__Example:__

Example 1:

![2872-1](https://assets.leetcode.com/uploads/2023/08/07/example12-cropped2svg.jpg)

```text
Input: n = 5, edges = [[0,2],[1,2],[1,3],[2,4]], values = [1,8,1,4,4], k = 6
Output: 2
Explanation: We remove the edge connecting node 1 with 2. The resulting split is valid because:
```

- The value of the component containing nodes 1 and 3 is values[1] + values[3] = 12.
- The value of the component containing nodes 0, 2, and 4 is values[0] + values[2] + values[4] = 6.

It can be shown that no other valid split has more than 2 connected components.

Example 2:

![2872-2](https://assets.leetcode.com/uploads/2023/08/07/example21svg-1.jpg)

```text
Input: n = 7, edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [3,0,6,1,5,2,1], k = 3
Output: 3
Explanation: We remove the edge connecting node 0 with 2, and the edge connecting node 0 with 1. The resulting split is valid because:
```

- The value of the component containing node 0 is values[0] = 3.
- The value of the component containing nodes 2, 5, and 6 is values[2] + values[5] + values[6] = 9.
- The value of the component containing nodes 1, 3, and 4 is values[1] + values[3] + values[4] = 6.

It can be shown that no other valid split has more than 3 connected components.

__Constraints:__

- `1 <= n <= 3 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `values.length == n`
- `0 <= values[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`
- Sum of `values` is divisible by `k`.
- The input is generated such that `edges` represents a valid tree.

__题目描述:__

给你一棵 `n` 个节点的无向树，节点编号为 `0` 到 `n - 1` 。给你整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 有一条边。

同时给你一个下标从 __0__ 开始长度为 `n` 的整数数组 `values` ，其中 `values[i]` 是第 `i` 个节点的 __值__ 。再给你一个整数 `k` 。

你可以从树中删除一些边，也可以一条边也不删，得到若干连通块。一个 __连通块的值__ 定义为连通块中所有节点值之和。如果所有连通块的值都可以被 `k` 整除，那么我们说这是一个 __合法分割__ 。

请你返回所有合法分割中，连通块数目的最大值 。

__示例:__

示例 1：

![2872-3](https://assets.leetcode.com/uploads/2023/08/07/example12-cropped2svg.jpg)

```text
输入：n = 5, edges = [[0,2],[1,2],[1,3],[2,4]], values = [1,8,1,4,4], k = 6
输出：2
解释：我们删除节点 1 和 2 之间的边。这是一个合法分割，因为：
```

- 节点 1 和 3 所在连通块的值为 values[1] + values[3] = 12 。
- 节点 0 ，2 和 4 所在连通块的值为 values[0] + values[2] + values[4] = 6 。

最多可以得到 2 个连通块的合法分割。

示例 2：

![2872-4](https://assets.leetcode.com/uploads/2023/08/07/example21svg-1.jpg)

```text
输入：n = 7, edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[2,6]], values = [3,0,6,1,5,2,1], k = 3
输出：3
解释：我们删除节点 0 和 2 ，以及节点 0 和 1 之间的边。这是一个合法分割，因为：
```

- 节点 0 的连通块的值为 values[0] = 3 。
- 节点 2 ，5 和 6 所在连通块的值为 values[2] + values[5] + values[6] = 9 。
- 节点 1 ，3 和 4 的连通块的值为 values[1] + values[3] + values[4] = 6 。

最多可以得到 3 个连通块的合法分割。

__提示：__

- `1 <= n <= 3 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `values.length == n`
- `0 <= values[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`
- `values` 之和可以被 `k` 整除。
- 输入保证 `edges` 是一棵无向树。

__思路:__

```text
DFS
注意到 values 的和是能整除 k 的
用一个数组记录所有的子树, 包括自己的值的和
遍历到当前节点先继续向下遍历
然后加上当前节点的子树的和
如果当前的子树和对 k 取模为 0
说明当前子树可以被单独拿出来作为一个连通块
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int maxKDivisibleComponents(int n, vector<vector<int>>& edges, vector<int>& values, int k) {
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        int result = 0;

        auto dfs = [&](this auto&& dfs, int u, int fa) -> long long
        {
            long long cur = values[u];
            for (const auto& v : graph[u]) if (v != fa) cur += dfs(v, u);
            result += !(cur % k);
            return cur;
        };
        dfs(0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private int result;
    private int[] values;
    private int k;

    public int maxKDivisibleComponents(int n, int[][] edges, int[] values, int k) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        this.values = values;
        this.k = k;
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        dfs(0, -1);
        return result;
    }

    private long dfs(int u, int fa) {
        long cur = values[u];
        for (int v : graph[u]) if (v != fa) cur += dfs(v, u);
        if (cur % k == 0L) ++result;
        return cur;
    }
}
```

__Python__:

```Python
class Solution:
    def maxKDivisibleComponents(self, n: int, edges: List[List[int]], values: List[int], k: int) -> int:
        graph, result = defaultdict(set), 0
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        
        def dfs(u: int, fa: int) -> int:
            nonlocal result
            cur = values[u]
            for v in graph[u]:
                if v != fa:
                    cur += dfs(v, u)
            result += not (cur % k)
            return cur
        dfs(0, -1)
        return result
```
