# 2973 Find Number of Coins to Place in Tree Nodes 树中每个节点放置的金币数目

__Description:__

You are given an __undirected__ tree with `n` nodes labeled from `0` to `n - 1`, and rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

You are also given a __0-indexed__ integer array `cost` of length `n`, where `cost[i]` is the __cost__ assigned to the `i ^ th` node.

You need to place some coins on every node of the tree. The number of coins to be placed at node `i` can be calculated as:

- If size of the subtree of node `i` is less than `3`, place `1` coin.
- Otherwise, place an amount of coins equal to the __maximum__ product of cost values assigned to `3` distinct nodes in the subtree of node `i`. If this product is __negative__, place `0` coins.

Return _an array_ `coin` _of size_ `n` _such that_ `coin[i]` _is the number of coins placed at node_ `i`_._

__Example:__

Example 1:

![2973-1](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

```text
Input: edges = [[0,1],[0,2],[0,3],[0,4],[0,5]], cost = [1,2,3,4,5,6]
Output: [120,1,1,1,1,1]
Explanation: For node 0 place 6 * 5 * 4 = 120 coins. All other nodes are leaves with subtree of size 1, place 1 coin on each of them.
```

Example 2:

![2973-2](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012614.png)

```text
Input: edges = [[0,1],[0,2],[1,3],[1,4],[1,5],[2,6],[2,7],[2,8]], cost = [1,4,2,3,5,7,8,-4,2]
Output: [280,140,32,1,1,1,1,1,1]
Explanation: The coins placed on each node are:
```

- Place 8 \* 7 \* 5 = 280 coins on node 0.
- Place 7 \* 5 \* 4 = 140 coins on node 1.
- Place 8 \* 2 \* 2 = 32 coins on node 2.
- All other nodes are leaves with subtree of size 1, place 1 coin on each of them.

Example 3:

![2973-3](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

```text
Input: edges = [[0,1],[0,2]], cost = [1,2,-2]
Output: [0,1,1]
Explanation: Node 1 and 2 are leaves with subtree of size 1, place 1 coin on each of them. For node 0 the only possible product of cost is 2 * 1 * -2 = -4. Hence place 0 coins on node 0.
```

__Constraints:__

- `2 <= n <= 2 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `cost.length == n`
- `1 <= |cost[i]| <= 10 ^ 4`
- The input is generated such that `edges` represents a valid tree.

__题目描述:__

给你一棵 `n` 个节点的 __无向__ 树，节点编号为 `0` 到 `n - 1` ，树的根节点在节点 `0` 处。同时给你一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `cost` ，其中 `cost[i]` 是第 `i` 个节点的 _开销_ 。

你需要在树中每个节点都放置金币，在节点 `i` 处的金币数目计算方法如下:

- 如果节点 `i` 对应的子树中的节点数目小于 `3` ，那么放 `1` 个金币。
- 否则，计算节点 `i` 对应的子树内 `3` 个不同节点的开销乘积的 __最大值__ ，并在节点 `i` 处放置对应数目的金币。如果最大乘积是 _负数_ ，那么放置 `0` 个金币。

请你返回一个长度为 `n` 的数组 `coin` ， `coin[i]`是节点 `i` 处的金币数目。

__示例:__

示例 1：

![2973-4](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

```text
输入：edges = [[0,1],[0,2],[0,3],[0,4],[0,5]], cost = [1,2,3,4,5,6]
输出：[120,1,1,1,1,1]
解释：在节点 0 处放置 6 * 5 * 4 = 120 个金币。所有其他节点都是叶子节点，子树中只有 1 个节点，所以其他每个节点都放 1 个金币。
```

示例 2：

![2973-5](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012614.png)

```text
输入：edges = [[0,1],[0,2],[1,3],[1,4],[1,5],[2,6],[2,7],[2,8]], cost = [1,4,2,3,5,7,8,-4,2]
输出：[280,140,32,1,1,1,1,1,1]
解释：每个节点放置的金币数分别为：
```

- 节点 0 处放置 8 \* 7 \* 5 = 280 个金币。
- 节点 1 处放置 7 \* 5 \* 4 = 140 个金币。
- 节点 2 处放置 8 \* 2 \* 2 = 32 个金币。
- 其他节点都是叶子节点，子树内节点数目为 1 ，所以其他每个节点都放 1 个金币。

示例 3：

![2973-6](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

```text
输入：edges = [[0,1],[0,2]], cost = [1,2,-2]
输出：[0,1,1]
解释：节点 1 和 2 都是叶子节点，子树内节点数目为 1 ，各放置 1 个金币。节点 0 处唯一的开销乘积是 2 * 1 * -2 = -4 。所以在节点 0 处放置 0 个金币。
```

__提示：__

- `2 <= n <= 2 * 10 ^ 4`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `cost.length == n`
- `1 <= |cost[i]| <= 10 ^ 4`
- `edges` 一定是一棵合法的树。

__思路:__

```text
DFS
在 DFS 的同时记录下所有子节点的 cost
如果当前父节点的子节点数量少于 3 直接返回 1
否则对所有子节点进行排序
选择最大的 3 个数或者最小的 2 个数(可能是负数) 与最大的数的乘积以及 0 中的较大值作为父节点的答案
如果父节点的儿子数超过了 5 中间的不可能使得结果增大可以直接舍去
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> placedCoins(vector<vector<int>>& edges, vector<int>& cost) 
    {
        int n = cost.size(), m = 0;
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        vector<long long> result(n, 1LL);
        auto dfs = [&](this auto&& dfs, int u, int fa) -> vector<int>
        {
            vector<int> cur = { cost[u] };
            for (const auto& v : graph[u])
            {
                if (v != fa)
                {
                    auto nxt = dfs(v, u);
                    cur.insert(cur.end(), nxt.begin(), nxt.end());
                }
            }
            sort(cur.begin(), cur.end());
            result[u] = (m = cur.size()) < 3 ? 1LL : max({ (long long)cur[m - 3] * cur[m - 2] * cur.back(), (long long)cur.front() * cur[1] * cur.back(), 0LL });
            return m > 5 ? vector<int>{ cur.front(), cur[1], cur[m - 3], cur[m - 2], cur.back() } : cur;
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
    long[] result;
    int[] cost;

    public long[] placedCoins(int[][] edges, int[] cost) {
        int n = cost.length;
        this.cost = cost;
        this.result = new long[n];
        this.graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        dfs(0, -1);
        return result;
    }

    private List<Integer> dfs(int u, int fa) {
        List<Integer> cur = new ArrayList(){{ add(cost[u]); }};
        for (int v : graph[u]) if (v != fa) cur.addAll(dfs(v, u));
        Collections.sort(cur);
        int m = cur.size();
        result[u] = m < 3 ? 1 : Math.max((long) cur.get(m - 3) * cur.get(m - 2) * cur.get(m - 1), Math.max((long) cur.get(0) * cur.get(1) * cur.get(m - 1), 0));
        return m > 5 ? List.of(cur.get(0), cur.get(1), cur.get(m - 3), cur.get(m - 2), cur.get(m - 1)) : cur;
    }
    
}
```

__Python__:

```Python
class Solution:
    def placedCoins(self, edges: List[List[int]], cost: List[int]) -> List[int]:
        graph, result = defaultdict(set), [1] * (n := len(cost))
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
            
        def dfs(u: int, fa: int) -> List[int]:
            cur = [cost[u]]
            for v in graph[u]:
                if v != fa:
                    cur.extend(dfs(v, u))
            cur.sort()
            m = len(cur)
            if m >= 3:
                result[u] = max(cur[-3] * cur[-2] * cur[-1], cur[0] * cur[1] * cur[-1], 0)
            return cur if m <= 5 else cur[:2] + cur[-3:]
        dfs(0, -1)
        return result
```
