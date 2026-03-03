# 2924 Find Champion II 找到冠军 II

__Description:__

There are `n` teams numbered from `0` to `n - 1` in a tournament; each team is also a node in a __DAG__.

You are given the integer `n` and a __0-indexed__ 2D integer array `edges` of length `m` representing the __DAG__, where `edges[i] = [ui, vi]` indicates that there is a directed edge from team `ui` to team `vi` in the graph.

A directed edge from `a` to `b` in the graph means that team `a` is __stronger__ than team `b` and team `b` is __weaker__ than team `a`.

Team `a` will be the __champion__ of the tournament if there is no team `b` that is __stronger__ than team `a`.

Return _the team that will be the __champion__ of the tournament if there is a __unique__ champion, otherwise, return_ `-1`_._

Notes

- A __cycle__ is a series of nodes `a1, a2, ..., an, an+1` such that node `a1` is the same node as node `an+1`, the nodes `a1, a2, ..., an` are distinct, and there is a directed edge from the node `ai` to node `ai+1` for every `i` in the range `[1, n]`.
- A __DAG__ is a directed graph that does not have any __cycle__.

__Example:__

Example 1:

![2924-1](https://assets.leetcode.com/uploads/2023/10/19/graph-3.png)

```text
Input: n = 3, edges = [[0,1],[1,2]]
Output: 0
Explanation: Team 1 is weaker than team 0. Team 2 is weaker than team 1. So the champion is team 0.
```

Example 2:

![2924-2](https://assets.leetcode.com/uploads/2023/10/19/graph-4.png)

```text
Input: n = 4, edges = [[0,2],[1,3],[1,2]]
Output: -1
Explanation: Team 2 is weaker than team 0 and team 1. Team 3 is weaker than team 1. But team 1 and team 0 are not weaker than any other teams. So the answer is -1.
```

__Constraints:__

- `1 <= n <= 100`
- `m == edges.length`
- `0 <= m <= n * (n - 1) / 2`
- `edges[i].length == 2`
- `0 <= edge[i][j] <= n - 1`
- `edges[i][0] != edges[i][1]`
- The input is generated such that if team `a` is stronger than team `b`, team `b` is not stronger than team `a`.
- The input is generated such that if team `a` is stronger than team `b` and team `b` is stronger than team `c`, then team `a` is stronger than team `c`.

__题目描述:__

一场比赛中共有 `n` 支队伍，按从 `0` 到  `n - 1` 编号。每支队伍也是 __有向无环图（DAG）__ 上的一个节点。

给你一个整数 `n` 和一个下标从 __0__ 开始、长度为 `m` 的二维整数数组 `edges` 表示这个有向无环图，其中 `edges[i] = [ui, vi]` 表示图中存在一条从 `ui` 队到 `vi` 队的有向边。

从 `a` 队到 `b` 队的有向边意味着 `a` 队比 `b` 队 __强__ ，也就是 `b` 队比 `a` 队 __弱__ 。

在这场比赛中，如果不存在某支强于 `a` 队的队伍，则认为 `a` 队将会是 __冠军__ 。

如果这场比赛存在 __唯一__ 一个冠军，则返回将会成为冠军的队伍。否则，返回 `-1` _。_

注意

- __环__ 是形如 `a1, a2, ..., an, an+1` 的一个序列，且满足:节点 `a1` 与节点 `an+1` 是同一个节点；节点 `a1, a2, ..., an` 互不相同；对于范围 `[1, n]` 中的每个 `i` ，均存在一条从节点 `ai` 到节点 `ai+1` 的有向边。
- __有向无环图__ 是不存在任何环的有向图。

__示例:__

示例 1：

![2924-3](https://assets.leetcode.com/uploads/2023/10/19/graph-3.png)

```text
输入：n = 3, edges = [[0,1],[1,2]]
输出：0
解释：1 队比 0 队弱。2 队比 1 队弱。所以冠军是 0 队。
```

示例 2：

![2924-4](https://assets.leetcode.com/uploads/2023/10/19/graph-4.png)

```text
输入：n = 4, edges = [[0,2],[1,3],[1,2]]
输出：-1
解释：2 队比 0 队和 1 队弱。3 队比 1 队弱。但是 1 队和 0 队之间不存在强弱对比。所以答案是 -1 。
```

__提示：__

- `1 <= n <= 100`
- `m == edges.length`
- `0 <= m <= n * (n - 1) / 2`
- `edges[i].length == 2`
- `0 <= edge[i][j] <= n - 1`
- `edges[i][0] != edges[i][1]`
- 生成的输入满足:如果 `a` 队比 `b` 队强，就不存在 `b` 队比 `a` 队强
- 生成的输入满足:如果 `a` 队比 `b` 队强， `b` 队比 `c` 队强，那么 `a` 队比 `c` 队强

__思路:__

```text
模拟
找到所有节点的入度
如果只有一个入度为 0 的节点那就是冠军
否则无法比较返回 -1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findChampion(int n, vector<vector<int>>& edges) 
    {
        vector<int> in_degree(n);
        int result = -1, i = 0;
        for (const auto& edge : edges) ++in_degree[edge.back()];
        for (; i < n && result == -1; i++) if (!in_degree[i]) result = i;
        for (i = n - 1; i > result; i--) if (!in_degree[i] and result != i) return -1;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findChampion(int n, int[][] edges) {
        int inDegree[] = new int[n], result = -1, i = 0;
        for (var edge : edges) ++inDegree[edge[1]];
        for (; i < n && result == -1; i++) if (inDegree[i] == 0) result = i;
        for (i = n - 1; i > result; i--) if (inDegree[i] == 0 && result != i) return -1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findChampion(self, n: int, edges: List[List[int]]) -> int:
        return result[0] if ((in_degree := [sum(y == i for _, y in edges) for i in range(n)]) and (result := [i for i in range(n) if not in_degree[i]])) and len(result) == 1 else -1
```
