# 323 Number of Connected Components in an Undirected Graph 无向图中连通分量的数目

__Description:__

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.

Return the number of connected components in the graph.

__Example:__

Example 1:

![323-1](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

```text
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

Example 2:

![323-2](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

```text
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

__Constraints:__

- `1 <= n <= 2000`
- `1 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai <= bi < n`
- `ai != bi`
- There are no repeated edges.

__题目描述:__

你有一个包含 `n` 个节点的图。给定一个整数 `n` 和一个数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示图中 `ai` 和 `bi` 之间有一条边。

返回 图中已连接分量的数目 。

__示例:__

示例 1:

![323-3](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

```text
__输入:__ `n = 5`, `edges = [[0, 1], [1, 2], [3, 4]]`
__输出:__ 2
```

示例 2:

![323-4](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

```text
__输入:__ `n = 5,` `edges = [[0,1], [1,2], [2,3], [3,4]]`
__输出: __ 1
```

__提示：__

- `1 <= n <= 2000`
- `1 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai <= bi < n`
- `ai != bi`
- `edges` 中不会出现重复的边

__思路:__

```text
1. 并查集
将相邻的边连接在并查集中
统计一共有几个并查集即可
时间复杂度为 O(M), 空间复杂度为 O(N)
2. DFS
先建立图
遍历图, 每次遍历到一个新的结点, 说明遇到了一个连通分量
时间复杂度为 O(M), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    vector<int> parent;
    
    int find(int p) 
    {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }
public:
    int countComponents(int n, vector<vector<int>>& edges) 
    {
        parent.resize(n);
        iota(parent.begin(), parent.end(), 0);
        int p = 0, q = 0, count = 0, t = 0;
        for (const auto& edge : edges) 
        {
            if ((p = find(edge.front())) != (q = find(edge.back()))) 
            {
                ++count;
                parent[p] = q;
            }
        }
        return n - count;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;
    public int countComponents(int n, int[][] edges) {
        parent = new int[n];
        int p = 0, q = 0, count = 0;
        for (int i = 1; i < n; i++) parent[i] = i;
        for (int[] edge : edges) {
            if ((p = find(edge[0])) != (q = find(edge[1]))) {
                ++count;
                parent[p] = q;
            }
        }
        return n - count;
    }
    
    private int find(int p) {
        return parent[p] == p ? p : (parent[p] = find(parent[p]));
    }
}
```

__Python__:

```Python
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        result, graph, visited = 0, defaultdict(set), [False] * n
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        
        def dfs(start: int) -> NoReturn:
            visited[start] = True
            for v in graph[start]:
                if not visited[v]:
                    dfs(v)
        for i in range(n):
            if not visited[i]:
                dfs(i)
                result += 1
        return result
```
