# 1129 Shortest Path with Alternating Colors 颜色交替的最短路径

__Description__:
You are given an integer n, the number of nodes in a directed graph where the nodes are labeled from 0 to n - 1. Each edge is red or blue in this graph, and there could be self-edges and parallel edges.

You are given two arrays redEdges and blueEdges where:

redEdges[i] = [ai, bi] indicates that there is a directed red edge from node ai to node bi in the graph, and
blueEdges[j] = [uj, vj] indicates that there is a directed blue edge from node uj to node vj in the graph.
Return an array answer of length n, where each answer[x] is the length of the shortest path from node 0 to node x such that the edge colors alternate along the path, or -1 if such a path does not exist.

__Example:__

Example 1:

Input: n = 3, redEdges = [[0,1],[1,2]], blueEdges = []
Output: [0,1,-1]

Example 2:

Input: n = 3, redEdges = [[0,1]], blueEdges = [[2,1]]
Output: [0,1,-1]

__Constraints:__

1 <= n <= 100
0 <= redEdges.length, blueEdges.length <= 400
redEdges[i].length == blueEdges[j].length == 2
0 <= ai, bi, uj, vj < n

__题目描述__:
在一个有向图中，节点分别标记为 0, 1, ..., n-1。图中每条边为红色或者蓝色，且存在自环或平行边。

red_edges 中的每一个 [i, j] 对表示从节点 i 到节点 j 的红色有向边。类似地，blue_edges 中的每一个 [i, j] 对表示从节点 i 到节点 j 的蓝色有向边。

返回长度为 n 的数组 answer，其中 answer[X] 是从节点 0 到节点 X 的红色边和蓝色边交替出现的最短路径的长度。如果不存在这样的路径，那么 answer[x] = -1。

__示例 :__

示例 1：

输入：n = 3, red_edges = [[0,1],[1,2]], blue_edges = []
输出：[0,1,-1]

示例 2：

输入：n = 3, red_edges = [[0,1]], blue_edges = [[2,1]]
输出：[0,1,-1]

示例 3：

输入：n = 3, red_edges = [[1,0]], blue_edges = [[2,1]]
输出：[0,-1,-1]

示例 4：

输入：n = 3, red_edges = [[0,1]], blue_edges = [[1,2]]
输出：[0,1,2]

示例 5：

输入：n = 3, red_edges = [[0,1],[0,2]], blue_edges = [[1,0]]
输出：[0,1,1]

__提示:__

1 <= n <= 100
red_edges.length <= 400
blue_edges.length <= 400
red_edges[i].length == blue_edges[i].length == 2
0 <= red_edges[i][j], blue_edges[i][j] < n

__思路__:

BFS
用两条路径分别记录红色和蓝色, 红色记为 0, 蓝色记为 1
从 0 出发分别按照红色和蓝色路径搜索
下一条路径切换颜色可以用异或运算
用 visited 数组记录已经访问过的结点, 需要跳过访问过的结点
将访问过的结点作为另一种颜色的起点加入队列
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) 
    {
        vector<vector<vector<int>>> edges(2, vector<vector<int>>(n));
        for (const auto& r : redEdges) edges.front()[r.front()].emplace_back(r.back());
        for (const auto& b : blueEdges) edges.back()[b.front()].emplace_back(b.back());
        vector<int> result(n, -1);
        int cur = 1;
        result.front() = 0;
        vector<vector<bool>> visited(2, vector<bool>(n));
        queue<vector<int>> q;
        q.push(vector<int>{0, 0});
        q.push(vector<int>{0, 1});
        while (!q.empty())
        {
            for (int i = 0, size = q.size(); i < size; i++)
            {
                vector<int> pos = q.front();
                q.pop();
                for (const auto& next : edges[pos.back()][pos.front()])
                {
                    if (visited[pos.back() ^ 1][next]) continue;
                    visited[pos.back() ^ 1][next] = true;
                    if (result[next] == -1) result[next] = cur;
                    q.push(vector<int>{next, pos.back() ^ 1});
                }
            }
            ++cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] shortestAlternatingPaths(int n, int[][] redEdges, int[][] blueEdges) {
        List<Integer>[][] edges = new List[2][n];
        for (int i = 0; i < n; i++) {
            edges[0][i] = new ArrayList();
            edges[1][i] = new ArrayList();
        }
        for (int[] r : redEdges) edges[0][r[0]].add(r[1]);
        for (int[] b : blueEdges) edges[1][b[0]].add(b[1]);
        Deque<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{0, 0});
        queue.offer(new int[]{0, 1});
        int cur = 1, result[] = new int[n];
        Arrays.fill(result, -1);
        result[0] = 0;
        boolean[][] visited = new boolean[2][n];
        while (!queue.isEmpty()) {
            for (int i = 0, size = queue.size(); i < size; i++) {
                int[] pos = queue.poll();
                for (int next : edges[pos[1]][pos[0]]) {
                    if (visited[pos[1] ^ 1][next]) continue;
                    visited[pos[1] ^ 1][next] = true;
                    if (result[next] == -1) result[next] = cur;
                    queue.offer(new int[]{next, pos[1] ^ 1});
                }
            }
            ++cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestAlternatingPaths(self, n: int, redEdges: List[List[int]], blueEdges: List[List[int]]) -> List[int]:
        edges, queue, result, visited, cur = [[[] for _ in range(n)] for _ in range(2)], [[0, 0], [0, 1]], [0] + [-1] * (n - 1), [[False] * n for _ in range(2)], 1
        for r in redEdges:
            edges[0][r[0]].append(r[1])
        for b in blueEdges:
            edges[1][b[0]].append(b[1])
        while queue:
            size = len(queue)
            for _ in range(size):
                pos = queue.pop(0)
                for next in edges[pos[1]][pos[0]]:
                    if visited[pos[1] ^ 1][next]:
                        continue
                    visited[pos[1] ^ 1][next] = True
                    if result[next] == -1:
                        result[next] = cur
                    queue.append([next, pos[1] ^ 1])
            cur += 1
        return result
```
