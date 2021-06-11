# 684 Redundant Connection 冗余连接

__Description__:
In this problem, a tree is an undirected graph that is connected and has no cycles.

You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.

Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.

__Example:__

Example 1:

![reduntant graph 1](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg)

Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]

Example 2:

![reduntant graph 2](https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg)

Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
Output: [1,4]

__Constraints:__

n == edges.length
3 <= n <= 1000
edges[i].length == 2
1 <= ai < bi <= edges.length
ai != bi
There are no repeated edges.
The given graph is connected.

__题目描述__:
在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边 [u, v] 应满足相同的格式 u < v。

__示例 :__

示例 1：

输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的无向图为:

```text
  1
 / \
2 - 3
```

示例 2：

输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
解释: 给定的无向图为:

```text
5 - 1 - 2
    |   |
    4 - 3
```

__注意:__

输入的二维数组大小在 3 到 1000。
二维数组中的整数在1到N之间，其中N是输入数组的大小。

__思路__:

1. 并查集
每次如果两个点不在同一棵树上, 将两个点连起来
否则直接返回这两个点即可
时间复杂度为 O(n), 空间复杂度为 O(n), 可以使用路径压缩和按秩合并降低时间复杂度
2. 拓扑排序
每次从 edges 中选择度为 1 的点移出
反向遍历 edges 度大于 1 的就是结果
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) 
    {
        int n = edges.size();
        vector<int> parent(n + 1, 0);
        for (int i = 0; i < n; i++) parent[i + 1] = i + 1;
        for (const auto &e : edges) 
        {
            int p1 = [&](int i) {while (parent[i] != i) i = parent[i]; return i;} (e[0]);
            int p2 = [&](int i) {while (parent[i] != i) i = parent[i]; return i;} (e[1]);
            if (p1 == p2) return e;
            else parent[p1] = p2;
        }
        return vector<int>{};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        int[] parent = new int[n + 1];
        for (int i = 0; i < n; i++) parent[i + 1] = i + 1;
        for (int[] edge : edges) {
            int v = edge[0], w = edge[1];
            v = find(parent, v);
            w = find(parent, w);
            if (v == w) return edge;
            else parent[v] = w;
        }
        return null;
    }
    
    private int find(int[] parent, int p) {
        while (p != parent[p]) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
}
```

__Python__:

```Python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        parent = {i: {i} for i in range(1, len(edges) + 1)}
        for x, y in edges:
            if parent[x] is not parent[y]:
                parent[x] |= parent[y]
                for item in parent[y]:
                    parent[item] = parent[x]
            else:
                return [min(x, y), max(x, y)]
```
