# 1617 Count Subtrees With Max Distance Between Cities 统计子树中城市之间最大距离

__Description:__

There are `n` cities numbered from `1` to `n`. You are given an array `edges` of size `n-1`, where `edges[i] = [ui, vi]` represents a bidirectional edge between cities `ui` and `vi`. There exists a unique path between each pair of cities. In other words, the cities form a __tree__.

A subtree is a subset of cities where every city is reachable from every other city in the subset, where the path between each pair passes through only the cities from the subset. Two subtrees are different if there is a city in one subtree that is not present in the other.

For each `d` from `1` to `n-1`, find the number of subtrees in which the __maximum distance__ between any two cities in the subtree is equal to `d`.

Return _an array of size_ `n-1` _where the_ `d ^ th` _element __(1-indexed)__ is the number of subtrees in which the __maximum distance__ between any two cities is equal to_ `d`.

Notice that the distance between the two cities is the number of edges in the path between them.

__Example:__

Example 1:

![【图解】回溯/二进制枚举/O(n^3)枚举直径端点（Python/Java/C++/Go） - 统计子树中城市之间最大距离 - 力扣（LeetCode）-1](https://assets.leetcode.com/uploads/2020/09/21/p1.png)

```text
Input: n = 4, edges = [[1,2],[2,3],[2,4]]
Output: [3,4,0]
Explanation:
The subtrees with subsets {1,2}, {2,3} and {2,4} have a max distance of 1.
The subtrees with subsets {1,2,3}, {1,2,4}, {2,3,4} and {1,2,3,4} have a max distance of 2.
No subtree has two nodes where the max distance between them is 3.
```

Example 2:

```text
Input: n = 2, edges = [[1,2]]
Output: [1]
```

Example 3:

```text
Input: n = 3, edges = [[1,2],[2,3]]
Output: [2,1]
```

__Constraints:__

- `2 <= n <= 15`
- `edges.length == n-1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- All pairs `(ui, vi)` are distinct.

__题目描述:__

给你 `n` 个城市，编号为从 `1` 到 `n` 。同时给你一个大小为 `n-1` 的数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示城市 `ui` 和 `vi` 之间有一条双向边。题目保证任意城市之间只有唯一的一条路径。换句话说，所有城市形成了一棵 __树__ 。

一棵 子树 是城市的一个子集，且子集中任意城市之间可以通过子集中的其他城市和边到达。两个子树被认为不一样的条件是至少有一个城市在其中一棵子树中存在，但在另一棵子树中不存在。

对于 `d` 从 `1` 到 `n-1` ，请你找到城市间 __最大距离__ 恰好为 `d` 的所有子树数目。

请你返回一个大小为 `n-1` 的数组，其中第 `d` 个元素（__下标从 1 开始__）是城市间 __最大距离__ 恰好等于 `d` 的子树数目。

请注意，两个城市间距离定义为它们之间需要经过的边的数目。

__示例:__

示例 1：

![【图解】回溯/二进制枚举/O(n^3)枚举直径端点（Python/Java/C++/Go） - 统计子树中城市之间最大距离 - 力扣（LeetCode）-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/p1.png)

```text
输入：n = 4, edges = [[1,2],[2,3],[2,4]]
输出：[3,4,0]
解释：
子树 {1,2}, {2,3} 和 {2,4} 最大距离都是 1 。
子树 {1,2,3}, {1,2,4}, {2,3,4} 和 {1,2,3,4} 最大距离都为 2 。
不存在城市间最大距离为 3 的子树。
```

示例 2：

```text
输入：n = 2, edges = [[1,2]]
输出：[1]
```

示例 3：

```text
输入：n = 3, edges = [[1,2],[2,3]]
输出：[2,1]
```

__提示：__

- `2 <= n <= 15`
- `edges.length == n-1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- 题目保证 `(ui, vi)` 所表示的边互不相同。

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {

    }
}
```

__Java__:

```Java
class Solution {
    public int[] countSubgraphsForEachDiameter(int n, int[][] edges) {

    }
}
```

__Python__:

```Python
class Solution:
    def countSubgraphsForEachDiameter(self, n: int, edges: List[List[int]]) -> List[int]:
        graph, dis, result = defaultdict(set), [[0] * n for _ in range(n)], [0] * (n - 1)
        for u, v in edges:
            graph[u - 1].add(v - 1)
            graph[v - 1].add(u - 1)
            
        def dfs(u: int, p: int) -> NoReturn:
            for v in graph[u]:
                if v != p:
                    dis[i][v] = dis[i][u] + 1
                    dfs(v, u)
        for i in range(n):
            dfs(i, -1)
            
        def dfs2(u: int, p: int) -> int:
            count = 1
            for v in graph[u]:
                if v != p and (di[v] < d or di[v] == d and v > j) and (dj[v] < d or dj[v] == d and v > i):
                    count *= dfs2(v, u)
            if di[u] + dj[u] > d:
                count += 1
            return count
        for i, di in enumerate(dis):
            for j in range(i + 1, n):
                dj = dis[j]
                result[(d := di[j]) - 1] += dfs2(i, -1)
        return result
```
