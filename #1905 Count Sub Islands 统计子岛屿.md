# 1905 Count Sub Islands 统计子岛屿

__Description:__

You are given two `m x n` binary matrices `grid1` and `grid2` containing only `0`'s (representing water) and `1`'s (representing land). An __island__ is a group of `1`'s connected __4-directionally__ (horizontal or vertical). Any cells outside of the grid are considered water cells.

An island in `grid2` is considered a __sub-island__ if there is an island in `grid1` that contains __all__ the cells that make up __this__ island in `grid2`.

Return the ___number__ of islands in_ `grid2` _that are considered __sub-islands___.

__Example:__

Example 1:

![1905-1](https://assets.leetcode.com/uploads/2021/06/10/test1.png)

```text
Input: grid1 = [[1,1,1,0,0],[0,1,1,1,1],[0,0,0,0,0],[1,0,0,0,0],[1,1,0,1,1]], grid2 = [[1,1,1,0,0],[0,0,1,1,1],[0,1,0,0,0],[1,0,1,1,0],[0,1,0,1,0]]
Output: 3
Explanation: In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are three sub-islands.
```

Example 2:

![1905-2](https://assets.leetcode.com/uploads/2021/06/03/testcasex2.png)

```text
Input: grid1 = [[1,0,1,0,1],[1,1,1,1,1],[0,0,0,0,0],[1,1,1,1,1],[1,0,1,0,1]], grid2 = [[0,0,0,0,0],[1,1,1,1,1],[0,1,0,1,0],[0,1,0,1,0],[1,0,0,0,1]]
Output: 2 
Explanation: In the picture above, the grid on the left is grid1 and the grid on the right is grid2.
The 1s colored red in grid2 are those considered to be part of a sub-island. There are two sub-islands.
```

__Constraints:__

- `m == grid1.length == grid2.length`
- `n == grid1[i].length == grid2[i].length`
- `1 <= m, n <= 500`
- `grid1[i][j]` and `grid2[i][j]` are either `0` or `1`.

__题目描述:__

给你两个 `m x n` 的二进制矩阵 `grid1` 和 `grid2` ，它们只包含 `0` （表示水域）和 `1` （表示陆地）。一个 __岛屿__ 是由 __四个方向__ （水平或者竖直）上相邻的 `1` 组成的区域。任何矩阵以外的区域都视为水域。

如果 `grid2` 的一个岛屿，被 `grid1` 的一个岛屿 __完全__ 包含，也就是说 `grid2` 中该岛屿的每一个格子都被 `grid1` 中同一个岛屿完全包含，那么我们称 `grid2` 中的这个岛屿为 __子岛屿__ 。

请你返回 `grid2` 中 __子岛屿__ 的 __数目__ 。

__示例:__

示例 1：

![1905-3](https://assets.leetcode.com/uploads/2021/06/10/test1.png)

```text
输入：grid1 = [[1,1,1,0,0],[0,1,1,1,1],[0,0,0,0,0],[1,0,0,0,0],[1,1,0,1,1]], grid2 = [[1,1,1,0,0],[0,0,1,1,1],[0,1,0,0,0],[1,0,1,1,0],[0,1,0,1,0]]
输出：3
解释：如上图所示，左边为 grid1 ，右边为 grid2 。
grid2 中标红的 1 区域是子岛屿，总共有 3 个子岛屿。
```

示例 2：

![1905-4](https://assets.leetcode.com/uploads/2021/06/03/testcasex2.png)

```text
输入：grid1 = [[1,0,1,0,1],[1,1,1,1,1],[0,0,0,0,0],[1,1,1,1,1],[1,0,1,0,1]], grid2 = [[0,0,0,0,0],[1,1,1,1,1],[0,1,0,1,0],[0,1,0,1,0],[1,0,0,0,1]]
输出：2 
解释：如上图所示，左边为 grid1 ，右边为 grid2 。
grid2 中标红的 1 区域是子岛屿，总共有 2 个子岛屿。
```

__提示：__

- `m == grid1.length == grid2.length`
- `n == grid1[i].length == grid2[i].length`
- `1 <= m, n <= 500`
- `grid1[i][j]` 和 `grid2[i][j]` 都要么是 `0` 要么是 `1` 。

__思路:__

```text
DFS
遍历 grid2
如果找到某个岛屿开始 DFS, 设置 flag 为 true 表示是岛屿
直接在 grid2 上修改, grid2 对应位置置为 0
如果对应 grid1 为 0 说明不是子岛屿, 将 flag 设为 false
如果 flag 为 true, 说明是子岛屿, 结果加 1
时间复杂度为 O(MN), 空间复杂度为 O(1), M, N 分别为 grid1 和 grid2 的行数和列数, 使用 grid2 不需要额外空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSubIslands(vector<vector<int>>& grid1, vector<vector<int>>& grid2) 
    {
        int m = grid1.size(), n = grid1.front().size(), result = 0;
        vector<vector<int>> d{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid2[i][j]) 
                {
                    dfs(i, j, m, n, d, grid1, grid2);
                    result += flag;
                    flag = true;
                }
            }
        }
        return result;
    }
private:
    bool flag = true;

    void dfs(int x, int y, int m, int n, vector<vector<int>>& d, vector<vector<int>>& grid1, vector<vector<int>>& grid2) 
    {
        if (x > m - 1 or y > n - 1 or x < 0 or y < 0 or !grid2[x][y]) return;
        grid2[x][y] = 0;
        if (!grid1[x][y]) flag = false;
        for (const auto& dir : d) dfs(x + dir.front(), y + dir.back(), m, n, d, grid1, grid2);
    }
};
```

__Java__:

```Java
class Solution {
    private boolean flag = true;

    public int countSubIslands(int[][] grid1, int[][] grid2) {
        int m = grid1.length, n = grid1[0].length, result = 0, d[][] = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid2[i][j] == 1) {
                    dfs(i, j, m, n, d, grid1, grid2);
                    result += flag ? 1 : 0;
                    flag = true;
                }
            }
        }
        return result;
    }
        
    private void dfs(int x, int y, int m, int n, int[][] d, int[][] grid1, int[][] grid2) {
        if (x > m - 1 || y > n - 1 || x < 0 || y < 0 || grid2[x][y] == 0) return;
        grid2[x][y] = 0;
        if (grid1[x][y] == 0) flag = false;
        for (int[] dir : d) dfs(x + dir[0], y + dir[1], m, n, d, grid1, grid2);
    }
}
```

__Python__:

```Python
class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        m, n, result, d, flag = len(grid1), len(grid1[0]), 0, [(0, 1), (0, -1), (1, 0), (-1, 0)], True
        
        def dfs(x: int, y: int) -> NoReturn:
            nonlocal flag
            if x > m - 1 or y > n - 1 or x < 0 or y < 0 or not grid2[x][y]:
                return
            grid2[x][y] = 0
            if not grid1[x][y]:
                flag = False
            for dx, dy in d:
                dfs(x + dx, y + dy)
            
        for i in range(m):
            for j in range(n):
                if grid2[i][j]:
                    dfs(i, j)
                    result += flag
                    flag = True
        return result
```
