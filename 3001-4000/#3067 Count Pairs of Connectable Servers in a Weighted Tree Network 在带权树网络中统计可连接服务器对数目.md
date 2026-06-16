# 3067 Count Pairs of Connectable Servers in a Weighted Tree Network 在带权树网络中统计可连接服务器对数目

__Description:__

You are given an unrooted weighted tree with `n` vertices representing servers numbered from `0` to `n - 1`, an array `edges` where `edges[i] = [ai, bi, weighti]` represents a bidirectional edge between vertices `ai` and `bi` of weight `weighti`. You are also given an integer `signalSpeed`.

Two servers `a` and `b` are __connectable__ through a server `c` if:

- `a < b`, `a != c` and `b != c`.
- The distance from `c` to `a` is divisible by `signalSpeed`.
- The distance from `c` to `b` is divisible by `signalSpeed`.
- The path from `c` to `b` and the path from `c` to `a` do not share any edges.

Return _an integer array_ `count` _of length_ `n` _where_ `count[i]` _is the __number__ of server pairs that are __connectable__ through_ _the server_ `i`.

__Example:__

Example 1:

![3067-1](https://assets.leetcode.com/uploads/2024/01/21/example22.png)

```text
Input: edges = [[0,1,1],[1,2,5],[2,3,13],[3,4,9],[4,5,2]], signalSpeed = 1
Output: [0,4,6,6,4,0]
Explanation: Since signalSpeed is 1, count[c] is equal to the number of pairs of paths that start at c and do not share any edges.
In the case of the given path graph, count[c] is equal to the number of servers to the left of c multiplied by the servers to the right of c.
```

Example 2:

![3067-2](https://assets.leetcode.com/uploads/2024/01/21/example11.png)

```text
Input: edges = [[0,6,3],[6,5,3],[0,3,1],[3,2,7],[3,1,6],[3,4,2]], signalSpeed = 3
Output: [2,0,0,0,0,0,2]
Explanation: Through server 0, there are 2 pairs of connectable servers: (4, 5) and (4, 6).
Through server 6, there are 2 pairs of connectable servers: (4, 5) and (0, 5).
It can be shown that no two servers are connectable through servers other than 0 and 6.
```

__Constraints:__

- `2 <= n <= 1000`
- `edges.length == n - 1`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `edges[i] = [ai, bi, weighti]`
- `1 <= weighti <= 10 ^ 6`
- `1 <= signalSpeed <= 10 ^ 6`
- The input is generated such that `edges` represents a valid tree.

__题目描述:__

给你一棵无根带权树，树中总共有 `n` 个节点，分别表示 `n` 个服务器，服务器从 `0` 到 `n - 1` 编号。同时给你一个数组 `edges` ，其中 `edges[i] = [ai, bi, weighti]` 表示节点 `ai` 和 `bi` 之间有一条双向边，边的权值为 `weighti` 。再给你一个整数 `signalSpeed` 。

如果两台服务器 `a` 和 `b` 是通过服务器 `c` __可连接的__，则:

- `a < b` ， `a != c` 且 `b != c` 。
- 从 `c` 到 `a` 的距离是可以被 `signalSpeed` 整除的。
- 从 `c` 到 `b` 的距离是可以被 `signalSpeed` 整除的。
- 从 `c` 到 `b` 的路径与从 `c` 到 `a` 的路径没有任何公共边。

请你返回一个长度为 `n` 的整数数组 `count` ，其中 `count[i]` 表示通过服务器 `i` __可连接__ 的服务器对的 __数目__ 。

__示例:__

示例 1：

![3067-3](https://assets.leetcode.com/uploads/2024/01/21/example22.png)

```text
输入：edges = [[0,1,1],[1,2,5],[2,3,13],[3,4,9],[4,5,2]], signalSpeed = 1
输出：[0,4,6,6,4,0]
解释：由于 signalSpeed 等于 1 ，count[c] 等于所有从 c 开始且没有公共边的路径对数目。
在输入图中，count[c] 等于服务器 c 左边服务器数目乘以右边服务器数目。
```

示例 2：

![3067-4](https://assets.leetcode.com/uploads/2024/01/21/example11.png)

```text
输入：edges = [[0,6,3],[6,5,3],[0,3,1],[3,2,7],[3,1,6],[3,4,2]], signalSpeed = 3
输出：[2,0,0,0,0,0,2]
解释：通过服务器 0 ，有 2 个可连接服务器对(4, 5) 和 (4, 6) 。
通过服务器 6 ，有 2 个可连接服务器对 (4, 5) 和 (0, 5) 。
所有服务器对都必须通过服务器 0 或 6 才可连接，所以其他服务器对应的可连接服务器对数目都为 0 。
```

__提示：__

- `2 <= n <= 1000`
- `edges.length == n - 1`
- `edges[i].length == 3`
- `0 <= ai, bi < n`
- `edges[i] = [ai, bi, weighti]`
- `1 <= weighti <= 10 ^ 6`
- `1 <= signalSpeed <= 10 ^ 6`
- 输入保证 `edges` 构成一棵合法的树。

__思路:__

```text
DFS
以每一个节点为根进行 DFS
求出当前 DFS 的结果 c 以及累计的结果 cur
将每一段的 c 和 cur 相乘并累加就是该根节点的和
当前节点数少于 2 个时一定为 0 可以不用 DFS
在 DFS 时第一条路径与 signalSpeed 取模如果为 0 则初始化结果为 1 否则为 0
然后一边 DFS 一边累加结果
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countPairsOfConnectableServers(vector<vector<int>>& edges, int signalSpeed) 
    {
        int n = edges.size() + 1;
        vector<int> result(n);
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge[0]].emplace_back(make_pair(edge[1], edge[2]));
            graph[edge[1]].emplace_back(make_pair(edge[0], edge[2]));
        }
        auto dfs = [&](this auto&& dfs, int u, int fa, int acc) -> int 
        {
            int result = !(acc % signalSpeed);
            for (const auto &[v, w] : graph[u]) if (v != fa) result += dfs(v, u, acc + w);
            return result;
        };
        for (int u = 0, cur = 0, c = 0; u < n; u++, cur = 0)
        {
            if (graph[u].size() > 1)
            {
                for (const auto &[v, w] : graph[u])
                {
                    result[u] += (c = dfs(v, u, w)) * cur;
                    cur += c;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countPairsOfConnectableServers(int[][] edges, int signalSpeed) {
        int n = edges.length + 1, result[] = new int[n];
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, i -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(new int[]{ edge[1], edge[2] });
            graph[edge[1]].add(new int[]{ edge[0], edge[2] });
        }
        for (int u = 0, cur = 0, c = 0; u < n; u++, cur = 0) {
            if (graph[u].size() > 1) {
                for (var edge : graph[u]) {
                    result[u] += (c = dfs(edge[0], u, edge[1], graph, signalSpeed)) * cur;
                    cur += c;
                }
            }
        }
        return result;
    }

    private int dfs(int u, int fa, int cur, List<int[]>[] graph, int signalSpeed) {
        int result = cur % signalSpeed == 0 ? 1 : 0, v = 0;
        for (var edge : graph[u]) if ((v = edge[0]) != fa) result += dfs(v, u, cur + edge[1], graph, signalSpeed);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPairsOfConnectableServers(self, edges: List[List[int]], signalSpeed: int) -> List[int]:
        graph, result = defaultdict(set), [0] * (n := len(edges) + 1)
        for u, v, w in edges:
            graph[u].add((v, w))
            graph[v].add((u, w))

        @lru_cache(None)
        def dfs(u: int, fa: int, acc: int) -> NoReturn:
            result = 0 if acc % signalSpeed else 1
            for v, w in graph[u]:
                if v != fa:
                    result += dfs(v, u, acc + w)
            return result

        for u in range(n):
            if len(graph[u]) > 1:
                cur = 0
                for v, w in graph[u]:
                    result[u] += (c := dfs(v, u, w)) * cur
                    cur += c
        return result
```
