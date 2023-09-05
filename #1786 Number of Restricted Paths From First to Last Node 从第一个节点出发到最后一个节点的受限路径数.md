# 1786 Number of Restricted Paths From First to Last Node 从第一个节点出发到最后一个节点的受限路径数

__Description:__

There is an undirected weighted connected graph. You are given a positive integer `n` which denotes that the graph has `n` nodes labeled from `1` to `n`, and an array `edges` where each `edges[i] = [ui, vi, weighti]` denotes that there is an edge between nodes `ui` and `vi` with weight equal to `weighti`.

A path from node `start` to node `end` is a sequence of nodes `[z0, z1, z2, ..., zk]` such that `z0 = start` and `zk = end` and there is an edge between `zi` and `zi+1` where `0 <= i <= k-1`.

The distance of a path is the sum of the weights on the edges of the path. Let `distanceToLastNode(x)` denote the shortest distance of a path between node `n` and node `x`. A __restricted path__ is a path that also satisfies that `distanceToLastNode(zi) > distanceToLastNode(zi+1)` where `0 <= i <= k-1`.

Return _the number of restricted paths from node_ `1` _to node_ `n`. Since that number may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1786-1](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png)

```text
Input:  n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
Output:  3
Explanation:  Each circle contains the node number in black and its `distanceToLastNode value in blue.` The three restricted paths are:
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5
```

Example 2:

![1786-2](https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png)

```text
Input:  n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
Output:  1
Explanation:  Each circle contains the node number in black and its `distanceToLastNode value in blue.` The only restricted path is 1 --> 3 --> 7.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 4`
- `n - 1 <= edges.length <= 4 * 10 ^ 4`
- `edges[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= weighti <= 10 ^ 5`
- There is at most one edge between any two nodes.
- There is at least one path between any two nodes.

__题目描述:__

现有一个加权无向连通图。给你一个正整数 `n` ，表示图中有 `n` 个节点，并按从 `1` 到 `n` 给节点编号；另给你一个数组 `edges` ，其中每个 `edges[i] = [ui, vi, weighti]` 表示存在一条位于节点 `ui` 和 `vi` 之间的边，这条边的权重为 `weighti` 。

从节点 `start` 出发到节点 `end` 的路径是一个形如 `[z0, z1, z2, ..., zk]` 的节点序列，满足 `z0 = start` 、 `zk = end` 且在所有符合 `0 <= i <= k-1` 的节点 `zi` 和 `zi+1` 之间存在一条边。

路径的距离定义为这条路径上所有边的权重总和。用 `distanceToLastNode(x)` 表示节点 `n` 和 `x` 之间路径的最短距离。__受限路径__ 为满足 `distanceToLastNode(zi) > distanceToLastNode(zi+1)` 的一条路径，其中 `0 <= i <= k-1` 。

返回从节点 `1` 出发到节点 `n` 的 __受限路径数__ 。由于数字可能很大，请返回对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

![1786-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex1.png)

```text
输入：n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
输出：3
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。三条受限路径分别是：
1) 1 --> 2 --> 5
2) 1 --> 2 --> 3 --> 5
3) 1 --> 3 --> 5
```

示例 2：

![1786-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/07/restricted_paths_ex22.png)

```text
输入：n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
输出：1
解释：每个圆包含黑色的节点编号和蓝色的 distanceToLastNode 值。唯一一条受限路径是：1 --> 3 --> 7 。
```

__提示：__

- `1 <= n <= 2 * 10 ^ 4`
- `n - 1 <= edges.length <= 4 * 10 ^ 4`
- `edges[i].length == 3`
- `1 <= ui, vi <= n`
- `ui != vi`
- `1 <= weighti <= 10 ^ 5`
- 任意两个节点之间至多存在一条边
- 任意两个节点之间至少存在一条路径

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int countRestrictedPaths(int n, vector<vector<int>>& edges) {

    }
};
```

__Java__:

```Java
class Solution {
    public int countRestrictedPaths(int n, int[][] edges) {

    }
}
```

__Python__:

```Python
def dijkstra(g: List[List[Tuple[int]]], start: int) -> List[int]:
    dist = [inf] * len(g)
    dist[start] = 0
    h = [(0, start)]
    while h:
        d, x = heappop(h)
        if d > dist[x]:
            continue
        for y, wt in g[x]:
            new_d = dist[x] + wt
            if new_d < dist[y]:
                dist[y] = new_d
                heappush(h, (new_d, y))
    return dist
mod=10**9+7
class Solution:
    def countRestrictedPaths(self, n: int, edges: List[List[int]]) -> int:
        g=[[] for _ in range(n)]
        for u,v,w in edges:
            g[u-1].append((v-1,w))
            g[v-1].append((u-1,w))
        dij=dijkstra(g,n-1)
        @cache
        def dfs(x):
            if x==0:
                return 1
            else:
                ans=0
                for nx,w in g[x]:
                    if dij[x]<dij[nx]:
                        ans=(ans+dfs(nx))%mod
                return ans
        return dfs(n-1)
```
