# 785 Is Graph Bipartite? 判断二分图

__Description__:
There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v. The graph has the following properties:

There are no self-edges (graph[u] does not contain u).
There are no parallel edges (graph[u] does not contain duplicate values).
If v is in graph[u], then u is in graph[v] (the graph is undirected).
The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.
A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.

Return true if and only if it is bipartite.

__Example:__

Example 1:

![bi 1](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
Output: false
Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.

Example 2:

![bi 2](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

Input: graph = [[1,3],[0,2],[1,3],[0,2]]
Output: true
Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.

__Constraints:__

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] does not contain u.
All the values of graph[u] are unique.
If graph[u] contains v, then graph[v] contains u.

__题目描述__:
存在一个 无向图 ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。给你一个二维数组 graph ，其中 graph[u] 是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。该无向图同时具有以下属性：
不存在自环（graph[u] 不包含 u）。
不存在平行边（graph[u] 不包含重复值）。
如果 v 在 graph[u] 内，那么 u 也应该在 graph[v] 内（该图是无向图）
这个图可能不是连通图，也就是说两个节点 u 和 v 之间可能不存在一条连通彼此的路径。
二分图 定义：如果能将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合，就将这个图称为 二分图 。

如果图是二分图，返回 true ；否则，返回 false 。

__示例 :__

示例 1：

![二分图 1](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
输出：false
解释：不能将节点分割成两个独立的子集，以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。

示例 2：

![二分图 2](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

输入：graph = [[1,3],[0,2],[1,3],[0,2]]
输出：true
解释：可以将节点分成两组: {0, 2} 和 {1, 3} 。

__提示:__

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] 不会包含 u
graph[u] 的所有值 互不相同
如果 graph[u] 包含 v，那么 graph[v] 也会包含 u

__思路__:

染色法
选择一个结点染成红色, 然后遍历它的邻居, 染成绿色
如果遍历过程中, 结点已经被染色且颜色与邻居相同, 返回 false
否则遍历完成之后返回 true
时间复杂度为 O(V + E), 空间复杂度为 O(V), 其中 V 和 E 分别为无向图中的点数和边数

__代码__:
__C++__:

```C++
class Solution 
{
private:
    bool dfs(const vector<vector<int>> &graph, int i, int c, vector<int> &v) 
    {
        if (v[i] != -1) return v[i] == c;
        v[i] = c;   
        for (const auto& j : graph[i]) if (!dfs(graph, j, !c, v)) return false;  
        return true;
    }
public:
    bool isBipartite(vector<vector<int>>& graph) 
    {
        vector<int> v(graph.size(), -1);  
        for (int i = 0, n = graph.size(); i < n; i++) if (v[i] == -1 and !dfs(graph, i, 0, v)) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length, v[] = new int[n];
        for (int i = 0; i < n; i++) if (v[i] == 0 && !dfs(graph, i, 1, v)) return false;
        return true;
    }
    
    private boolean dfs(int[][] graph, int i, int c, int[] v) 
    {
        if (v[i] != 0) return v[i] == c;
        v[i] = c;   
        for (int j : graph[i]) if (!dfs(graph, j, c == 1 ? 2 : 1, v)) return false;  
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        v = [-1] * (n := len(graph))
        
        def dfs(i: int, c: int) -> bool:
            if v[i] != -1:
                return v[i] == c
            v[i] = c
            return all(dfs(j, 1 - c) for j in graph[i])
            
        return not any(v[i] == -1 and not dfs(i, 0) for i in range(n))
```
