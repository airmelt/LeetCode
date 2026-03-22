# 685 Redundant Connection II 冗余连接 II

__Description__:
In this problem, a rooted tree is a directed graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with n nodes (with distinct values from 1 to n), with one additional directed edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [ui, vi] that represents a directed edge connecting nodes ui and vi, where ui is a parent of child vi.

Return an edge that can be removed so that the resulting graph is a rooted tree of n nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

__Example:__

Example 1:

![reduntant graph 1](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

Input: edges = [[1,2],[1,3],[2,3]]
Output: [2,3]

Example 2:

![reduntant graph 2](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

Input: edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
Output: [4,1]

__Constraints:__

n == edges.length
3 <= n <= 1000
edges[i].length == 2
1 <= ui, vi <= n
ui != vi

__题目描述__:
在本问题中，有根树指满足以下条件的 有向 图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。

输入一个有向图，该图由一个有着 n 个节点（节点值不重复，从 1 到 n）的树及一条附加的有向边构成。附加的边包含在 1 到 n 中的两个不同顶点间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组 edges 。 每个元素是一对 [ui, vi]，用以表示 有向 图中连接顶点 ui 和顶点 vi 的边，其中 ui 是 vi 的一个父节点。

返回一条能删除的边，使得剩下的图是有 n 个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

__示例 :__

示例 1：

![冗余图 1](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

输入：edges = [[1,2],[1,3],[2,3]]
输出：[2,3]

示例 2：

![冗余图 2](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

输入：edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
输出：[4,1]

__提示:__

n == edges.length
3 <= n <= 1000
edges[i].length == 2
1 <= ui, vi <= n

__思路__:

并查集
用一个数组记录当前节点的起点, 每次合并时, 将终点对应数组元素指向起点
这样的话, 如果遍历到当前节点不等于当前节点的起点, 说明产生了冲突, 记录下冲突的下标
利用并查集的 find() 可以找到有向图中的环的下标
根据环和冲突(有两个父节点)的下标有 3 种不同的返回
没有冲突, 说明有一个环, 将环上最后两个节点返回即可
只有环没有冲突, 返回冲突的边
既有环又有冲突, 返回冲突的边的终点及其父节点
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class UF 
{
private:
    vector<int> parent;
public:
    UF(int n) 
    {
        parent = vector<int>(n + 1);
        for (int i = 0; i < n; i++) parent[i + 1] = i + 1;
    }

    void connect(int p, int q) 
    {
        parent[find(p)] = find(q);
    }

    int find(int p) 
    {
        while (parent[p] != p) 
        {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
};

class Solution 
{
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) 
    {
        int n = edges.size(), conflict = -1, cycle = -1;
        UF *uf = new UF(n);
        int cur[n + 1];
        for (int i = 0; i < n; i++) cur[i + 1] = i + 1;
        for (int i = 0; i < n; i++) 
        {
            int u = edges[i][0], v = edges[i][1];
            if (cur[v] != v) conflict = i;
            else 
            {
                cur[v] = u;
                if (uf -> find(u) == uf -> find(v)) cycle = i;
                else uf -> connect(u, v);
            }
        }
        return conflict < 0 ? vector<int>{ edges[cycle][0], edges[cycle][1] } : (cycle >= 0 ?  vector<int>{ cur[edges[conflict][1]], edges[conflict][1] } : vector<int>{ edges[conflict][0], edges[conflict][1] });
    }
};
```

__Java__:

```Java
class UF {
    private int[] parent;

    public UF(int n) {
        parent = new int[n + 1];
        for (int i = 0; i < n; i++) parent[i + 1] = i + 1;
    }

    public void union(int p, int q) {
        parent[find(p)] = find(q);
    }

    public int find(int p) {
        while (parent[p] != p) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }
}

class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length, conflict = -1, cycle = -1;
        UF uf = new UF(n);
        int[] cur = new int[n + 1];
        for (int i = 0; i < n; i++) cur[i + 1] = i + 1;
        for (int i = 0; i < n; i++) {
            int u = edges[i][0], v = edges[i][1];
            if (cur[v] != v) conflict = i;
            else {
                cur[v] = u;
                if (uf.find(u) == uf.find(v)) cycle = i;
                else uf.union(u, v);
            }
        }
        return conflict < 0 ? new int[]{ edges[cycle][0], edges[cycle][1] } : (cycle >= 0 ?  new int[]{ cur[edges[conflict][1]], edges[conflict][1] } : new int[]{ edges[conflict][0], edges[conflict][1] });
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n):
        self.parent = list(range(n + 1))
    
    
    def union(self, p: int, q: int):
        self.parent[self.find(p)] = self.find(q)
    
    
    def find(self, p: int) -> int:
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        uf, cycle, conflict, cur = UF(n := len(edges)), -1, -1, list(range(n + 1))
        for i, (u, v) in enumerate(edges):
            if cur[v] != v:
                conflict = i
            else:
                cur[v] = u
                if uf.find(u) == uf.find(v):
                    cycle = i
                else:
                    uf.union(u, v)
        return [edges[cycle][0], edges[cycle][1]] if conflict < 0 else [cur[edges[conflict][1]], edges[conflict][1]] if cycle >= 0 else [edges[conflict][0], edges[conflict][1]]
```
