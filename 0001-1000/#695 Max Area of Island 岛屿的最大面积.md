# 695 Max Area of Island 岛屿的最大面积

__Description__:
You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

__Example:__

Example 1:

![maxarea](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.

Example 2:

Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 50
grid[i][j] is either 0 or 1.

__题目描述__:
给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

__示例 :__

示例 1:

```text
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。

示例 2:

[[0,0,0,0,0,0,0,0]]
对于上面这个给定的矩阵, 返回 0。

__注意:__
给定的矩阵grid 的长度和宽度都不超过 50。

__思路__:

DFS
向 4 个方向搜索 grid[i][j] == 1 的总和, 更新结果的最大值
时间复杂度为 O(mn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<pair<int, int>> d{make_pair(-1, 0), make_pair(1, 0), make_pair(0, 1), make_pair(0, -1)};
    
    int dfs(vector<vector<int>>& grid, int i, int j, int m, int n)
    {
        if (i < 0 or i > m - 1 or j < 0 or j > n - 1 or grid[i][j] == 0) return 0;
        grid[i][j] = 0;
        int count = 1;
        for (auto p : d) count += dfs(grid, i + p.first, j + p.second, m, n);
        return count;
    }
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) 
    {
        int result = 0, m = grid.size(), n = grid.front().size();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j]) result = max(dfs(grid, i, j, m, n), result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int d[][] = new int[][]{ {-1, 0}, {1, 0}, {0, 1}, {0, -1} };
    
    private int dfs(int[][] grid, int i, int j, int m, int n) {
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || grid[i][j] == 0) return 0;
        grid[i][j] = 0;
        int count = 1;
        for (int[] p : d) count += dfs(grid, i + p[0], j + p[1], m, n);
        return count;
    }
    
    public int maxAreaOfIsland(int[][] grid) {
        int result = 0, m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) result = Math.max(dfs(grid, i, j, m, n), result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        d, result, m, n = [[-1, 0], [1, 0], [0, 1], [0, -1]], 0, len(grid), len(grid[0])
        
        def dfs(x: int, y: int) -> int:
            if x < 0 or x > m - 1 or y < 0 or y > n - 1 or not grid[x][y]: 
                return 0
            grid[x][y], count = 0, 1
            for p in d:
                count += dfs(x + p[0], y + p[1])
            return count
        
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    result = max(result, dfs(i, j))
        return result
```
