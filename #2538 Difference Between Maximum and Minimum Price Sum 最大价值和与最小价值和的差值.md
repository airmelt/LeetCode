# 2538 Difference Between Maximum and Minimum Price Sum 最大价值和与最小价值和的差值

__Description:__

There exists an undirected and initially unrooted tree with `n` nodes indexed from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Each node has an associated price. You are given an integer array `price`, where `price[i]` is the price of the `i ^ th` node.

The price sum of a given path is the sum of the prices of all nodes lying on that path.

The tree can be rooted at any node `root` of your choice. The incurred __cost__ after choosing `root` is the difference between the maximum and minimum __price sum__ amongst all paths starting at `root`.

Return the maximum possible cost amongst all possible root choices.

__Example:__

Example 1:

![2538-1](https://assets.leetcode.com/uploads/2022/12/01/example14.png)

```text
Input: n = 6, edges = [[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]
Output: 24
Explanation: The diagram above denotes the tree after rooting it at node 2. The first part (colored in red) shows the path with the maximum price sum. The second part (colored in blue) shows the path with the minimum price sum.
```

- The first path contains nodes [2,1,3,4]: the prices are [7,8,6,10], and the sum of the prices is 31.
- The second path contains the node [2] with the price [7].

The difference between the maximum and minimum price sum is 24. It can be proved that 24 is the maximum cost.

Example 2:

![2538-2](https://assets.leetcode.com/uploads/2022/11/24/p1_example2.png)

```text
Input: n = 3, edges = [[0,1],[1,2]], price = [1,1,1]
Output: 2
Explanation: The diagram above denotes the tree after rooting it at node 0. The first part (colored in red) shows the path with the maximum price sum. The second part (colored in blue) shows the path with the minimum price sum.
```

- The first path contains nodes [0,1,2]: the prices are [1,1,1], and the sum of the prices is 3.
- The second path contains node [0] with a price [1].
The difference between the maximum and minimum price sum is 2. It can be proved that 2 is the maximum cost.

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `0 <= ai, bi <= n - 1`
- `edges` represents a valid tree.
- `price.length == n`
- `1 <= price[i] <= 10 ^ 5`

__题目描述:__

给你一个 `n` 个节点的无向无根图，节点编号为 `0` 到 `n - 1` 。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。

每个节点都有一个价值。给你一个整数数组 `price` ，其中 `price[i]` 是第 `i` 个节点的价值。

一条路径的 价值和 是这条路径上所有节点的价值之和。

你可以选择树中任意一个节点作为根节点 `root` 。选择 `root` 为根的 __开销__ 是以 `root` 为起点的所有路径中，__价值和__ 最大的一条路径与最小的一条路径的差值。

请你返回所有节点作为根节点的选择中，最大 的 开销 为多少。

__示例:__

示例 1：

![2538-3](https://assets.leetcode.com/uploads/2022/12/01/example14.png)

```text
输入：n = 6, edges = [[0,1],[1,2],[1,3],[3,4],[3,5]], price = [9,8,7,6,10,5]
输出：24
解释：上图展示了以节点 2 为根的树。左图（红色的节点）是最大价值和路径，右图（蓝色的节点）是最小价值和路径。
```

- 第一条路径节点为 [2,1,3,4]：价值为 [7,8,6,10] ，价值和为 31 。
- 第二条路径节点为 [2] ，价值为 [7] 。

最大路径和与最小路径和的差值为 24 。24 是所有方案中的最大开销。

示例 2：

![2538-4](https://assets.leetcode.com/uploads/2022/11/24/p1_example2.png)

```text
输入：n = 3, edges = [[0,1],[1,2]], price = [1,1,1]
输出：2
解释：上图展示了以节点 0 为根的树。左图（红色的节点）是最大价值和路径，右图（蓝色的节点）是最小价值和路径。
```

- 第一条路径包含节点 [0,1,2]：价值为 [1,1,1] ，价值和为 3 。
- 第二条路径节点为 [0] ，价值为 [1] 。

最大路径和与最小路径和的差值为 2 。2 是所有方案中的最大开销。

__提示：__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `0 <= ai, bi <= n - 1`
- `edges` 表示一棵符合题面要求的树。
- `price.length == n`
- `1 <= price[i] <= 10 ^ 5`

__思路:__

```text
树形 DP
由于最小的路径和即为节点本身
所以我们只需要计算最大路径和再减去节点本身即可
我们可以用 DFS 遍历树
记录下以节点 u 出发的带叶子节点的最大路径和和不带叶子节点的最大路径和
当节点 v 返回的时候
结果为 u 带叶子的最大路径和加上 v 的不带叶子节点的最大路径和以及 u 不带叶子节点的最大路径和加上 v 的带叶子节点的最大路径和两者的最大值, 这里是因为最后结果返回的时候要去掉一个叶子节点
然后用节点 v 的带叶子节点的最大路径和和不带叶子节点的最大路径和来更新节点 u 的带叶子节点的最大路径和和不带叶子节点的最大路径和
u 的带叶子节点的最大路径和为 u 的带叶子节点的最大路径和和 v 的带叶子节点的最大路径和加上 u 的价格的最大值
u 的不带叶子节点的最大路径和为 u 的不带叶子节点的最大路径和和 v 的不带叶子节点的最大路径和加上 u 的价格的最大值
最后返回节点 u 的带叶子节点的最大路径和和不带叶子节点的最大路径和用于给 u 的父节点计算
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxOutput(int n, vector<vector<int>>& edges, vector<int>& price) 
    {
        long long result = 0LL;
        unordered_map<int, vector<int>> graph;
        for (const auto& edge : edges)
        {
            int u = edge.front(), v = edge.back();
            graph[u].emplace_back(v);
            graph[v].emplace_back(u);
        }
        auto dfs = [&](this auto&& dfs, int u, int fa) -> vector<long long>
        {
            long long max_without_leaf = 0LL, max_with_leaf = price[u], price_u = price[u];
            for (const auto& v : graph[u])
            {
                if (v != fa)
                {
                    vector<long long> s = dfs(v, u);
                    long long s1 = s.front(), s2 = s.back();
                    result = max({result, max_without_leaf + s.back(), max_with_leaf + s.front()});
                    max_without_leaf = max(max_without_leaf, s.front() + price_u);
                    max_with_leaf = max(max_with_leaf, s.back() + price_u);
                }
            }
            return vector<long long>{max_without_leaf, max_with_leaf};
        };
        dfs(0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private long result = 0L;
    private Map<Integer, List<Integer>> graph = new HashMap<>();

    public long maxOutput(int n, int[][] edges, int[] price) {
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
            graph.computeIfAbsent(v, k -> new ArrayList<>()).add(u);
        }
        dfs(0, -1, price);
        return result;
    }

    private long[] dfs(int u, int fa, int[] price) {
        long maxWithoutLeaf = 0L, maxWithLeaf = price[u], priceU = price[u];
        for (int v : graph.getOrDefault(u, new ArrayList<>())) {
            if (v != fa) {
                long s[] = dfs(v, u, price), s1 = s[0], s2 = s[1];
                result = Math.max(result, Math.max(maxWithoutLeaf + s2, maxWithLeaf + s1));
                maxWithoutLeaf = Math.max(maxWithoutLeaf, s1 + priceU);
                maxWithLeaf = Math.max(maxWithLeaf, s2 + priceU);
            }
        }
        return new long[]{maxWithoutLeaf, maxWithLeaf};
    }
}
```

__Python__:

```Python
class Solution:
    def maxOutput(self, n: int, edges: List[List[int]], price: List[int]) -> int:
        result, graph = 0, defaultdict(list)
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        
        def dfs(u: int, fa: int) -> int:
            nonlocal result
            max_without_leaf, max_with_leaf, price_u = 0, price[u], price[u]
            for v in graph[u]:
                if v != fa:
                    s1, s2 = dfs(v, u)
                    result, max_without_leaf, max_with_leaf = max(result, max_without_leaf + s2, max_with_leaf + s1), max(max_without_leaf, s1 + price_u), max(max_with_leaf, s2 + price_u)
            return max_without_leaf, max_with_leaf
        dfs(0, -1)
        return result
```
