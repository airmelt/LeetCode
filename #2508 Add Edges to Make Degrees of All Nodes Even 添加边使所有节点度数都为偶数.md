# 2508 Add Edges to Make Degrees of All Nodes Even 添加边使所有节点度数都为偶数

__Description:__

There is an __undirected__ graph consisting of `n` nodes numbered from `1` to `n`. You are given the integer `n` and a __2D__ array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi`. The graph can be disconnected.

You can add at most two additional edges (possibly none) to this graph so that there are no repeated edges and no self-loops.

Return `true` _if it is possible to make the degree of each node in the graph even, otherwise return_ `false`_._

The degree of a node is the number of edges connected to it.

__Example:__

Example 1:

![2508-1](https://assets.leetcode.com/uploads/2022/10/26/agraphdrawio.png)

```text
Input: n = 5, edges = [[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]
Output: true
Explanation: The above diagram shows a valid way of adding an edge.
Every node in the resulting graph is connected to an even number of edges.
```

Example 2:

![2508-2](https://assets.leetcode.com/uploads/2022/10/26/aagraphdrawio.png)

```text
Input: n = 4, edges = [[1,2],[3,4]]
Output: true
Explanation: The above diagram shows a valid way of adding two edges.
```

Example 3:

![2508-3](https://assets.leetcode.com/uploads/2022/10/26/aaagraphdrawio.png)

```text
Input: n = 4, edges = [[1,2],[1,3],[1,4]]
Output: false
Explanation: It is not possible to obtain a valid graph with adding at most 2 edges.
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `2 <= edges.length <= 10 ^ 5`
- `edges[i].length == 2`
- `1 <= ai, bi <= n`
- `ai != bi`
- There are no repeated edges.

__题目描述:__

给你一个有 `n` 个节点的 __无向__ 图，节点编号为 `1` 到 `n` 。再给你整数 `n` 和一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 和 `bi` 之间有一条边。图不一定连通。

你可以给图中添加 至多 两条额外的边（也可以一条边都不添加），使得图中没有重边也没有自环。

如果添加额外的边后，可以使得图中所有点的度数都是偶数，返回 `true` ，否则返回 `false` 。

点的度数是连接一个点的边的数目。

__示例:__

示例 1：

![2508-4](https://assets.leetcode.com/uploads/2022/10/26/agraphdrawio.png)

```text
输入：n = 5, edges = [[1,2],[2,3],[3,4],[4,2],[1,4],[2,5]]
输出：true
解释：上图展示了添加一条边的合法方案。
最终图中每个节点都连接偶数条边。
```

示例 2：

![2508-5](https://assets.leetcode.com/uploads/2022/10/26/aagraphdrawio.png)

```text
输入：n = 4, edges = [[1,2],[3,4]]
输出：true
解释：上图展示了添加两条边的合法方案。
```

示例 3：

![2508-6](https://assets.leetcode.com/uploads/2022/10/26/aaagraphdrawio.png)

```text
输入：n = 4, edges = [[1,2],[1,3],[1,4]]
输出：false
解释：无法添加至多 2 条边得到一个符合要求的图。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `2 <= edges.length <= 10 ^ 5`
- `edges[i].length == 2`
- `1 <= ai, bi <= n`
- `ai != bi`
- 图中不会有重边

__思路:__

```text
分类讨论
先将所有的边加入到图中
遍历所有的节点, 找到所有度数为奇数的节点
如果没有奇数节点, 直接返回 true
如果有两个奇数节点, 先判断这两个节点之间是否有边
如果没有边, 直接返回 true
如果有边, 遍历所有的节点, 找到一个节点, 使得这个节点和这两个奇数节点都没有边
如果找到了, 直接返回 true
如果有四个奇数节点, 先判断这四个节点之间是否有边
设为 a, b, c, d
如果 a 和 b 之间没有边, c 和 d 之间没有边, 直接返回 true
如果 a 和 c 之间没有边, b 和 d 之间没有边, 直接返回 true
如果 a 和 d 之间没有边, c 和 b 之间没有边, 直接返回 true
其他情况直接返回 false
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), 其中 M 为边的数量, N 为节点的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isPossible(int n, vector<vector<int>>& edges) 
    {
        vector<unordered_set<int>> graph(n);
        for (const auto& edge : edges) 
        {
            graph[edge.front() - 1].insert(edge.back() - 1);
            graph[edge.back() - 1].insert(edge.front() - 1);
        }
        vector<int> odd;
        for (int i = 0; i < n; i++) if ((graph[i].size() & 1)) odd.emplace_back(i);
        int m = odd.size();
        if (m == 0) return true;
        else if (m == 2) 
        {
            int x = odd.front(), y = odd.back();
            if (!graph[x].count(y)) return true;
            for (int i = 0; i < n; i++) if (i != x and i != y and !graph[x].count(i) and !graph[y].count(i)) return true;
        }
        else if (m == 4) 
        {
            int a = odd[0], b = odd[1], c = odd[2], d = odd[3];
            return !graph[a].count(b) and !graph[c].count(d) or !graph[a].count(c) and !graph[b].count(d) or !graph[a].count(d) and !graph[c].count(b);
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPossible(int n, List<List<Integer>> edges) {
        var graph = new Set[n];
        Arrays.setAll(graph, e -> new HashSet<Integer>());
        for (var edge : edges) {
            graph[edge.get(0) - 1].add(edge.get(1) - 1);
            graph[edge.get(1) - 1].add(edge.get(0) - 1);
        }
        List<Integer> odd = new ArrayList<Integer>();
        for (int i = 0; i < n; i++) if ((graph[i].size() & 1) != 0) odd.add(i);
        int m = odd.size();
        if (m == 0) return true;
        else if (m == 2) {
            int x = odd.get(0), y = odd.get(1);
            if (!graph[x].contains(y)) return true;
            for (int i = 0; i < n; i++) if (i != x && i != y && !graph[x].contains(i) && !graph[y].contains(i)) return true;
        }
        else if (m == 4) {
            int a = odd.get(0), b = odd.get(1), c = odd.get(2), d = odd.get(3);
            return !graph[a].contains(b) && !graph[c].contains(d) || !graph[a].contains(c) && !graph[b].contains(d) || !graph[a].contains(d) && !graph[c].contains(b);
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isPossible(self, n: int, edges: List[List[int]]) -> bool:
        graph = defaultdict(set)
        for u, v in edges:
            graph[u - 1].add(v - 1)
            graph[v - 1].add(u - 1)
        return not (m := len(odd := [u for u in range(n) if len(graph[u]) & 1])) or (m == 2 and (odd[0] not in graph[odd[1]] or any(u != odd[0] and u != odd[1] and u not in graph[odd[0]] and u not in graph[odd[1]] for u in range(n)))) or (m == 4 and ((odd[1] not in graph[odd[0]] and odd[3] not in graph[odd[2]]) or (odd[2] not in graph[odd[0]] and odd[3] not in graph[odd[1]]) or (odd[3] not in graph[odd[0]] and odd[1] not in graph[odd[2]])))
```
