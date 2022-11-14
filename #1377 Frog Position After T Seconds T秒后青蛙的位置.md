# 1377 Frog Position After T Seconds T秒后青蛙的位置

__Description:__

Given an undirected tree consisting of n vertices numbered from 1 to n. A frog starts jumping from vertex 1. In one second, the frog jumps from its current vertex to another unvisited vertex if they are directly connected. The frog can not jump back to a visited vertex. In case the frog can jump to several vertices, it jumps randomly to one of them with the same probability. Otherwise, when the frog can not jump to any unvisited vertex, it jumps forever on the same vertex.

The edges of the undirected tree are given in the array edges, where edges[i] = [ai, bi] means that exists an edge connecting the vertices ai and bi.

Return the probability that after t seconds the frog is on the vertex target. Answers within 10-5 of the actual answer will be accepted.

__Example:__

Example 1:

![1377-1](https://assets.leetcode.com/uploads/2021/12/21/frog1.jpg)

Input: n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 2, target = 4
Output: 0.16666666666666666
Explanation: The figure above shows the given graph. The frog starts at vertex 1, jumping with 1/3 probability to the vertex 2 after second 1 and then jumping with 1/2 probability to vertex 4 after second 2. Thus the probability for the frog is on the vertex 4 after 2 seconds is 1/3 * 1/2 = 1/6 = 0.16666666666666666.

Example 2:

![1377-2](https://assets.leetcode.com/uploads/2021/12/21/frog2.jpg)

Input: n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 1, target = 7
Output: 0.3333333333333333
Explanation: The figure above shows the given graph. The frog starts at vertex 1, jumping with 1/3 = 0.3333333333333333 probability to the vertex 7 after second 1.

__Constraints:__

1 <= n <= 100
edges.length == n - 1
edges[i].length == 2
1 <= ai, bi <= n
1 <= t <= 50
1 <= target <= n

__描述:__

给你一棵由 n 个顶点组成的无向树，顶点编号从 1 到 n。青蛙从 顶点 1 开始起跳。规则如下：

在一秒内，青蛙从它所在的当前顶点跳到另一个 未访问 过的顶点（如果它们直接相连）。
青蛙无法跳回已经访问过的顶点。
如果青蛙可以跳到多个不同顶点，那么它跳到其中任意一个顶点上的机率都相同。
如果青蛙不能跳到任何未访问过的顶点上，那么它每次跳跃都会停留在原地。

无向树的边用数组 edges 描述，其中 edges[i] = [fromi, toi] 意味着存在一条直接连通 fromi 和 toi 两个顶点的边。

返回青蛙在 t 秒后位于目标顶点 target 上的概率。

__示例:__

示例 1：

![1377-3](https://assets.leetcode.com/uploads/2021/12/21/frog1.jpg)

输入：n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 2, target = 4
输出：0.16666666666666666
解释：上图显示了青蛙的跳跃路径。青蛙从顶点 1 起跳，第 1 秒 有 1/3 的概率跳到顶点 2 ，然后第 2 秒 有 1/2 的概率跳到顶点 4，因此青蛙在 2 秒后位于顶点 4 的概率是 1/3 * 1/2 = 1/6 = 0.16666666666666666 。

示例 2：

![1377-4](https://assets.leetcode.com/uploads/2021/12/21/frog2.jpg)

输入：n = 7, edges = [[1,2],[1,3],[1,7],[2,4],[2,6],[3,5]], t = 1, target = 7
输出：0.3333333333333333
解释：上图显示了青蛙的跳跃路径。青蛙从顶点 1 起跳，有 1/3 = 0.3333333333333333 的概率能够 1 秒 后跳到顶点 7 。

__提示：__

1 <= n <= 100
edges.length == n - 1
edges[i].length == 2
1 <= ai, bi <= n
1 <= t <= 50
1 <= target <= n

__思路:__

```text
DFS
用 visited 数组记录已经访问过的点
先将 edges 转化为无向图
从 1(下标为 0) 出发搜索
如果搜索步数小于 0 且已经搜索到目标更新结果为当前的概率的最大值
否则搜索步数小于 0 终止搜索
否则统计还能跳的点, 如果只能在原地, 剪枝并更新结果
否则跳到下一个还能去的点并更新概率
时间复杂度为 O(n), 空间复杂度为 O(n)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    double result;
    unordered_map<int, set<int>> graph;
​
    void dfs(int cur, double p, int step, int target, vector<bool>& visited) 
    {
        if (step < 0 and target == cur) 
        {
            result = max(result, p);
            return;
        }
        if (step < 0) return;
        int count = 0;
        for (const auto& i : graph[cur]) if (!visited[i]) ++count;
        if (!count and target == cur) 
        {
            result = max(result, p);
            return;
        }
        if (!count) return;
        for (const auto& i : graph[cur])
        {
            if (!visited[i]) 
            {
                visited[i] = true;
                dfs(i, 1.0 / count * p, step - 1, target, visited);
            }
        }
    }
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) 
    {
        result = 0.0;
        vector<bool> visited(n);
        visited.front() = true;
        for (const auto& edge : edges)
        {
            graph[edge.front() - 1].insert(edge.back() - 1);
            graph[edge.back() - 1].insert(edge.front() - 1);
        }
        dfs(0, 1.0, t - 1, target - 1, visited);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private double result = 0.0;
    private Map<Integer, List<Integer>> graph = new HashMap<>();
​
    public double frogPosition(int n, int[][] edges, int t, int target) {
        boolean[] visited = new boolean[n];
        visited[0] = true;
        for (int i = 0; i < n; i++) graph.put(i, new ArrayList<>());
        for (int[] edge : edges) {
            graph.get(edge[0] - 1).add(edge[1] - 1);
            graph.get(edge[1] - 1).add(edge[0] - 1);
        }
        dfs(0, 1.0, t - 1, target - 1, visited);
        return result;
    }
​
    private void dfs(int cur, double p, int step, int target, boolean[] visited) {
        if (step < 0 && target == cur) {
            result = Math.max(result, p);
            return;
        }
        if (step < 0) return;
        int count = 0;
        for (int i : graph.get(cur)) if (!visited[i]) ++count;
        if (count == 0 && target == cur) {
            result = Math.max(result, p);
            return;
        }
        if (count == 0) return;
        for (int i : graph.get(cur)) {
            if (!visited[i]) {
                visited[i] = true;
                dfs(i, 1.0 / count * p, step - 1, target, visited);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def frogPosition(self, n: int, edges: List[List[int]], t: int, target: int) -> float:
        result, visited, graph, target = 0, [True] + [False] * (n - 1), defaultdict(set), target - 1
        for u, v in edges:
            graph[u - 1].add(v - 1)
            graph[v - 1].add(u - 1)
        
        def dfs(cur: int, p: float, step: int) -> None:
            nonlocal result
            if step < 0 and target == cur:
                result = max(result, p)
                return
            if step < 0:
                return
            if not (count := sum(not visited[i] for i in graph[cur])) and target == cur:
                result = max(result, p)
                return
            if not count:
                return
            for i in graph[cur]:
                if not visited[i]:
                    visited[i] = True
                    dfs(i, 1 / count * p, step - 1)
        dfs(0, 1.0, t - 1)
        return result
```
