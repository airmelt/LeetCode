# 847 Shortest Path Visiting All Nodes 访问所有节点的最短路径

__Description__:
You have an undirected, connected graph of n nodes labeled from 0 to n - 1. You are given an array graph where graph[i] is a list of all the nodes connected with node i by an edge.

Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.

__Example:__

Example 1:

![shortest graph 1](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

Input: graph = [[1,2,3],[0],[0],[0]]
Output: 4
Explanation: One possible path is [1,0,2,0,3]

Example 2:

![shortest graph 2](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

Input: graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]
Output: 4
Explanation: One possible path is [0,1,4,2,3]

__Constraints:__

n == graph.length
1 <= n <= 12
0 <= graph[i].length < n
graph[i] does not contain i.
If graph[a] contains b, then graph[b] contains a.
The input graph is always connected.

__题目描述__:
存在一个由 n 个节点组成的无向连通图，图中的节点按从 0 到 n - 1 编号。

给你一个数组 graph 表示这个图。其中，graph[i] 是一个列表，由所有与节点 i 直接相连的节点组成。

返回能够访问所有节点的最短路径的长度。你可以在任一节点开始和停止，也可以多次重访节点，并且可以重用边。

__示例 :__

示例 1：

![最短路径 1](https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg)

输入：graph = [[1,2,3],[0],[0],[0]]
输出：4
解释：一种可能的路径为 [1,0,2,0,3]

示例 2：

![最短路径 2](https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg)

输入：graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]
输出：4
解释：一种可能的路径为 [0,1,4,2,3]

__提示:__

n == graph.length
1 <= n <= 12
0 <= graph[i].length < n
graph[i] 不包含 i
如果 graph[a] 包含 b ，那么 graph[b] 也包含 a
输入的图总是连通图

__思路__:

状态压缩 ➕ BFS
先将所有结点标记, 即当作起点
每次进入循环 result 自增
搜索所有当前结点的邻居结点
用 mask | (1 << neighbor) 表示经过邻居结点
检查 mask 是否为 (1 << n) - 1 判断是否到达终点
时间复杂度为 O(n ^ 2 \* 2 ^ n), 空间复杂度为 O(n \* 2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int shortestPathLength(vector<vector<int>>& graph) 
    {
        int n = graph.size() == 1 ? (graph[0].size() == 0 ? 0 : graph[0].size()) : graph.size(), result = 0;
        queue<pair<int, int>> q;
        vector<vector<bool>> visited(n, vector<bool>(1 << n));
        for (int i = 0; i < n; i++) 
        {
            q.push(make_pair(i, 1 << i));
            visited[i][1 << i] = true;
        }
        while (!q.empty()) 
        {
            ++result;
            for (int i = 0, m = q.size(); i < m; i++) 
            {
                auto [u, mask] = q.front();
                q.pop();
                for (int v : graph[u]) 
                {
                    int next_state = mask | (1 << v);
                    if (next_state == (1 << n) - 1) return result;
                    if (!visited[v][next_state]) 
                    {
                        q.push(make_pair(v, next_state));
                        visited[v][next_state] = true;
                    }
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
    public int shortestPathLength(int[][] graph) {
        int n = graph.length, result = 0;
        Queue<int[]> queue = new LinkedList<int[]>();
        boolean[][] visited = new boolean[n][1 << n];
        for (int i = 0; i < n; ++i) {
            queue.offer(new int[]{ i, 1 << i });
            visited[i][1 << i] = true;
        }
        while (!queue.isEmpty()) {
            ++result;
            for (int i = 0, m = queue.size(); i < m; i++) {
                int cur[] = queue.poll();
                for (int neighbor : graph[cur[0]]) {
                    int nextState = cur[1] | (1 << neighbor);
                    if (nextState == (1 << n) - 1) return result;
                    if (!visited[neighbor][nextState]) {
                        queue.offer(new int[]{ neighbor, nextState });
                        visited[neighbor][nextState] = true;
                    }
                }
            }
        }
        return 0;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestPathLength(self, graph: List[List[int]]) -> int:
        q, visited, n, result = deque((i, 1 << i) for i in range(len(graph))), set((i, (1 << i)) for i in range(len(graph))), len(graph), 0
        while q:
            result += 1
            for _ in range(len(q)):
                cur, cur_state = q.popleft()
                for neighbor in graph[cur]:
                    next_state = cur_state | (1 << neighbor)
                    if next_state == (1 << n) - 1:
                        return result
                    if (neighbor, next_state) not in visited:
                        visited.add((neighbor, next_state))
                        q.append((neighbor, next_state))
        return 0
```
