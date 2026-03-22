# 1254 Number of Closed Islands 统计封闭岛屿的数目

__Description:__

Given a 2D grid consists of 0s (land) and 1s (water).  An island is a maximal 4-directionally connected group of 0s and a closed island is an island totally (all left, top, right, bottom) surrounded by 1s.

Return the number of closed islands.

__Example:__

Example 1:

![Islands 1](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

Input: grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
Output: 2
Explanation:
Islands in gray are closed because they are completely surrounded by water (group of 1s).

Example 2:

![Islands 2](https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png)

Input: grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
Output: 1

Example 3:

Input: grid = [[1,1,1,1,1,1,1],
               [1,0,0,0,0,0,1],
               [1,0,1,1,1,0,1],
               [1,0,1,0,1,0,1],
               [1,0,1,1,1,0,1],
               [1,0,0,0,0,0,1],
               [1,1,1,1,1,1,1]]
Output: 2

__Constraints:__

1 <= grid.length, grid[0].length <= 100
0 <= grid[i][j] <=1

__题目描述：__

二维矩阵 grid 由 0 （土地）和 1 （水）组成。岛是由最大的4个方向连通的 0 组成的群，封闭岛是一个 完全 由1包围（左、上、右、下）的岛。

请返回 封闭岛屿 的数目。

__示例：__

示例 1：

![岛屿 1](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

输入：grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
输出：2
解释：
灰色区域的岛屿是封闭岛屿，因为这座岛屿完全被水域包围（即被 1 区域包围）。

示例 2：

![岛屿 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/07/sample_4_1610.png)

输入：grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
输出：1

示例 3：

输入：grid = [[1,1,1,1,1,1,1],
             [1,0,0,0,0,0,1],
             [1,0,1,1,1,0,1],
             [1,0,1,0,1,0,1],
             [1,0,1,1,1,0,1],
             [1,0,0,0,0,0,1],
             [1,1,1,1,1,1,1]]
输出：2

__提示：__

1 <= grid.length, grid[0].length <= 100
0 <= grid[i][j] <=1

__思路：__

DFS
因为封闭岛屿需要完全被水域包围, 所以岛屿不能触碰边界
只要岛屿到达边界立即返回 false
如果岛屿碰到水域可以直接返回 true
需要将岛屿的 4 个方向都搜索一遍, 不能使用 && 运算符, 会短路
时间复杂度为 O(mn), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    bool dfs(int x, int y, vector<vector<int>>& grid, int m, int n)
    {
        if (x < 0 or x >= m or y < 0 or y >= n) return 0;
        if (grid[x][y]) return 1;
        grid[x][y] = 1;
        return dfs(x + 1, y, grid, m, n) & dfs(x, y + 1, grid, m, n) & dfs(x - 1, y, grid, m, n) & dfs(x, y - 1, grid, m, n);
    }
public:
    int closedIsland(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 0 and dfs(i, j, grid, m, n)) ++result; 
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int closedIsland(int[][] grid) {
        int m = grid.length, n = grid[0].length, result = 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 0 && dfs(i, j, grid, m, n)) ++result; 
        return result;
    }
    
    private boolean dfs(int x, int y, int[][] grid, int m, int n) {
        if (x < 0 || x >= m || y < 0 || y >= n) return false;
        if (grid[x][y] == 1) return true;
        grid[x][y] = 1;
        return dfs(x + 1, y, grid, m, n) & dfs(x, y + 1, grid, m, n) & dfs(x - 1, y, grid, m, n) & dfs(x, y - 1, grid, m, n);
    }
}
```

__Python__:

```Python
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        m, n, d = len(grid), len(grid[0]), [(0, 1), (0, -1), (1, 0), (-1, 0)]
        
        def dfs(x: int, y: int) -> int:
            if (not -1 < x < m) or (not -1 < y < n):
                return 0
            if grid[x][y]:
                return 1
            grid[x][y] = 1
            return reduce(and_, (dfs(x + dx, y + dy) for dx, dy in d))
        return sum(not grid[i][j] and dfs(i, j) for i in range(m) for j in range(n))
```
