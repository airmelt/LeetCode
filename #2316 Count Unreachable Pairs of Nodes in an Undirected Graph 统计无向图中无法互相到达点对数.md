# 2316 Count Unreachable Pairs of Nodes in an Undirected Graph 统计无向图中无法互相到达点对数

__Description:__

You are given an integer `n`. There is an __undirected__ graph with `n` nodes, numbered from `0` to `n - 1`. You are given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an __undirected__ edge connecting nodes `ai` and `bi`.

Return the number of pairs of different nodes that are unreachable from each other.

__Example:__

Example 1:

![2316-1](https://assets.leetcode.com/uploads/2022/05/05/tc-3.png)

```text
Input: n = 3, edges = [[0,1],[0,2],[1,2]]
Output: 0
Explanation: There are no pairs of nodes that are unreachable from each other. Therefore, we return 0.
```

Example 2:

![2316-2](https://assets.leetcode.com/uploads/2022/05/05/tc-2.png)

```text
Input: n = 7, edges = [[0,2],[0,5],[2,4],[1,6],[5,4]]
Output: 14
Explanation: There are 14 pairs of nodes that are unreachable from each other:
[[0,1],[0,3],[0,6],[1,2],[1,3],[1,4],[1,5],[2,3],[2,6],[3,4],[3,5],[3,6],[4,6],[5,6]].
Therefore, we return 14.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `0 <= edges.length <= 2 * 10 ^ 5`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- There are no repeated edges.

__题目描述:__

给你一个整数 `n` ，表示一张 __无向图__ 中有 `n` 个节点，编号为 `0` 到 `n - 1` 。同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 和 `bi` 之间有一条 __无向__ 边。

请你返回 无法互相到达 的不同 点对数目 。

__示例:__

示例 1：

![2316-3](https://assets.leetcode.com/uploads/2022/05/05/tc-3.png)

```text
输入：n = 3, edges = [[0,1],[0,2],[1,2]]
输出：0
解释：所有点都能互相到达，意味着没有点对无法互相到达，所以我们返回 0 。
```

示例 2：

![2316-4](https://assets.leetcode.com/uploads/2022/05/05/tc-2.png)

```text
输入：n = 7, edges = [[0,2],[0,5],[2,4],[1,6],[5,4]]
输出：14
解释：总共有 14 个点对互相无法到达：
[[0,1],[0,3],[0,6],[1,2],[1,3],[1,4],[1,5],[2,3],[2,6],[3,4],[3,5],[3,6],[4,6],[5,6]]
所以我们返回 14 。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `0 <= edges.length <= 2 * 10 ^ 5`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- 不会有重复边。

__思路:__

```text
并查集
将所有有边的点进行合并
最后统计所有根节点的点数
使用乘法原理两两组合即可得到无法互相到达的点对数目
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countPairs(int n, vector<vector<int>>& edges) 
    {
        vector<int> parent(n), weight(n, 1);
        long long result = 0LL;
        iota(parent.begin(), parent.end(), 0);
        function<int(int)> find = [&](int p)  -> int
        {
            return parent[p] == p ? p : (parent[p] = find(parent[p]));
        };

        auto unite = [&](int p, int q) -> void
        {
            if ((p = find(p)) == (q = find(q))) return;
            if (weight[p] > weight[q]) 
            {
                parent[q] = p;
                weight[p] += weight[q];
            } 
            else 
            {
                parent[p] = q;
                weight[q] += weight[p];
            }
        };

        for (const auto& edge : edges) unite(edge.front(), edge.back());
        for (int u = 0; u < n; u++) if (parent[u] == u) result += (long long)(n - weight[u]) * weight[u];
        return result >> 1LL;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;
    private int[] weight;

    public long countPairs(int n, int[][] edges) {
        parent = new int[n];
        weight = new int[n];
        for (int i = 1; i < n; i++) parent[i] = i;
        Arrays.fill(weight, 1);
        long result = 0L;
        for (int[] edge : edges) union(edge[0], edge[1]);
        for (int u = 0; u < n; u++) if (parent[u] == u) result += (long)(n - weight[u]) * weight[u];
        return result >>> 1L;
    }

    private void union(int p, int q) {
        if ((p = find(p)) == (q = find(q))) return;
        if (weight[p] > weight[q]) {
            parent[q] = p;
            weight[p] += weight[q];
        } else {
            parent[p] = q;
            weight[q] += weight[p];
        }
    }

    private int find(int p) {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }
}
```

__Python__:

```Python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

"""
@description: 并查集/Union Find
@file_name: UF.py
@project: Algorithm
@version: 1.0
@date: 2021/03/03 17:07
@author: Air
"""

__author__ = 'Air'


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
    def countPairs(self, n: int, edges: List[List[int]]) -> int:
        uf, result = UF(n), 0
        for u, v in edges:
            uf.union(u, v)
        for u in range(n):
            if uf.parent[u] == u:
                result += (n - uf.weight[u]) * uf.weight[u]
        return result >> 1
```
