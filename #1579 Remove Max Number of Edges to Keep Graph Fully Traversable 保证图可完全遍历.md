# 1579 Remove Max Number of Edges to Keep Graph Fully Traversable 保证图可完全遍历

__Description:__

Alice and Bob have an undirected graph of `n` nodes and three types of edges:

- Type 1: Can be traversed by Alice only.
- Type 2: Can be traversed by Bob only.
- Type 3: Can be traversed by both Alice and Bob.

Given an array `edges` where `edges[i] = [typei, ui, vi]` represents a bidirectional edge of type `typei` between nodes `ui` and `vi`, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return _the maximum number of edges you can remove, or return_ `-1` _if Alice and Bob cannot fully traverse the graph._

__Example:__

Example 1:

![1579-1](https://assets.leetcode.com/uploads/2020/08/19/ex1.png)

```text
Input: n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
Output: 2
Explanation: If we remove the 2 edges [1,1,2] and [1,1,3]. The graph will still be fully traversable by Alice and Bob. Removing any additional edge will not make it so. So the maximum number of edges we can remove is 2.
```

Example 2:

![1579-2](https://assets.leetcode.com/uploads/2020/08/19/ex2.png)

```text
Input: n = 4, edges = [[3,1,2],[3,2,3],[1,1,4],[2,1,4]]
Output: 0
Explanation: Notice that removing any edge will not make the graph fully traversable by Alice and Bob.
```

Example 3:

![1579-3](https://assets.leetcode.com/uploads/2020/08/19/ex3.png)

```text
Input: n = 4, edges = [[3,2,3],[1,1,2],[2,3,4]]
Output: -1
Explanation: In the current graph, Alice cannot reach node 4 from the other nodes. Likewise, Bob cannot reach 1. Therefore it's impossible to make the graph fully traversable.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `1 <= edges.length <= min(10 ^ 5, 3 * n * (n - 1) / 2)`
- `edges[i].length == 3`
- `1 <= typei <= 3`
- `1 <= ui < vi <= n`
- All tuples `(typei, ui, vi)` are distinct.

__题目描述:__

Alice 和 Bob 共有一个无向图，其中包含 n 个节点和 3  种类型的边：

- 类型 1:只能由 Alice 遍历。
- 类型 2:只能由 Bob 遍历。
- 类型 3:Alice 和 Bob 都可以遍历。

给你一个数组 `edges` ，其中 `edges[i] = [typei, ui, vi]` 表示节点 `ui` 和 `vi` 之间存在类型为 `typei` 的双向边。请你在保证图仍能够被 Alice和 Bob 完全遍历的前提下，找出可以删除的最大边数。如果从任何节点开始，Alice 和 Bob 都可以到达所有其他节点，则认为图是可以完全遍历的。

返回可以删除的最大边数，如果 Alice 和 Bob 无法完全遍历图，则返回 -1 。

__示例:__

示例 1：

![1579-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex1.png)

```text
输入：n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
输出：2
解释：如果删除 [1,1,2] 和 [1,1,3] 这两条边，Alice 和 Bob 仍然可以完全遍历这个图。再删除任何其他的边都无法保证图可以完全遍历。所以可以删除的最大边数是 2 。
```

示例 2：

![1579-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex2.png)

```text
输入：n = 4, edges = [[3,1,2],[3,2,3],[1,1,4],[2,1,4]]
输出：0
解释：注意，删除任何一条边都会使 Alice 和 Bob 无法完全遍历这个图。
```

示例 3：

![1579-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/06/5510ex3.png)

```text
输入：n = 4, edges = [[3,2,3],[1,1,2],[2,3,4]]
输出：-1
解释：在当前图中，Alice 无法从其他节点到达节点 4 。类似地，Bob 也不能达到节点 1 。因此，图无法完全遍历。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `1 <= edges.length <= min(10 ^ 5, 3 * n * (n-1) / 2)`
- `edges[i].length == 3`
- `1 <= edges[i][0] <= 3`
- `1 <= edges[i][1] < edges[i][2] <= n`
- 所有元组 `(typei, ui, vi)` 互不相同

__思路:__

```text
并查集
公共边的重要性大于两人的独立边
先将公共边通过并查集连接起来, 如果已经连接就可以删除这条边
然后遍历所有独立边, 如果已经连接,就可以删除这条边
时间复杂度为 O(M), 空间复杂度为 O(N), M 为 edges 数组的长度
```

__代码:__

__C++__:

```C++
class UF 
{
public:
    vector<int> parent;
    vector<int> weight;
    int n, size;
public:
    UF(int _n): n(_n), size(_n), parent(_n), weight(_n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x) {
        return parent[x] == x ? x : parent[x] = find(parent[x]);
    }
    
    bool unite(int x, int y) 
    {
        if ((x = find(x)) == (y = find(y))) return false;
        if (weight[x] < weight[y]) swap(x, y);
        parent[y] = x;
        weight[x] += weight[y];
        --size;
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
    int maxNumEdgesToRemove(int n, vector<vector<int>>& edges) 
    {
        UF ufa(n), ufb(n);
        int result = 0;
        for (auto& edge: edges) 
        {
            --edge[1];
            --edge[2];
        }
        for (const auto& edge: edges) if (edge.front() == 3) result += (!ufa.unite(edge[1], edge[2])) & (!ufb.unite(edge[1], edge[2]));
        for (const auto& edge: edges) result += (edge.front() == 1 and !ufa.unite(edge[1], edge[2])) or (edge.front() == 2 and !ufb.unite(edge[1], edge[2]));
        return ufa.size == 1 and ufb.size == 1 ? result : -1;
    }
};
```

__Java__:

```Java
class UF {
    int[] parent;
    int[] weight;
    int n, size;

    public UF(int n) {
        this.n = n;
        this.size = n;
        this.parent = new int[n];
        this.weight = new int[n];
        Arrays.fill(weight, 1);
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    
    public int find(int x) {
        return parent[x] == x ? x : (parent[x] = find(parent[x]));
    }
    
    public boolean unite(int x, int y) {
        if ((x = find(x)) == (y = find(y))) return false;
        if (weight[x] < weight[y]) {
            x ^= y;
            y ^= x;
            x ^= y;
        }
        parent[y] = x;
        weight[x] += weight[y];
        --size;
        return true;
    }
    
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}
class Solution {
    public int maxNumEdgesToRemove(int n, int[][] edges) {
        UF ufa = new UF(n), ufb = new UF(n);
        int result = 0;
        for (int[] edge: edges) {
            --edge[1];
            --edge[2];
        }
        for (int[] edge: edges) if (edge[0] == 3) result += ((!ufa.unite(edge[1], edge[2])) & (!ufb.unite(edge[1], edge[2]))) ? 1 : 0;
        for (int[] edge: edges) result += ((edge[0] == 1 && !ufa.unite(edge[1], edge[2])) || (edge[0] == 2 && !ufb.unite(edge[1], edge[2]))) ? 1 : 0;
        return (ufa.size == 1 && ufb.size == 1) ? result : -1;
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
    def maxNumEdgesToRemove(self, n: int, edges: List[List[int]]) -> int:
        result, uf_alice, uf_bob = 0, UF(n), UF(n)
        for t, u, v in edges:
            if t == 3:
                if not uf_alice.connected(u - 1, v - 1):
                    uf_alice.union(u - 1, v - 1)
                    uf_bob.union(u - 1, v - 1)
                else:
                    result += 1
        for t, u, v in edges:
            if t == 1:
                if not uf_alice.connected(u - 1, v - 1):
                    uf_alice.union(u - 1, v - 1)
                else:
                    result += 1
            elif t == 2:
                if not uf_bob.connected(u - 1, v - 1):
                    uf_bob.union(u - 1, v - 1)
                else:
                    result += 1
        return result if uf_alice.count == 1 and uf_bob.count == 1 else -1
```
