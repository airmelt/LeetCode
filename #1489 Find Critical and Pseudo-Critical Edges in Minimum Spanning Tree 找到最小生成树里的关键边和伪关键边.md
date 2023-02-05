# 1489 Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree 找到最小生成树里的关键边和伪关键边

__Description:__

Given a weighted undirected connected graph with `n` vertices numbered from `0` to `n - 1`, and an array `edges` where `edges[i] = [ai, bi, weighti]` represents a bidirectional and weighted edge between nodes `ai` and `bi`. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles and with the minimum possible total edge weight.

Find all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST). An MST edge whose deletion from the graph would cause the MST weight to increase is called a critical edge. On the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.

__Example:__

Example 1:

![1489-1](https://assets.leetcode.com/uploads/2020/06/04/ex1.png)

```text
Input: n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
Output: [[0,1],[2,3,4,5]]
Explanation: The figure above describes the graph.
The following figure shows all the possible MSTs:

Notice that the two edges 0 and 1 appear in all MSTs, therefore they are critical edges, so we return them in the first list of the output.
The edges 2, 3, 4, and 5 are only part of some MSTs, therefore they are considered pseudo-critical edges. We add them to the second list of the output.
```

![1489-2](https://assets.leetcode.com/uploads/2020/06/04/msts.png)

Example 2:

![1489-3](https://assets.leetcode.com/uploads/2020/06/04/ex2.png)

```text
Input: n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
Output: [[],[0,1,2,3]]
Explanation: We can observe that since all 4 edges have equal weight, choosing any 3 edges from the given 4 will yield an MST. Therefore all 4 edges are pseudo-critical.
```

__Constraints:__

- `2 <= n <= 100`
- `1 <= edges.length <= min(200, n * (n - 1) / 2)`
- `edges[i].length == 3`
- `0 <= ai < bi < n`
- `1 <= weighti <= 1000`
- All pairs `(ai, bi)` are __distinct__.

__题目描述:__

给你一个 `n` 个点的带权无向连通图，节点编号为 `0` 到 `n-1` ，同时还有一个数组 `edges` ，其中 `edges[i] = [from` `i, toi, weighti]` 表示在 `fromi` 和 `toi` 节点之间有一条带权无向边。最小生成树 (MST) 是给定图中边的一个子集，它连接了所有节点且没有环，而且这些边的权值和最小。

请你找到给定图中最小生成树的所有关键边和伪关键边。如果从图中删去某条边，会导致最小生成树的权值和增加，那么我们就说它是一条关键边。伪关键边则是可能会出现在某些最小生成树中但不会出现在所有最小生成树中的边。

请注意，你可以分别以任意顺序返回关键边的下标和伪关键边的下标。

__示例:__

示例 1：

![1489-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/21/ex1.png)

```text
输入：n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
输出：[[0,1],[2,3,4,5]]
解释：上图描述了给定图。
下图是所有的最小生成树。

注意到第 0 条边和第 1 条边出现在了所有最小生成树中，所以它们是关键边，我们将这两个下标作为输出的第一个列表。
边 2，3，4 和 5 是所有 MST 的剩余边，所以它们是伪关键边。我们将它们作为输出的第二个列表。
```

![1489-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/21/msts.png)

示例 2 ：

![1489-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/21/ex2.png)

```text
输入：n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
输出：[[],[0,1,2,3]]
解释：可以观察到 4 条边都有相同的权值，任选它们中的 3 条可以形成一棵 MST 。所以 4 条边都是伪关键边。
```

__提示：__

- `2 <= n <= 100`
- `1 <= edges.length <= min(200, n * (n - 1) / 2)`
- `edges[i].length == 3`
- `0 <= fromi < toi < n`
- `1 <= weighti <= 1000`
- 所有 `(fromi, toi)` 数对都是互不相同的。

__思路:__

```text
Kruskal ➕ 并查集
由于已经给了每一条边的权重, 考虑用 Kruskal 算法得到最小生成树
遍历每一条边
如果为关键边, 那么去掉这一条边要么不能得到最小生成树, 要么会使得生成树的权重增加
如果为伪关键边, 这条边不会使得生成树的权重增加
时间复杂度为 O(M ^ 2), 空间复杂度为 O(M + N), 使用 Kruskal 需要对边权重进行排序, 也可以使用优先队列, 时间复杂度为 O(MlogM), 使用带权重的并查集遍历边的时间复杂度为 O(M ^ 2a(M)), 其中 a(M) 为阿克曼函数的反函数, 近似为 O(1), 其中 M 为 edges 数组的长度
```

__代码:__

__C++__:

```C++

class UF 
{
private:
    int find(int x) 
    {
        return parent[x] == x ? x : (parent[x] = find(parent[x]));
    }
public:
    vector<int> parent;
    vector<int> weight;
    int count;
    
    UF(int _n): count(_n), parent(_n), weight(_n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }
    
    bool connect(int x, int y) 
    {
        x = find(x);
        y = find(y);
        if (x == y) return false;
        if (weight[x] < weight[y]) swap(x, y);
        parent[y] = x;
        weight[x] += weight[y];
        --count;
        return true;
    }
    
    bool connected(int x, int y) 
    {
        return find(x) == find(y);
    }
};

class Solution 
{
public:
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) 
    {
        int m = edges.size(), value = 0;
        for (int i = 0; i < m; i++) edges[i].emplace_back(i);
        sort(edges.begin(), edges.end(), [](const auto& a, const auto& b) {return a[2] < b[2];});
        UF uf(n);
        for (int i = 0; i < m; i++) if (uf.connect(edges[i][0], edges[i][1])) value += edges[i][2];
        vector<vector<int>> result(2, vector<int>());
        for (int i = 0; i < m; i++) 
        {
            uf = UF(n);
            int v = 0;
            for (int j = 0; j < m; j++) if (i != j and uf.connect(edges[j][0], edges[j][1])) v += edges[j][2];
            if (uf.count != 1 or (uf.count == 1 and v > value)) 
            {
                result.front().emplace_back(edges[i][3]);
                continue;
            }
            uf = UF(n);
            uf.connect(edges[i][0], edges[i][1]);
            v = edges[i][2];
            for (int j = 0; j < m; j++) if (i != j and uf.connect(edges[j][0], edges[j][1])) v += edges[j][2];
            if (v == value) result.back().emplace_back(edges[i][3]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> findCriticalAndPseudoCriticalEdges(int n, int[][] edges) {
        int m = edges.length, graph[][] = new int[m][4], value = 0;
        for (int i = 0; i < m; i++) {
            graph[i][3] = i;
            for (int j = 0; j < 3; j++) graph[i][j] = edges[i][j];
        }
        Arrays.sort(graph, (int[] a, int[] b) -> a[2] - b[2]);
        UF uf = new UF(n);
        for (int i = 0; i < m; i++) if (uf.union(graph[i][0], graph[i][1])) value += graph[i][2];
        List<List<Integer>> result = new ArrayList<List<Integer>>(2){{
            add(new ArrayList<Integer>());
            add(new ArrayList<Integer>());
        }};
        for (int i = 0; i < m; i++) {
            uf = new UF(n);
            int v = 0;
            for (int j = 0; j < m; j++) if (i != j && uf.union(graph[j][0], graph[j][1])) v += graph[j][2];
            if (uf.count != 1 || (uf.count == 1 && v > value)) {
                result.get(0).add(graph[i][3]);
                continue;
            }
            uf = new UF(n);
            uf.union(graph[i][0], graph[i][1]);
            v = graph[i][2];
            for (int j = 0; j < m; j++) if (i != j && uf.union(graph[j][0], graph[j][1])) v += graph[j][2];
            if (v == value) result.get(1).add(graph[i][3]);
        }
        return result;
    }
}

class UF {
    int[] parent;
    int[] weight;
    int count;

    public UF(int n) {
        this.count = n;
        this.parent = new int[n];
        this.weight = new int[n];
        Arrays.fill(weight, 1);
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    
    public int find(int x) {
        return parent[x] == x ? x : (parent[x] = find(parent[x]));
    }
    
    public boolean union(int x, int y) {
        x = find(x);
        y = find(y);
        if (x == y) return false;
        if (weight[x] < weight[y]) {
            x ^= y;
            y ^= x;
            x ^= y;
        }
        parent[y] = x;
        weight[x] += weight[y];
        --count;
        return true;
    }
    
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def findCriticalAndPseudoCriticalEdges(self, n: int, edges: List[List[int]]) -> List[List[int]]:
        for i, edge in enumerate(edges):
            edge.append(i)
        edges.sort(key=lambda x: x[2])
        result, uf, value, m = [[], []], UF(n), 0, len(edges)
        for i in range(m):
            if not uf.connected(edges[i][0], edges[i][1]):
                uf.union(edges[i][0], edges[i][1])
                value += edges[i][2]
        for i in range(m):
            uf, v = UF(n), 0
            for j in range(m):
                if i != j and not uf.connected(edges[j][0], edges[j][1]):
                    uf.union(edges[j][0], edges[j][1])
                    v += edges[j][2]
            if uf.count != 1 or (uf.count == 1 and v > value):
                result[0].append(edges[i][3])
                continue
            uf, v = UF(n), edges[i][2]
            uf.union(edges[i][0], edges[i][1])
            for j in range(m):
                if i != j and not uf.connected(edges[j][0], edges[j][1]):
                    uf.union(edges[j][0], edges[j][1])
                    v += edges[j][2]
            if v == value:
                result[1].append(edges[i][3])
        return result
```
