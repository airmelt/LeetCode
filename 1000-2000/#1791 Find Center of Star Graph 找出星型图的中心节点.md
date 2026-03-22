# 1791 Find Center of Star Graph 找出星型图的中心节点

__Description:__

There is an undirected __star__ graph consisting of `n` nodes labeled from `1` to `n`. A star graph is a graph where there is one __center__ node and __exactly__ `n - 1` edges that connect the center node with every other node.

You are given a 2D integer array `edges` where each `edges[i] = [ui, vi]` indicates that there is an edge between the nodes `ui` and `vi`. Return the center of the given star graph.

__Example:__

Example 1:

![1791-1](https://assets.leetcode.com/uploads/2021/02/24/star_graph.png)

```text
Input: edges = [[1,2],[2,3],[4,2]]
Output: 2
Explanation: As shown in the figure above, node 2 is connected to every other node, so 2 is the center.
```

Example 2:

```text
Input: edges = [[1,2],[5,1],[1,3],[1,4]]
Output: 1
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- The given `edges` represent a valid star graph.

__题目描述:__

有一个无向的 __星型__ 图，由 `n` 个编号从 `1` 到 `n` 的节点组成。星型图有一个 __中心__ 节点，并且恰有 `n - 1` 条边将中心节点与其他每个节点连接起来。

给你一个二维整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示在节点 `ui` 和 `vi` 之间存在一条边。请你找出并返回 `edges` 所表示星型图的中心节点。

__示例:__

示例 1：

![1791-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/14/star_graph.png)

```text
输入：edges = [[1,2],[2,3],[4,2]]
输出：2
解释：如上图所示，节点 2 与其他每个节点都相连，所以节点 2 是中心节点。
```

示例 2：

```text
输入：edges = [[1,2],[5,1],[1,3],[1,4]]
输出：1
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- 题目数据给出的 `edges` 表示一个有效的星型图

__思路:__

```text
模拟
因为中心节点会在每一个边中出现 1 次
所以只需要判断前两条边出现 2 次的结点即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findCenter(vector<vector<int>>& edges) 
    {
        return edges.front().front() == edges.back().front() or edges.front().front() == edges.back().back() ? edges.front().front() : edges.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int findCenter(int[][] edges) {
        return edges[0][0] == edges[1][0] || edges[0][0] == edges[1][1] ? edges[0][0] : edges[0][1];
    }
}
```

__Python__:

```Python
class Solution:
    def findCenter(self, edges: List[List[int]]) -> int:
        return edges[0][0] if edges[0][0] in edges[1] else edges[0][1]
```
