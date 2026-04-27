# 1020 Number of Enclaves 飞地的数量

__Description__:
You are given an m x n binary matrix grid, where 0 represents a sea cell and 1 represents a land cell.

A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.

Return the number of land cells in grid for which we cannot walk off the boundary of the grid in any number of moves.

__Example:__

Example 1:

![Enclaves 1](https://assets.leetcode.com/uploads/2021/02/18/enclaves1.jpg)

Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3
Explanation: There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.

Example 2:

![Enclaves 2](https://assets.leetcode.com/uploads/2021/02/18/enclaves2.jpg)

Input: grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
Output: 0
Explanation: All 1s are either on the boundary or can reach the boundary.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 500
grid[i][j] is either 0 or 1.

__题目描述__:
给你一个大小为 m x n 的二进制矩阵 grid ，其中 0 表示一个海洋单元格、1 表示一个陆地单元格。

一次 移动 是指从一个陆地单元格走到另一个相邻（上、下、左、右）的陆地单元格或跨过 grid 的边界。

返回网格中 无法 在任意次数的移动中离开网格边界的陆地单元格的数量。

__示例 :__

示例 1：

![飞地 1](https://assets.leetcode.com/uploads/2021/02/18/enclaves1.jpg)

输入：grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
输出：3
解释：有三个 1 被 0 包围。一个 1 没有被包围，因为它在边界上。

示例 2：

![飞地 2](https://assets.leetcode.com/uploads/2021/02/18/enclaves2.jpg)

输入：grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
输出：0
解释：所有 1 都在边界上或可以到达边界。

__提示:__

m == grid.length
n == grid[i].length
1 <= m, n <= 500
grid[i][j] 的值为 0 或 1

__思路__:

DFS
从边界开始遍历
找到不为 1 的点或者已经访问过的点就返回
最后统计所有内部没访问过的为 1 的点
时间复杂度为 O(mn), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<vector<int>> d{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y, int m, int n) 
    {
        if (x < 0 or x >= m or y < 0 or y >= n or !grid[x][y] or visited[x][y]) return;
        visited[x][y] = true;
        for (const auto& delta : d) dfs(grid, visited, x + delta.front(), y + delta.back(), m, n);
    }
public:
    int numEnclaves(vector<vector<int>>& grid) 
    {
        int result = 0, m = grid.size(), n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n));
        for (int i = 0; i < m; i++) 
        {
            dfs(grid, visited, i, 0, m, n);
            dfs(grid, visited, i, n - 1, m, n);
        }
        for (int j = 0; j < n; j++) 
        {
            dfs(grid, visited, 0, j, m, n);
            dfs(grid, visited, m - 1, j, m, n);
        }
        for (int i = 1; i < m - 1; i++) for (int j = 1; j < n - 1; j++) if (grid[i][j] and !visited[i][j]) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[][] d = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    public int numEnclaves(int[][] grid) {
        int result = 0, m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            dfs(grid, visited, i, 0, m, n);
            dfs(grid, visited, i, n - 1, m, n);
        }
        for (int j = 0; j < n; j++) {
            dfs(grid, visited, 0, j, m, n);
            dfs(grid, visited, m - 1, j, m, n);
        }
        for (int i = 1; i < m - 1; i++) for (int j = 1; j < n - 1; j++) if (grid[i][j] == 1 && !visited[i][j]) ++result;
        return result;
    }
    
    private void dfs(int[][] grid, boolean[][] visited, int x, int y, int m, int n) {
        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 0 || visited[x][y]) return;
        visited[x][y] = true;
        for (int[] delta : d) dfs(grid, visited, x + delta[0], y + delta[1], m, n);
    }
}
```

__Python__:

```Python
class Solution:
    def numEnclaves(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        visited = [[False] * n for _ in range(m)]

        def dfs(r: int, c: int) -> None:
            if r < 0 or r >= m or c < 0 or c >= n or grid[r][c] == 0 or visited[r][c]:
                return
            visited[r][c] = True
            for x, y in ((r - 1, c), (r + 1, c), (r, c - 1), (r, c + 1)):
                dfs(x, y)

        for i in range(m):
            dfs(i, 0)
            dfs(i, n - 1)
        for j in range(1, n - 1):
            dfs(0, j)
            dfs(m - 1, j)
        return sum(grid[i][j] and not visited[i][j] for i in range(1, m - 1) for j in range(1, n - 1))
```
