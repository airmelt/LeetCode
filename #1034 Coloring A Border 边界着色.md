# 1034 Coloring A Border 边界着色

__Description__:
You are given an m x n integer matrix grid, and three integers row, col, and color. Each value in the grid represents the color of the grid square at that location.

Two squares belong to the same connected component if they have the same color and are next to each other in any of the 4 directions.

The border of a connected component is all the squares in the connected component that are either 4-directionally adjacent to a square not in the component, or on the boundary of the grid (the first or last row or column).

You should color the border of the connected component that contains the square grid[row][col] with color.

Return the final grid.

__Example:__

Example 1:

Input: grid = [[1,1],[1,2]], row = 0, col = 0, color = 3
Output: [[3,3],[3,2]]

Example 2:

Input: grid = [[1,2,2],[2,3,2]], row = 0, col = 1, color = 3
Output: [[1,3,3],[2,3,3]]

Example 3:

Input: grid = [[1,1,1],[1,1,1],[1,1,1]], row = 1, col = 1, color = 2
Output: [[2,2,2],[2,1,2],[2,2,2]]

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j], color <= 1000
0 <= row < m
0 <= col < n

__题目描述__:
给你一个大小为 m x n 的整数矩阵 grid ，表示一个网格。另给你三个整数 row、col 和 color 。网格中的每个值表示该位置处的网格块的颜色。

两个网格块属于同一 连通分量 需满足下述全部条件：

两个网格块颜色相同
在上、下、左、右任意一个方向上相邻
连通分量的边界 是指连通分量中满足下述条件之一的所有网格块：

在上、下、左、右任意一个方向上与不属于同一连通分量的网格块相邻
在网格的边界上（第一行/列或最后一行/列）
请你使用指定颜色 color 为所有包含网格块 grid[row][col] 的 连通分量的边界 进行着色，并返回最终的网格 grid 。

__示例 :__

示例 1：

输入：grid = [[1,1],[1,2]], row = 0, col = 0, color = 3
输出：[[3,3],[3,2]]

示例 2：

输入：grid = [[1,2,2],[2,3,2]], row = 0, col = 1, color = 3
输出：[[1,3,3],[2,3,3]]

示例 3：

输入：grid = [[1,1,1],[1,1,1],[1,1,1]], row = 1, col = 1, color = 2
输出：[[2,2,2],[2,1,2],[2,2,2]]

__提示:__

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j], color <= 1000
0 <= row < m
0 <= col < n

__思路__:

DFS
题意是将与 grid[row][col] 同色的区域的最外层改为 color, 如示例 3 中间的格子不需要改变
如果本来就是同一个颜色就不用修改
用 DFS 先将与 grid[row][col] 联通的区域全部改为 color 并记录下区域的位置
查询区域的中间部分, 改回原来的颜色即可
时间复杂度为 O(mn), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int row, int col, int color) 
    {
        if (grid[row][col] == color) return grid;
        int pre = grid[row][col], m = grid.size(), n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n));
        dfs(grid, row, col, pre, color, visited, m, n);
        for (int i = 1; i < m - 1; i++) for(int j = 1; j < n - 1; j++) if (visited[i][j] and visited[i - 1][j] and visited[i + 1][j] and visited[i][j - 1] and visited[i][j + 1]) grid[i][j] = pre;
        return grid;
    }
private:
    vector<vector<int>> dir{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    
    void dfs(vector<vector<int>>& grid, int x, int y, int pre, int color, vector<vector<bool>>& visited, int m, int n) 
    {
        if (x < 0 or x >= m or y < 0 or y >= n or grid[x][y] != pre) return;
        grid[x][y] = color;
        visited[x][y] = 1;
        for (const auto& d : dir) dfs(grid, x + d.front(), y + d.back(), pre, color, visited, m, n);
    }
};
```

__Java__:

```Java
class Solution {
    private int[][] dir = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        
    public int[][] colorBorder(int[][] grid, int row, int col, int color) {
        if (grid[row][col] == color) return grid;
        int pre = grid[row][col], m = grid.length, n = grid[0].length, visited[][] = new int[m][n];
        dfs(grid, row, col, pre, color, visited, m, n);
        for (int i = 1; i < m - 1; i++) for(int j = 1; j < n - 1; j++) if (visited[i][j] == 1 && visited[i - 1][j] == 1 && visited[i + 1][j] == 1 && visited[i][j - 1] == 1 && visited[i][j + 1] == 1) grid[i][j] = pre;
        return grid;
    }
    
    private void dfs(int[][] grid, int x, int y, int pre, int color, int[][] visited, int m, int n) {
        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != pre) return;
        grid[x][y] = color;
        visited[x][y] = 1;
        for (int[] d : dir) dfs(grid, x + d[0], y + d[1], pre, color, visited, m, n);
    }
}
```

__Python__:

```Python
class Solution:
    def colorBorder(self, grid: List[List[int]], row: int, col: int, color: int) -> List[List[int]]:
        if (pre := grid[row][col]) == color:
            return grid
        m, n, d = len(grid), len(grid[0]), [(0, -1), (0, 1), (1, 0), (-1, 0)]
        visited = [[False] * n for _ in range(m)]
        
        def dfs(x: int, y: int) -> None:
            if not -1 < x < m or not -1 < y < n or grid[x][y] != pre:
                return
            grid[x][y], visited[x][y] = color, True
            for dx, dy in d:
                dfs(x + dx, y + dy)        
        dfs(row, col)
        for i in range(1, m - 1):
            for j in range(1, n - 1):
                if visited[i][j] and all(visited[i + dx][j + dy] for dx, dy in d):
                    grid[i][j] = pre
        return grid
```
