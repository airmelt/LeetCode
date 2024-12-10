# 2368 Reachable Nodes With Restrictions 受限条件下可到达节点的数目

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree. You are also given an integer array `restricted` which represents __restricted__ nodes.

Return _the __maximum__ number of nodes you can reach from node_ `0` _without visiting a restricted node._

Note that node `0` will __not__ be a restricted node.

__Example:__

Example 1:

![2368-1](https://assets.leetcode.com/uploads/2022/06/15/ex1drawio.png)

```text
Input: n = 7, edges = [[0,1],[1,2],[3,1],[4,0],[0,5],[5,6]], restricted = [4,5]
Output: 4
Explanation: The diagram above shows the tree.
We have that [0,1,2,3] are the only nodes that can be reached from node 0 without visiting a restricted node.
```

Example 2:

![2368-2](https://assets.leetcode.com/uploads/2022/06/15/ex2drawio.png)

```text
Input: n = 7, edges = [[0,1],[0,2],[0,5],[0,4],[3,2],[6,5]], restricted = [4,2,1]
Output: 3
Explanation: The diagram above shows the tree.
We have that [0,5,6] are the only nodes that can be reached from node 0 without visiting a restricted node.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.
- `1 <= restricted.length < n`
- `1 <= restricted[i] < n`
- All the values of `restricted` are __unique__.

__题目描述:__

现有一棵由 `n` 个节点组成的无向树，节点编号从 `0` 到 `n - 1` ，共有 `n - 1` 条边。

给你一个二维整数数组 `edges` ，长度为 `n - 1` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间存在一条边。另给你一个整数数组 `restricted` 表示 __受限__ 节点。

在不访问受限节点的前提下，返回你可以从节点 `0` 到达的 __最多__ 节点数目_。_

注意，节点 `0` __不__ 会标记为受限节点。

__示例:__

示例 1：

![2368-3](https://assets.leetcode.com/uploads/2022/06/15/ex1drawio.png)

```text
输入：n = 7, edges = [[0,1],[1,2],[3,1],[4,0],[0,5],[5,6]], restricted = [4,5]
输出：4
解释：上图所示正是这棵树。
在不访问受限节点的前提下，只有节点 [0,1,2,3] 可以从节点 0 到达。
```

示例 2：

![2368-4](https://assets.leetcode.com/uploads/2022/06/15/ex2drawio.png)

```text
输入：n = 7, edges = [[0,1],[0,2],[0,5],[0,4],[3,2],[6,5]], restricted = [4,2,1]
输出：3
解释：上图所示正是这棵树。
在不访问受限节点的前提下，只有节点 [0,5,6] 可以从节点 0 到达。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵有效的树
- `1 <= restricted.length < n`
- `1 <= restricted[i] < n`
- `restricted` 中的所有值 __互不相同__

__思路:__

```text
DFS
先将 restricted 转化为 set
将 edges 转化为邻接表 graph
从节点 0 开始 DFS
访问所有能到达的节点
并将访问过的节点加入 visited
如果节点 v 不在 restricted 中且没有被访问过，则继续 DFS
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int reachableNodes(int n, vector<vector<int>>& edges, vector<int>& restricted) 
    {
        s = unordered_set<int>(restricted.begin(), restricted.end());
        for (const auto& edge : edges)
        {
            graph[edge.front()].insert(edge.back());
            graph[edge.back()].insert(edge.front());
        }
        dfs(0);
        return visited.size();
    }
private:
    unordered_map<int, unordered_set<int>> graph;
    unordered_set<int> s, visited;

    void dfs(int u) 
    {
        visited.insert(u);
        for (const auto& v : graph[u]) if (!s.count(v) and !visited.count(v)) dfs(v);
    }
};
```

__Java__:

```Java
class Solution {
    private Set<Integer> set, visited = new HashSet<>();
    private Map<Integer, Set<Integer>> graph = new HashMap<>();

    public int reachableNodes(int n, int[][] edges, int[] restricted) {
        set = Arrays.stream(restricted).boxed().collect(Collectors.toSet());
        for (int[] edge : edges) {
            graph.computeIfAbsent(edge[0], k -> new HashSet<>()).add(edge[1]);            
            graph.computeIfAbsent(edge[1], k -> new HashSet<>()).add(edge[0]);
        }
        dfs(0);
        return visited.size();
    }

    private void dfs(int u) {
        visited.add(u);
        for (int v : graph.get(u)) if (!set.contains(v) && !visited.contains(v)) dfs(v);
    }
}
```

__Python__:

```Python
class Solution:
    def reachableNodes(self, n: int, edges: List[List[int]], restricted: List[int]) -> int:
        graph, restricted, visited = defaultdict(set), set(restricted), set()
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        
        def dfs(u: int) -> NoReturn:
            visited.add(u)
            for v in graph[u]:
                if v not in restricted and v not in visited:
                    dfs(v)
        dfs(0)
        return len(visited)
```
