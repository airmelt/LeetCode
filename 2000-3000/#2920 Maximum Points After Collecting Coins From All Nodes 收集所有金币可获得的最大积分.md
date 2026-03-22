# 2920 Maximum Points After Collecting Coins From All Nodes 收集所有金币可获得的最大积分

__Description:__

There exists an undirected tree rooted at node `0` with `n` nodes labeled from `0` to `n - 1`. You are given a 2D __integer__ array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree. You are also given a __0-indexed__ array `coins` of size `n` where `coins[i]` indicates the number of coins in the vertex `i`, and an integer `k`.

Starting from the root, you have to collect all the coins such that the coins at a node can only be collected if the coins of its ancestors have been already collected.

Coins at `nodei` can be collected in one of the following ways:

- Collect all the coins, but you will get `coins[i] - k` points. If `coins[i] - k` is negative then you will lose `abs(coins[i] - k)` points.
- Collect all the coins, but you will get `floor(coins[i] / 2)` points. If this way is used, then for all the `nodej` present in the subtree of `nodei`, `coins[j]` will get reduced to `floor(coins[j] / 2)`.

Return the maximum points you can get after collecting the coins from all the tree nodes.

__Example:__

Example 1:

![2920-1](https://assets.leetcode.com/uploads/2023/09/18/ex1-copy.png)

```text
Input: edges = [[0,1],[1,2],[2,3]], coins = [10,10,3,3], k = 5
Output: 11                        
Explanation: 
Collect all the coins from node 0 using the first way. Total points = 10 - 5 = 5.
Collect all the coins from node 1 using the first way. Total points = 5 + (10 - 5) = 10.
Collect all the coins from node 2 using the second way so coins left at node 3 will be floor(3 / 2) = 1. Total points = 10 + floor(3 / 2) = 11.
Collect all the coins from node 3 using the second way. Total points = 11 + floor(1 / 2) = 11.
It can be shown that the maximum points we can get after collecting coins from all the nodes is 11.
```

Example 2:

![2920-2](https://assets.leetcode.com/uploads/2023/09/18/ex2.png)

```text
Input: edges = [[0,1],[0,2]], coins = [8,4,4], k = 0
Output: 16
Explanation: 
Coins will be collected from all the nodes using the first way. Therefore, total points = (8 - 0) + (4 - 0) + (4 - 0) = 16.
```

__Constraints:__

- `n == coins.length`
- `2 <= n <= 10 ^ 5`
- `0 <= coins[i] <= 10 ^ 4`
- `edges.length == n - 1`
- `0 <= edges[i][0], edges[i][1] < n`
- `0 <= k <= 10 ^ 4`

__题目描述:__

有一棵由 `n` 个节点组成的无向树，以 `0` 为根节点，节点编号从 `0` 到 `n - 1` 。给你一个长度为 `n - 1` 的二维 __整数__ 数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示在树上的节点 `ai` 和 `bi` 之间存在一条边。另给你一个下标从 __0__ 开始、长度为 `n` 的数组 `coins` 和一个整数 `k` ，其中 `coins[i]` 表示节点 `i` 处的金币数量。

从根节点开始，你必须收集所有金币。要想收集节点上的金币，必须先收集该节点的祖先节点上的金币。

节点 `i` 上的金币可以用下述方法之一进行收集:

- 收集所有金币，得到共计 `coins[i] - k` 点积分。如果 `coins[i] - k` 是负数，你将会失去 `abs(coins[i] - k)` 点积分。
- 收集所有金币，得到共计 `floor(coins[i] / 2)` 点积分。如果采用这种方法，节点 `i` 子树中所有节点 `j` 的金币数 `coins[j]` 将会减少至 `floor(coins[j] / 2)` 。

返回收集 所有 树节点的金币之后可以获得的最大积分。

__示例:__

示例 1：

![2920-3](https://assets.leetcode.com/uploads/2023/09/18/ex1-copy.png)

```text
输入：edges = [[0,1],[1,2],[2,3]], coins = [10,10,3,3], k = 5
输出：11                        
解释：
使用第一种方法收集节点 0 上的所有金币。总积分 = 10 - 5 = 5 。
使用第一种方法收集节点 1 上的所有金币。总积分 = 5 + (10 - 5) = 10 。
使用第二种方法收集节点 2 上的所有金币。所以节点 3 上的金币将会变为 floor(3 / 2) = 1 ，总积分 = 10 + floor(3 / 2) = 11 。
使用第二种方法收集节点 3 上的所有金币。总积分 =  11 + floor(1 / 2) = 11.
可以证明收集所有节点上的金币能获得的最大积分是 11 。
```

示例 2：

![2920-4](https://assets.leetcode.com/uploads/2023/09/18/ex2.png)

```text
输入：edges = [[0,1],[0,2]], coins = [8,4,4], k = 0
输出：16
解释：
使用第一种方法收集所有节点上的金币，因此，总积分 = (8 - 0) + (4 - 0) + (4 - 0) = 16 。
```

__提示：__

- `n == coins.length`
- `2 <= n <= 10 ^ 5`
- `0 <= coins[i] <= 10 ^ 4`
- `edges.length == n - 1`
- `0 <= edges[i][0], edges[i][1] < n`
- `0 <= k <= 10 ^ 4`

__思路:__

```text
动态规划
注意这里 0 <= coins[i] <= 10 ^ 4
所以最多为 2 ^ 14 次方
设 dp[i][j] 表示 i 结点对应位移 j 次的得分
如果用第一类得分 dp[i][j] = (coins[i] >> j) - k + sum(dp[i_children][j])
如果用第二类得分 dp[i][j] = (coins[i] >> (j + 1)) + sum(dp[i_children][j + 1])
取较大值
这里可以每个 i 返回一个长度为 14 的列表
dp[13] = (max(coins[i] >> 13) - k + dp[13], 0) 来特判最后情况
最后返回 dfs(-1, 0)[0] 即可
时间复杂度为 O(NlogM), 空间复杂度为 O(NlogM), M 为 max(coins)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumPoints(vector<vector<int>>& edges, vector<int>& coins, int k) 
    {
        vector<vector<int>> graph(coins.size());
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        auto dfs = [&](this auto&& dfs, int u, int fa) -> array<int, 14> 
        {
            array<int, 14> dp{}, dp_next;
            int i = 0;
            for (const auto& v : graph[u]) if (v != fa) for (i = 0, dp_next = dfs(v, u); i < 14; i++) dp[i] += dp_next[i];
            for (i = 0; i < 13; i++) dp[i] = max((coins[u] >> i) - k + dp[i], (coins[u] >> (i + 1)) + dp[i + 1]);
            dp.back() = max(dp.back() + (coins[u] >> 13) - k, 0);
            return dp;
        };
        return dfs(0, -1).front();
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    int[] coins;
    int k;

    public int maximumPoints(int[][] edges, int[] coins, int k) {
        graph = new ArrayList[coins.length];
        Arrays.setAll(graph, e -> new ArrayList<>());
        this.k = k;
        this.coins = coins;
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        return dfs(0, -1)[0];
    }

    private int[] dfs(int u, int fa) {
        var dp = new int[14];
        for (int v : graph[u]) if (v != fa) for (int i = 0, dpNext[] = dfs(v, u); i < 14; i++) dp[i] += dpNext[i];
        for (int i = 0; i < 13; i++) dp[i] = Math.max((coins[u] >> i) - k + dp[i], (coins[u] >> (i + 1)) + dp[i + 1]);
        dp[13] = Math.max(dp[13] + (coins[u] >> 13) - k, 0);
        return dp;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumPoints(self, edges: List[List[int]], coins: List[int], k: int) -> int:
        graph = defaultdict(set)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        
        def dfs(u: int, fa: int) -> List[int]:
            dp = [0] * 14
            for v in graph[u]:
                if v != fa:
                    for i, w in enumerate(dfs(v, u)):
                        dp[i] += w
            for i in range(13):
                dp[i] = max((coins[u] >> i) - k + dp[i], (coins[u] >> (i + 1)) + dp[i + 1])
            dp[-1] = max(dp[-1] + (coins[u] >> 13) - k, 0)
            return dp
        return dfs(0, -1)[0]
```
