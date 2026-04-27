# 1319 Number of Operations to Make Network Connected 连通网络的操作次数

__Description:__

There are n computers numbered from 0 to n - 1 connected by ethernet cables connections forming a network where connections[i] = [ai, bi] represents a connection between computers ai and bi. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return the minimum number of times you need to do this in order to make all the computers connected. If it is not possible, return -1.

__Example:__

Example 1:

![Network 1](https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png)

Input: n = 4, connections = [[0,1],[0,2],[1,2]]
Output: 1
Explanation: Remove cable between computer 1 and 2 and place between computers 1 and 3.

Example 2:

![Network 2](https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png)

Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
Output: 2

Example 3:

Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
Output: -1
Explanation: There are not enough cables.

__Constraints:__

1 <= n <= 10^5
1 <= connections.length <= min(n * (n - 1) / 2, 10^5)
connections[i].length == 2
0 <= ai, bi < n
ai != bi
There are no repeated connections.
No two computers are connected by more than one cable.

__题目描述：__

用以太网线缆将 n 台计算机连接成一个网络，计算机的编号从 0 到 n-1。线缆用 connections 表示，其中 connections[i] = [a, b] 连接了计算机 a 和 b。

网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。

给你这个计算机网络的初始布线 connections，你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。

__示例：__

示例 1：

![网络 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/11/sample_1_1677.png)

输入：n = 4, connections = [[0,1],[0,2],[1,2]]
输出：1
解释：拔下计算机 1 和 2 之间的线缆，并将它插到计算机 1 和 3 上。

示例 2：

![网络 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/11/sample_2_1677.png)

输入：n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
输出：2

示例 3：

输入：n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
输出：-1
解释：线缆数量不足。

示例 4：

输入：n = 5, connections = [[0,1],[0,2],[3,4],[2,3]]
输出：0

__提示：__

1 <= n <= 10^5
1 <= connections.length <= min(n*(n-1)/2, 10^5)
connections[i].length == 2
0 <= connections\[i][0], connections\[i][1] < n
connections\[i][0] != connections\[i][1]
没有重复的连接。
两台计算机不会通过多条线缆连接。

__思路：__

并查集
如果线缆数量不够返回 -1
将同一个线缆连成的的计算机并入一个并查集
返回并查集的数量减 1
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int makeConnected(int n, vector<vector<int>>& connections) 
    {
        if (connections.size() < n - 1) return -1;
        vector<int> parent(n);
        for (int i = 0; i < n; i++) parent[i] = i;
        for (const auto &c : connections) merge(parent, c.front(), c.back());
        int result = 0;
        for (int i = 0; i < n; i++) if (parent[i] == i) ++result;
        return result - 1;
    }
private:
    int find(vector<int> &parent, int x) 
    {
        return parent[x] == x ? x : (parent[x] = find(parent, parent[x]));
    }
    
    void merge(vector<int> &parent, int x, int y)
    {
        parent[find(parent, y)] = find(parent, x);
    }
};
```

__Java__:

```Java
class Solution {
    public int makeConnected(int n, int[][] connections) {
        if (connections.length < n - 1) return -1;
        int result = 0, parent[] = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        for (int[] c : connections) parent[find(parent, c[0])] = find(parent, c[1]);
        for (int i = 0; i < n; i++) if (parent[i] == i) ++result;
        return result - 1;
    }
    
    private int find(int[] parent, int x) {
        return parent[x] == x ? x : (parent[x] = find(parent, parent[x]));
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
    def makeConnected(self, n: int, connections: List[List[int]]) -> int:
        if len(connections) < n - 1:
            return -1
        uf, result = UF(n), 0
        for x, y in connections:
            uf.union(x, y)
        return uf.count - 1
```
