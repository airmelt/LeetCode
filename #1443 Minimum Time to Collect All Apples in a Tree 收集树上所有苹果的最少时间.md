# 1443 Minimum Time to Collect All Apples in a Tree 收集树上所有苹果的最少时间

__Description:__

Given an undirected tree consisting of `n` vertices numbered from `0` to `n-1`, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. _Return the minimum time in seconds you have to spend to collect all apples in the tree, starting at __vertex 0__ and coming back to this vertex._

The edges of the undirected tree are given in the array `edges`, where `edges[i] = [ai, bi]` means that exists an edge connecting the vertices `ai` and `bi`. Additionally, there is a boolean array `hasApple`, where `hasApple[i] = true` means that vertex `i` has an apple; otherwise, it does not have any apple.

__Example:__

Example 1:

![1443-1](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_1.png)

```text
Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
Output: 8 
Explanation: The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.
```

Example 2:

![1443-2](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_2.png)

```text
Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
Output: 6
Explanation: The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.
```

Example 3:

```text
Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
Output: 0
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai < bi <= n - 1`
- `fromi < toi`
- `hasApple.length == n`

__题目描述:__

给你一棵有 `n` 个节点的无向树，节点编号为 `0` 到 `n-1` ，它们中有一些节点有苹果。通过树上的一条边，需要花费 1 秒钟。你从 __节点 0__ 出发，请你返回最少需要多少秒，可以收集到所有苹果，并回到节点 0 。

无向树的边由 `edges` 给出，其中 `edges[i] = [fromi, toi]` ，表示有一条边连接 `from` 和 `toi` 。除此以外，还有一个布尔数组 `hasApple` ，其中 `hasApple[i] = true` 代表节点 `i` 有一个苹果，否则，节点 `i` 没有苹果。

__示例:__

示例 1：

![1443-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/10/min_time_collect_apple_1.png)

```text
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
输出：8 
解释：上图展示了给定的树，其中红色节点表示有苹果。一个能收集到所有苹果的最优方案由绿色箭头表示。
```

示例 2：

![1443-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/10/min_time_collect_apple_2.png)

```text
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
输出：6
解释：上图展示了给定的树，其中红色节点表示有苹果。一个能收集到所有苹果的最优方案由绿色箭头表示。
```

示例 3：

```text
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
输出：0
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `edges.length == n-1`
- `edges[i].length == 2`
- `0 <= fromi, toi <= n-1`
- `fromi < toi`
- `hasApple.length == n`

__思路:__

```text
DFS
对于任何一个子树的根
1. 其不含苹果, 那么访问这个子树的代价为 0
2. 其含有苹果, 那么访问这个子树的代价为 cur_cost = child_cost + cost, child_cost 为根结点到最深的苹果的代价, cost = 2
每次访问苹果至少为 2, 因为要访问到苹果并返回
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int dfs(int cur, vector<vector<int>>& graph, vector<bool> visited, vector<bool>& hasApple, int cost)
    {
        if (visited[cur]) return 0;
        visited[cur] = true;
        int child = 0;
        for (const auto& neighbor: graph[cur]) child += dfs(neighbor, graph, visited, hasApple, 2);
        return (!hasApple[cur] and !child) ? 0 : child + cost;
    }
public:
    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) 
    {
        vector<vector<int>> graph(n);
        vector<bool> visited(n);
        for (const auto edge: edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        return dfs(0, graph, visited, hasApple, 0);
    }
};
```

__Java__:

```Java
class Solution {
    private List<List<Integer>> graph = new ArrayList<>();
    private boolean[] visited;
    private List<Boolean> hasApple;
    
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        visited = new boolean[n];
        this.hasApple = hasApple;
        for (int[] edge: edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        return dfs(0, 0);
    }
    
    private int dfs(int cur, int cost) {
        if (visited[cur]) return 0;
        visited[cur] = true;
        int child = 0;
        for (int neighbor: graph.get(cur)) child += dfs(neighbor, 2);
        return (!hasApple.get(cur) && child == 0) ? 0 : child + cost;
    }
}
```

__Python__:

```Python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int: 
        visited, graph = [False] * n, [deque() for _ in range(n)]
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        
        def dfs(cur: int, cost: int) -> int:
            if visited[cur]:
                return 0
            visited[cur], child = True, 0
            for neighbor in graph[cur]:
                child += dfs(neighbor, 2)
            return child + cost if hasApple[cur] or child else 0
        return dfs(0, 0)
```
