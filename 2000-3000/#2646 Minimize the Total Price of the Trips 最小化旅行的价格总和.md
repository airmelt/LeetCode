# 2646 Minimize the Total Price of the Trips 最小化旅行的价格总和

__Description:__

There exists an undirected and unrooted tree with `n` nodes indexed from `0` to `n - 1`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Each node has an associated price. You are given an integer array `price`, where `price[i]` is the price of the `i ^ th` node.

The price sum of a given path is the sum of the prices of all nodes lying on that path.

Additionally, you are given a 2D integer array `trips`, where `trips[i] = [start_i, end_i]` indicates that you start the `i ^ th` trip from the node `start_i` and travel to the node `end_i` by any path you like.

Before performing your first trip, you can choose some non-adjacent nodes and halve the prices.

Return the minimum total price sum to perform all the given trips.

__Example:__

Example 1:

![2646-1](https://assets.leetcode.com/uploads/2023/03/16/diagram2.png)

```text
Input: n = 4, edges = [[0,1],[1,2],[1,3]], price = [2,2,10,6], trips = [[0,3],[2,1],[2,3]]
Output: 23
Explanation: The diagram above denotes the tree after rooting it at node 2. The first part shows the initial tree and the second part shows the tree after choosing nodes 0, 2, and 3, and making their price half.
For the 1st trip, we choose path [0,1,3]. The price sum of that path is 1 + 2 + 3 = 6.
For the 2nd trip, we choose path [2,1]. The price sum of that path is 2 + 5 = 7.
For the 3rd trip, we choose path [2,1,3]. The price sum of that path is 5 + 2 + 3 = 10.
The total price sum of all trips is 6 + 7 + 10 = 23.
It can be proven, that 23 is the minimum answer that we can achieve.
```

Example 2:

![2646-2](https://assets.leetcode.com/uploads/2023/03/16/diagram3.png)

```text
Input: n = 2, edges = [[0,1]], price = [2,2], trips = [[0,0]]
Output: 1
Explanation: The diagram above denotes the tree after rooting it at node 0. The first part shows the initial tree and the second part shows the tree after choosing node 0, and making its price half.
For the 1st trip, we choose path [0]. The price sum of that path is 1.
The total price sum of all trips is 1. It can be proven, that 1 is the minimum answer that we can achieve.
```

__Constraints:__

- `1 <= n <= 50`
- `edges.length == n - 1`
- `0 <= ai, bi <= n - 1`
- `edges` represents a valid tree.
- `price.length == n`
- `price[i]` is an even integer.
- `1 <= price[i] <= 1000`
- `1 <= trips.length <= 100`
- `0 <= start_i, end_i <= n - 1`

__题目描述:__

现有一棵无向、无根的树，树中有 `n` 个节点，按从 `0` 到 `n - 1` 编号。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间存在一条边。

每个节点都关联一个价格。给你一个整数数组 `price` ，其中 `price[i]` 是第 `i` 个节点的价格。

给定路径的 价格总和 是该路径上所有节点的价格之和。

另给你一个二维整数数组 `trips` ，其中 `trips[i] = [start_i, end_i]` 表示您从节点 `start_i` 开始第 `i` 次旅行，并通过任何你喜欢的路径前往节点 `end_i` 。

在执行第一次旅行之前，你可以选择一些 非相邻节点 并将价格减半。

返回执行所有旅行的最小价格总和。

__示例:__

示例 1：

![2646-3](https://assets.leetcode.com/uploads/2023/03/16/diagram2.png)

```text
输入：n = 4, edges = [[0,1],[1,2],[1,3]], price = [2,2,10,6], trips = [[0,3],[2,1],[2,3]]
输出：23
解释：
上图表示将节点 2 视为根之后的树结构。第一个图表示初始树，第二个图表示选择节点 0 、2 和 3 并使其价格减半后的树。
第 1 次旅行，选择路径 [0,1,3] 。路径的价格总和为 1 + 2 + 3 = 6 。
第 2 次旅行，选择路径 [2,1] 。路径的价格总和为 2 + 5 = 7 。
第 3 次旅行，选择路径 [2,1,3] 。路径的价格总和为 5 + 2 + 3 = 10 。
所有旅行的价格总和为 6 + 7 + 10 = 23 。可以证明，23 是可以实现的最小答案。
```

示例 2：

![2646-4](https://assets.leetcode.com/uploads/2023/03/16/diagram3.png)

```text
输入：n = 2, edges = [[0,1]], price = [2,2], trips = [[0,0]]
输出：1
解释：
上图表示将节点 0 视为根之后的树结构。第一个图表示初始树，第二个图表示选择节点 0 并使其价格减半后的树。 
第 1 次旅行，选择路径 [0] 。路径的价格总和为 1 。 
所有旅行的价格总和为 1 。可以证明，1 是可以实现的最小答案。
```

__提示：__

- `1 <= n <= 50`
- `edges.length == n - 1`
- `0 <= ai, bi <= n - 1`
- `edges` 表示一棵有效的树
- `price.length == n`
- `price[i]` 是一个偶数
- `1 <= price[i] <= 1000`
- `1 <= trips.length <= 100`
- `0 <= start_i, end_i <= n - 1`

__思路:__

```text
1. DFS
对 trips 中的每一个 trip 进行 DFS 遍历
计算经过每个点的次数
则每个点需要的价格为 price[u] * count[u]
对任意一个点, 要么 price[u] 不变, 子节点可以减半, 取两者较小值
要么 price[u] 减半, 则子节点不能减半
然后再从 0 开始 DFS 遍历计算所有的价格的最小值
时间复杂度为 O(MN), 空间复杂度为 O(N), 其中 M 为 trips 的数量
2. 树上差分 ➕ 并查集
设 start 到 end 的路径为 start - lca - end
对于 start - lca 路径 diff[start] 加 1, diff[lca] 减 1
对于 lca - end 路径 diff[end] 加 1, diff[parent[lca]] 减 1
其中 parent 为每个节点的父节点, 即并查集的父节点
lca 可以用 tarjan 算法求解
把累加 price 的值用 diff 求解
时间复杂度为 O(M + N), 空间复杂度为 O(N), 其中 M 为 trips 的数量
```

```C++
class Solution 
{
public:
    int minimumTotalPrice(int n, vector<vector<int>> &edges, vector<int> &price, vector<vector<int>> &trips) 
    {
        vector<vector<int>> graph(n), route(n);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        for (const auto& trip : trips) 
        {
            route[trip.front()].emplace_back(trip.back());
            if (trip.front() != trip.back()) route[trip.back()].emplace_back(trip.front());
        }
        vector<int> parent(n), diff(n), father(n), color(n);
        iota(parent.begin(), parent.end(), 0);
        auto find = [&](this auto&& find, int p) -> int
        {
            return parent[p] == p ? p : parent[p] = find(parent[p]);
        };
        auto tarjan = [&](this auto&& tarjan, int u, int fa) -> void
        {
            father[u] = fa;
            color[u] = 1;
            for (const auto& v : graph[u]) 
            {
                if (!color[v]) { 
                    tarjan(v, u);
                    parent[v] = u; 
                }
            }
            for (const auto& v : route[u]) 
            {
                if (v == u or color[v] == 2) 
                {
                    ++diff[u];
                    ++diff[v];
                    int lca = find(v), f = father[lca];
                    --diff[lca];
                    if (f >= 0) --diff[f];
                }
            }
            color[u] = 2;
        };
        tarjan(0, -1);
        auto dfs = [&](this auto&& dfs, int u, int fa) -> tuple<int, int, int> 
        {
            int origin = 0, half = 0, cur = diff[u];
            for (const auto& v : graph[u]) 
            {
                if (v != fa) 
                {
                    auto [pre_origin, pre_half, pre] = dfs(v, u); 
                    origin += min(pre_origin, pre_half);
                    half += pre_origin;
                    cur += pre;
                }
            }
            origin += price[u] * cur; 
            half += price[u] * cur >> 1;
            return { origin, half, cur };
        };
        auto [origin, half, cur] = dfs(0, -1);
        return min(origin, half);
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private int[] c;

    public int minimumTotalPrice(int n, int[][] edges, int[] price, int[][] trips) {
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        c = new int[n];
        for (int[] t : trips) dfs1(t[0], -1, t[1]);
        return Arrays.stream(dfs2(0, -1, price)).min().getAsInt();
    }

    private boolean dfs1(int u, int fa, int end) {
        if (u == end) {
            ++c[u];
            return true;
        }
        for (int v : graph[u]) {
            if (v != fa && dfs1(v, u, end)) {
                ++c[u];
                return true;
            }
        }
        return false;
    }

    private int[] dfs2(int u, int fa, int[] price) {
        int origin = price[u] * c[u], half = origin >> 1;
        for (int v : graph[u]) {
            if (v != fa) {
                int[] pre = dfs2(v, u, price);
                origin += Arrays.stream(pre).min().getAsInt();
                half += pre[0];
            }
        }
        return new int[]{ origin, half };
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTotalPrice(self, n: int, edges: List[List[int]], price: List[int], trips: List[List[int]]) -> int:
        graph, c = defaultdict(set), [0] * n
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs1(u: int, fa: int) -> bool:
            if u == end:
                c[u] += 1
                return True
            for v in graph[u]:
                if v != fa and dfs1(v, u):
                    c[u] += 1
                    return True
            return False
        for start, end in trips:
            dfs1(start, -1)

        def dfs2(u: int, fa: int) -> Tuple[int, int]:
            half = (origin := price[u] * c[u]) >> 1
            for v in graph[u]:
                if v != fa:
                    pre_origin, pre_half = dfs2(v, u)
                    origin += min(pre_origin, pre_half)
                    half += pre_origin
            return origin, half
        return min(dfs2(0, -1))
```
