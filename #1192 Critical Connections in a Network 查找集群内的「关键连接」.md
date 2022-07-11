# 1192 Critical Connections in a Network 查找集群内的「关键连接」

__Description__:
There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.

__Example:__

Example 1:

![critical_connections](https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png)

Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
Output: [[1,3]]
Explanation: [[3,1]] is also accepted.

Example 2:

Input: n = 2, connections = [[0,1]]
Output: [[0,1]]

__Constraints:__

2 <= n <= 10^5
n - 1 <= connections.length <= 10^5
0 <= ai, bi <= n - 1
ai != bi
There are no repeated connections.

__题目描述__:
力扣数据中心有 n 台服务器，分别按从 0 到 n-1 的方式进行了编号。它们之间以「服务器到服务器」点对点的形式相互连接组成了一个内部集群，其中连接 connections 是无向的。从形式上讲，connections[i] = [a, b] 表示服务器 a 和 b 之间形成连接。任何服务器都可以直接或者间接地通过网络到达任何其他服务器。

「关键连接」 是在该集群中的重要连接，也就是说，假如我们将它移除，便会导致某些服务器无法访问其他服务器。

请你以任意顺序返回该集群内的所有 「关键连接」。

__示例 :__

示例 1：

![关键连接](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/critical-connections-in-a-network.png)

输入：n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
输出：[[1,3]]
解释：[[3,1]] 也是正确的。

示例 2:

输入：n = 2, connections = [[0,1]]
输出：[[0,1]]

__提示:__

1 <= n <= 10^5
n-1 <= connections.length <= 10^5
connections[i][0] != connections[i][1]
不存在重复的连接

__思路__:

tarjan 算法
dfn: 深度优先搜索遍历时结点 u 被搜索的次序
low: 能够回溯到的最早的已经在栈中的结点. 设以 u 为根的子树为 tree. low 定义为以下结点的  的最小值: tree中的结点或者从 tree 通过一条不在搜索树上的边能到达的结点
所有在环上的点都不是所求结点
搜索 dfn[u] < low[v] 的结点即为所求结点
时间复杂度为 O(n + m), 空间复杂度为 O(n + m), m 为边数, 即数组 connections 的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) 
    {
        int dfs_id = 0, dfn[100005], low[100005];
        vector<vector<int>> graph(n), result;
        for (const auto& c : connections)
        {
            graph[c.front()].emplace_back(c.back());
            graph[c.back()].emplace_back(c.front());
        }
        memset(dfn, 0, n * sizeof(int));
        memset(low, 0, n * sizeof(int));
        auto tarjan = [&](auto&& tarjan, int u, int p) -> void 
        {
            dfn[u] = low[u] = ++dfs_id;
            for (const auto &v: graph[u]) 
            {
                if (v == p) continue;
                if (!dfn[v]) 
                {
                    tarjan(tarjan, v, u);
                    low[u] = min(low[u], low[v]);
                    if (low[v] > dfn[u]) result.emplace_back(vector<int>{u, v});
                } else low[u] = min(low[u], dfn[v]);
            }
        };
        tarjan(tarjan, 0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        List<List<Integer>> graph = new ArrayList<>(n), result = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (List<Integer> c : connections) {
            graph.get(c.get(0)).add(c.get(1));
            graph.get(c.get(1)).add(c.get(0));
        }
        int[] dfn = new int[100_005], low = new int[100_005];
        tarjan(graph, result, 0, -1, 0, dfn, low);
        return result;
    }
    
    private void tarjan(List<List<Integer>> graph, List<List<Integer>> result, int u, int p, int id, int[] dfn, int[] low) {
        dfn[u] = low[u] = ++id;
        for (int v : graph.get(u)) {
            if (v == p) continue;
            if (dfn[v] == 0) {
                tarjan(graph, result, v, u, id, dfn, low);
                low[u] = Math.min(low[u], low[v]);
                if (low[v] > dfn[u]) result.add(new ArrayList<>(){{
                    add(u);
                    add(v);
                }});
            } else low[u] = Math.min(low[u], dfn[v]);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def criticalConnections(self, n: int, connections: List[List[int]]) -> List[List[int]]:
        graph, result, dfn, low = defaultdict(list), [], [0] * 100005, [0] * 100005
        for u, v in connections:
            graph[u].append(v)
            graph[v].append(u)
        
        def tarjan(u: int, p: int, dfs_id: int) -> None:
            dfn[u] = low[u] = dfs_id + 1
            for v in graph[u]:
                if v == p:
                    continue
                if not dfn[v]:
                    tarjan(v, u, dfs_id + 1)
                    low[u] = min(low[u], low[v])
                    if low[v] > dfn[u]:
                        result.append([u, v])
                else:
                    low[u] = min(low[u], dfn[v])
                    
        tarjan(0, -1, 0)
        return result
```
