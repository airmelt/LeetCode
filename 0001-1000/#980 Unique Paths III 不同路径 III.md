# 980 Unique Paths III 不同路径 III

__Description__:
You are given an m x n integer array grid where grid[i][j] could be:

1 representing the starting square. There is exactly one starting square.
2 representing the ending square. There is exactly one ending square.
0 representing empty squares we can walk over.
-1 representing obstacles that we cannot walk over.
Return the number of 4-directional walks from the starting square to the ending square, that walk over every non-obstacle square exactly once.

__Example:__

Example 1:

![path 1](https://assets.leetcode.com/uploads/2021/08/02/lc-unique1.jpg)

Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
Output: 2
Explanation: We have the following two paths:

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)

Example 2:

![path 2](https://assets.leetcode.com/uploads/2021/08/02/lc-unique2.jpg)

Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,0,2]]
Output: 4
Explanation: We have the following four paths:

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)

Example 3:

![path 3](https://assets.leetcode.com/uploads/2021/08/02/lc-unique3-.jpg)

Input: grid = [[0,1],[2,0]]
Output: 0
Explanation: There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 20
1 <= m * n <= 20
-1 <= grid[i][j] <= 2
There is exactly one starting cell and one ending cell.

__题目描述__:
在二维网格 grid 上，有 4 种类型的方格：

1 表示起始方格。且只有一个起始方格。
2 表示结束方格，且只有一个结束方格。
0 表示我们可以走过的空方格。
-1 表示我们无法跨越的障碍。
返回在四个方向（上、下、左、右）上行走时，从起始方格到结束方格的不同路径的数目。

每一个无障碍方格都要通过一次，但是一条路径中不能重复通过同一个方格。

__示例 :__

示例 1：

输入：[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
输出：2
解释：我们有以下两条路径：

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)

示例 2：

输入：[[1,0,0,0],[0,0,0,0],[0,0,0,2]]
输出：4
解释：我们有以下四条路径：

1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)

示例 3：

输入：[[0,1],[2,0]]
输出：0
解释：
没有一条路能完全穿过每一个空的方格一次。
请注意，起始和结束方格可以位于网格中的任意位置。

__提示:__

1 <= grid.length * grid[0].length <= 20

__思路__:

DFS ➕ 回溯
先找到起点及 0 的数目
用 DFS 探索所有方向
探索过的区域置 -1 防止重复探索
探索到 2 时检查 0 是否已经完全遍历, 完全遍历结果自增
时间复杂度为 O(4 ^ mn), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int uniquePathsIII(vector<vector<int>>& grid) 
    {
        int step = 1, m = grid.size(), n = grid[0].size(), r = 0, c = 0;
        vector<vector<int>> d{{0, -1}, {0, 1}, {1, 0}, {-1, 0}};
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] == 1) 
                {
                    r = i;
                    c = j;
                } 
                else if (!grid[i][j]) ++step;
            }
        }
        return dfs(r, c, step, m, n, grid, d);
    }
private:
    int dfs(int x, int y, int cur, int m, int n, vector<vector<int>>& grid, vector<vector<int>>& d) 
    {
        if (x < 0 or x >= m or y < 0 or y >= n or grid[x][y] == -1) return 0;
        if (grid[x][y] == 2) return !cur;
        grid[x][y] = -1;
        int result = 0;
        for (const auto& delta : d) result += dfs(x + delta.front(), y + delta.back(), cur - 1, m, n, grid, d);
        grid[x][y] = 0;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int uniquePathsIII(int[][] grid) {
        int step = 1, m = grid.length, n = grid[0].length, d[][] = new int[][]{{0, -1}, {0, 1}, {1, 0}, {-1, 0}}, r = 0, c = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    r = i;
                    c = j;
                } else if (grid[i][j] == 0) ++step;
            }
        }
        return dfs(r, c, step, m, n, grid, d);
    }
    
    private int dfs(int x, int y, int cur, int m, int n, int[][] grid, int[][] d) {
        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == -1) return 0;
        if (grid[x][y] == 2) return cur == 0 ? 1 : 0;
        grid[x][y] = -1;
        int result = 0;
        for (int[] delta : d) result += dfs(x + delta[0], y + delta[1], cur - 1, m, n, grid, d);
        grid[x][y] = 0;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def uniquePathsIII(self, grid: List[List[int]]) -> int:
        step, m, n, d = 1, len(grid), len(grid[0]), [(0, 1), (0, -1), (1, 0), (-1, 0)]
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    r, c = i, j
                elif not grid[i][j]:
                    step += 1
        
        def dfs(x: int, y: int, cur: int) -> int:
            if x < 0 or x >= m or y < 0 or y >= n or grid[x][y] == -1:
                return 0
            if grid[x][y] == 2:
                return cur == 0
            grid[x][y] = -1
            result = sum(dfs(dx + x, dy + y, cur - 1) for dx, dy in d)
            grid[x][y] = 0
            return result
        return dfs(r, c, step)
```
