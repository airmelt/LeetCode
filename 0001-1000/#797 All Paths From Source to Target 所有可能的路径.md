# 797 All Paths From Source to Target 所有可能的路径

__Description__:
Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.

The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).

__Example:__

Example 1:

![paths 1](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

Input: graph = [[1,2],[3],[3],[]]
Output: [[0,1,3],[0,2,3]]
Explanation: There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.

Example 2:

![paths 2](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

Input: graph = [[4,3,1],[3,2,4],[3],[4],[]]
Output: [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]

Example 3:

Input: graph = [[1],[]]
Output: [[0,1]]

Example 4:

Input: graph = [[1,2,3],[2],[3],[]]
Output: [[0,1,2,3],[0,2,3],[0,3]]

Example 5:

Input: graph = [[1,3],[2],[3],[]]
Output: [[0,1,2,3],[0,3]]

__Constraints:__

n == graph.length
2 <= n <= 15
0 <= graph[i][j] < n
graph[i][j] != i (i.e., there will be no self-loops).
All the elements of graph[i] are unique.
The input graph is guaranteed to be a DAG.

__题目描述__:
给一个有 n 个结点的有向无环图，找到所有从 0 到 n-1 的路径并输出（不要求按顺序）

二维数组的第 i 个数组中的单元都表示有向图中 i 号结点所能到达的下一些结点（译者注：有向图是有方向的，即规定了 a→b 你就不能从 b→a ）空就是没有下一个结点了。

__示例 :__

示例 1：

![路径 1](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3

示例 2：

![路径 2](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]

示例 3：

输入：graph = [[1],[]]
输出：[[0,1]]

示例 4：

输入：graph = [[1,2,3],[2],[3],[]]
输出：[[0,1,2,3],[0,2,3],[0,3]]

示例 5：

输入：graph = [[1,3],[2],[3],[]]
输出：[[0,1,2,3],[0,3]]

__提示:__

n == graph.length
2 <= n <= 15
0 <= graph[i][j] < n
graph[i][j] != i
保证输入为有向无环图 (GAD)

__思路__:

DFS
按照回溯的方法依次遍历所有的边
时间复杂度为 O(2 ^ n * n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) 
    {
        vector<vector<int>> result;
        vector<int> path(1);
        dfs(graph, result, path, 0);
        return result;
    }
private:
    void dfs(vector<vector<int>>& graph, vector<vector<int>>& result, vector<int>& path, int start)
    {
        for (int d : graph[start]) 
        {
            path.emplace_back(d);
            if (d == graph.size() - 1) result.emplace_back(path);
            else dfs(graph, result, path, d);
            path.pop_back();
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        path.add(0);
        dfs(graph, result, path, 0);
        return result;
    }
    
    private void dfs(int[][] graph, List<List<Integer>> result, List<Integer> path, int start) {
        for (int d : graph[start]) {
            path.add(d);
            if (d == graph.length - 1) result.add(new ArrayList<>(path));
            else dfs(graph, result, path, d);
            path.remove(path.size() - 1);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        result, path, n = [], [0], len(graph)
        
        def dfs(start: int) -> None:
            for d in graph[start]:
                path.append(d)
                (d == n - 1 and result.append(path[:])) or dfs(d)
                path.pop()
                
        dfs(0)
        return result
```
