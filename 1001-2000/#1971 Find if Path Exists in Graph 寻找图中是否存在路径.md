# 1971 Find if Path Exists in Graph 寻找图中是否存在路径

__Description:__

There is a __bi-directional__ graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (__inclusive__). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by __at most one__ edge, and no vertex has an edge to itself.

You want to determine if there is a __valid path__ that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` _if there is a __valid path__ from_ `source` _to_ `destination`_, or_ `false` _otherwise__._

__Example:__

Example 1:

![1971-1](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```text
Input: n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
Output: true
Explanation: There are two paths from vertex 0 to vertex 2:
- 0 → 1 → 2
- 0 → 2
```

Example 2:

![1971-2](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```text
Input: n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
Output: false
Explanation: There is no path from vertex 0 to vertex 5.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 5`
- `0 <= edges.length <= 2 * 10 ^ 5`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- There are no duplicate edges.
- There are no self edges.

__题目描述:__

有一个具有 `n` 个顶点的 __双向__ 图，其中每个顶点标记从 `0` 到 `n - 1`（包含 `0` 和 `n - 1`）。图中的边用一个二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由 __最多一条__ 边连接，并且没有顶点存在与自身相连的边。

请你确定是否存在从顶点 `source` 开始，到顶点 `destination` 结束的 __有效路径__ 。

给你数组 `edges` 和整数 `n`、 `source` 和 `destination`，如果从 `source` 到 `destination` 存在 __有效路径__ ，则返回 `true`，否则返回 `false` 。

__示例:__

示例 1：

![1971-3](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```text
输入：n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
输出：true
解释：存在由顶点 0 到顶点 2 的路径:
- 0 → 1 → 2 
- 0 → 2
```

示例 2：

![1971-4](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```text
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

__提示：__

- `1 <= n <= 2 * 10 ^ 5`
- `0 <= edges.length <= 2 * 10 ^ 5`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- 不存在重复边
- 不存在指向顶点自身的边

__思路:__

```text
并查集
将所有边进行合并, 然后检查 source 和 destination 是否在同一个分量中
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) 
    {
        vector<int> parent(n);
        iota(parent.begin(), parent.end(), 0);
        for (const auto& edge : edges) parent[find(parent, edge.front())] = parent[find(parent, edge.back())];
        return find(parent, source) == find(parent, destination);
    }
private:
    int find(vector<int>& parent, int node) 
    {
        return node == parent[node] ? node : (parent[node] = find(parent, parent[node]));
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        int parent[] = new int[n];
        for (int i = 1; i < n; i++) parent[i] = i;
        for (int[] edge : edges) parent[find(parent, edge[0])] = parent[find(parent, edge[1])];
        return find(parent, source) == find(parent, destination);
    }

    private int find(int[] parent, int node) {
        return node == parent[node] ? node : (parent[node] = find(parent, parent[node]));
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
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        uf = UF(n)
        for a, b in edges:
            uf.union(a, b)
        return uf.connected(source, destination)
```
