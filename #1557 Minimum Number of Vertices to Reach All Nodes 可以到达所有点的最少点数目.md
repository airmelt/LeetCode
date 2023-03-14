# 1557 Minimum Number of Vertices to Reach All Nodes 可以到达所有点的最少点数目

__Description:__

Given a __directed acyclic graph__, with `n` vertices numbered from `0` to `n-1`, and an array `edges` where `edges[i] = [fromi, toi]` represents a directed edge from node `fromi` to node `toi`.

Find the smallest set of vertices from which all nodes in the graph are reachable. It's guaranteed that a unique solution exists.

Notice that you can return the vertices in any order.

__Example:__

Example 1:

![1557-1](https://assets.leetcode.com/uploads/2020/07/07/untitled22.png)

```text
Input: n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
Output: [0,3]
Explanation: It's not possible to reach all the nodes from a single vertex. From 0 we can reach [0,1,2,5]. From 3 we can reach [3,4,2,5]. So we output [0,3].
```

Example 2:

![1557-2](https://assets.leetcode.com/uploads/2020/07/07/untitled.png)

```text
Input: n = 5, edges = [[0,1],[2,1],[3,1],[1,4],[2,4]]
Output: [0,2,3]
Explanation: Notice that vertices 0, 3 and 2 are not reachable from any other node, so we must include them. Also any of these vertices can reach nodes 1 and 4.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= edges.length <= min(10 ^ 5, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi < n`
- All pairs `(fromi, toi)` are distinct.

__题目描述:__

给你一个 __有向无环图__ ， `n` 个节点编号为 `0` 到 `n-1` ，以及一个边数组 `edges` ，其中 `edges[i] = [fromi, toi]` 表示一条从点  `fromi` 到点 `toi` 的有向边。

找到最小的点集使得从这些点出发能到达图中所有点。题目保证解存在且唯一。

你可以以任意顺序返回这些节点编号。

__示例:__

示例 1：

![1557-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5480e1.png)

```text
输入：n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
输出：[0,3]
解释：从单个节点出发无法到达所有节点。从 0 出发我们可以到达 [0,1,2,5] 。从 3 出发我们可以到达 [3,4,2,5] 。所以我们输出 [0,3] 。
```

示例 2：

![1557-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5480e2.png)

```text
输入：n = 5, edges = [[0,1],[2,1],[3,1],[1,4],[2,4]]
输出：[0,2,3]
解释：注意到节点 0，3 和 2 无法从其他节点到达，所以我们必须将它们包含在结果点集中，这些点都能到达节点 1 和 4 。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `1 <= edges.length <= min(10 ^ 5, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi < n`
- 所有点对 `(fromi, toi)` 互不相同。

__思路:__

```text
拓扑排序
寻找入度为 0 的点即可
遍历边数组, 将所有边的终点设置为访问过
没访问过的就是起点
时间复杂度为 O(N + M), 空间复杂度为 O(N), N 为节点数, M 为边数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) 
    {
        vector<int> result, visited(n);
        for (const auto& edge: edges) visited[edge.back()] = 1;
        for (int i = 0; i < n; i++) if (!visited[i]) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findSmallestSetOfVertices(int n, List<List<Integer>> edges) {
        boolean visited[] = new boolean[n];
        for (List<Integer> edge: edges) visited[edge.get(1)] = true;
        return IntStream.range(0, n).filter(i -> !visited[i]).boxed().collect(Collectors.toList()); 
    }
}
```

__Python__:

```Python
class Solution:
    def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
        return list({x for x, y in edges} - {y for x, y in edges})
```
