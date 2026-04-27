# 2360 Longest Cycle in a Graph 图中的最长环

__Description:__

You are given a __directed__ graph of `n` nodes numbered from `0` to `n - 1`, where each node has __at most one__ outgoing edge.

The graph is represented with a given __0-indexed__ array `edges` of size `n`, indicating that there is a directed edge from node `i` to node `edges[i]`. If there is no outgoing edge from node `i`, then `edges[i] == -1`.

Return _the length of the __longest__ cycle in the graph_. If no cycle exists, return `-1`.

A cycle is a path that starts and ends at the same node.

__Example:__

Example 1:

![2360-1](https://assets.leetcode.com/uploads/2022/06/08/graph4drawio-5.png)

```text
Input: edges = [3,3,4,2,3]
Output: 3
Explanation: The longest cycle in the graph is the cycle: 2 -> 4 -> 3 -> 2.
The length of this cycle is 3, so 3 is returned.
```

Example 2:

![2360-2](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-1.png)

```text
Input: edges = [2,-1,3,1]
Output: -1
Explanation: There are no cycles in this graph.
```

__Constraints:__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `-1 <= edges[i] < n`
- `edges[i] != i`

__题目描述:__

给你一个 `n` 个节点的 _有向图_ ，节点编号为 `0` 到 `n - 1` ，其中每个节点 __至多__ 有一条出边。

图用一个大小为 `n` 下标从 __0__ 开始的数组 `edges` 表示，节点 `i` 到节点 `edges[i]` 之间有一条有向边。如果节点 `i` 没有出边，那么 `edges[i] == -1` 。

请你返回图中的 __最长__ 环，如果没有任何环，请返回 `-1` 。

一个环指的是起点和终点是 同一个 节点的路径。

__示例:__

示例 1：

![2360-3](https://assets.leetcode.com/uploads/2022/06/08/graph4drawio-5.png)

```text
输入：edges = [3,3,4,2,3]
输出去：3
解释：图中的最长环是：2 -> 4 -> 3 -> 2 。
这个环的长度为 3 ，所以返回 3 。
```

示例 2：

![2360-4](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-1.png)

```text
输入：edges = [2,-1,3,1]
输出：-1
解释：图中没有任何环。
```

__提示：__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `-1 <= edges[i] < n`
- `edges[i] != i`

__思路:__

```text
1. 并查集
将两个边加入并查集
如果两个边在同一个分量中, 则说明存在环
此时更新最大环的长度
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 内向基环树
用时间戳记录访问每个节点的时刻
如果 visited[i] != 0
说明是一个未访问过的节点
从该节点开始访问, 初始访问时间记为 start, 记录访问的时刻 clock 
如果访问到已经访问过的节点, visited[x] != 0, 则说明存在环
更新最大环的长度 clock - visited[x]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestCycle(vector<int>& edges) 
    {
        int n = edges.size(), result = -1;
        vector<int> visited(n);
        for (int i = 0, clock = 1; i < n; i++) 
        {
            if (!visited[i]) 
            {
                for (int x = i, start = clock; x >= 0; x = edges[x]) 
                {
                    if (visited[x]) 
                    {
                        if (visited[x] >= start) result = max(result, clock - visited[x]);
                        break;
                    }
                    visited[x] = clock++;
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestCycle(int[] edges) {
        int n = edges.length, result = -1, visited[] = new int[n];
        for (int i = 0, clock = 1; i < n; i++) {
            if (visited[i] == 0) {
                for (int x = i, start = clock; x >= 0; x = edges[x]) {
                    if (visited[x] > 0) {
                        if (visited[x] >= start) result = Math.max(result, clock - visited[x]);
                        break;
                    }
                    visited[x] = clock++;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [0] * n
        self.count = n
        self.max_value = -1

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        x = self.find(p)
        y = self.find(q)
        if x != y:
            self.parent[x], self.weight[p] = y, self.weight[q] + 1
        else:
            self.max_value = max(self.max_value, self.weight[p] + self.weight[q] + 1)

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
        if self.parent[p] != p:
            last = self.parent[p]
            self.parent[p] = self.find(self.parent[p])
            self.weight[p] += self.weight[last]
        return self.parent[p]

class Solution:
    def longestCycle(self, edges: List[int]) -> int:
        uf = UF(n := len(edges))
        for i in range(n):
            if edges[i] != -1:
                uf.union(i, edges[i])
        return uf.max_value
```
