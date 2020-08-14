__Description__:
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

__Example:__
Example 1:

Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Example 2:

Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

__题目描述__:
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

__示例 :__
示例 1:

输入:
[
['1','1','1','1','0'],
['1','1','0','1','0'],
['1','1','0','0','0'],
['0','0','0','0','0']
]
输出: 1

示例 2:

输入:
[
['1','1','0','0','0'],
['1','1','0','0','0'],
['0','0','1','0','0'],
['0','0','0','1','1']
]
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。

__思路__:
dfs
找到第一个为 '1'的, 即为岛屿, 然后向 4个方向搜索, 直到遇到不为 '1'的
递归终点可以设置为不为 '0'和 '1'的其他字符
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numIslands(vector<vector<char>>& grid) 
    {
        int result = 0;
        for (int i = 0; i < grid.size(); i++) for (int j = 0; j < grid[0].size(); j++) if (grid[i][j] == '1') 
        {
            dfs(grid, i, j);
            ++result;
        }
        return result;
    }
private:
    void dfs(vector<vector<char>>& grid, int i, int j)
    {
        if (i < 0 || i > grid.size() - 1 || j < 0 || j > grid[0].size() - 1 || grid[i][j] != '1') return;
        grid[i][j] = '#';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
};
```

__Java__:
```Java
class Solution {
    public int numIslands(char[][] grid) {
        int result = 0;
        for (int i = 0; i < grid.length; i++) for (int j = 0; j < grid[0].length; j++) if (grid[i][j] == '1') {
            dfs(grid, i, j);
            ++result;
        }
        return result;
    }
    
    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i > grid.length - 1 || j < 0 || j > grid[0].length - 1 || grid[i][j] != '1') return;
        grid[i][j] = '#';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
}
```

__Python__:
```Python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        result = 0
        def dfs(grid: List[List[str]], i: int, j: int) -> None:
            if i < 0 or i > len(grid) - 1 or j < 0 or j > len(grid[0]) - 1 or grid[i][j] != '1':
                return
            grid[i][j] = '#'
            dfs(grid, i + 1, j)
            dfs(grid, i - 1, j)
            dfs(grid, i, j + 1)
            dfs(grid, i, j - 1)
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(grid, i, j)
                    result += 1
        return result
```