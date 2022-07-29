# 1219 Path with Maximum Gold 黄金矿工

__Description:__

In a gold mine grid of size m x n, each cell in this mine has an integer representing the amount of gold in that cell, 0 if it is empty.

Return the maximum amount of gold you can collect under the conditions:

Every time you are located in a cell you will collect all the gold in that cell.
From your position, you can walk one step to the left, right, up, or down.
You can't visit the same cell more than once.
Never visit a cell with 0 gold.
You can start and stop collecting gold from any position in the grid that has some gold.

__Example:__

Example 1:

Input: grid = [[0,6,0],[5,8,7],[0,9,0]]
Output: 24
Explanation:
[[0,6,0],
 [5,8,7],
 [0,9,0]]
Path to get the maximum gold, 9 -> 8 -> 7.

Example 2:

Input: grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
Output: 28
Explanation:
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
Path to get the maximum gold, 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 15
0 <= grid[i][j] <= 100
There are at most 25 cells containing gold.

__题目描述：__

你要开发一座金矿，地质勘测学家已经探明了这座金矿中的资源分布，并用大小为 m * n 的网格 grid 进行了标注。每个单元格中的整数就表示这一单元格中的黄金数量；如果该单元格是空的，那么就是 0。

为了使收益最大化，矿工需要按以下规则来开采黄金：

每当矿工进入一个单元，就会收集该单元格中的所有黄金。
矿工每次可以从当前位置向上下左右四个方向走。
每个单元格只能被开采（进入）一次。
不得开采（进入）黄金数目为 0 的单元格。
矿工可以从网格中 任意一个 有黄金的单元格出发或者是停止。

__示例：__

示例 1：

输入：grid = [[0,6,0],[5,8,7],[0,9,0]]
输出：24
解释：
[[0,6,0],
 [5,8,7],
 [0,9,0]]
一种收集最多黄金的路线是：9 -> 8 -> 7。

示例 2：

输入：grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
输出：28
解释：
[[1,0,7],
 [2,0,6],
 [3,4,5],
 [0,3,0],
 [9,0,20]]
一种收集最多黄金的路线是：1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7。

__提示：__

1 <= grid.length, grid[i].length <= 15
0 <= grid[i][j] <= 100
最多 25 个单元格中有黄金。

__思路：__

DFS
从每一个不是 0 的点出发进行 DFS, 取得最大值即可
时间复杂度为 O(3 ^ mn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    int result, m, n;
    vector<vector<int>> d{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
    void dfs(int x, int y, int gold, vector<vector<int>>& grid) 
    {
        gold += grid[x][y];
        result = max(result, gold);
        int record = grid[x][y];
        grid[x][y] = 0;
        for (const auto& dir : d) if (x + dir.front() < m and x + dir.front() > -1 and y + dir.back() < n and y + dir.back() > -1 and grid[x + dir.front()][y + dir.back()] > 0) dfs(x + dir.front(), y + dir.back(), gold, grid);
        grid[x][y] = record;
    }
public:
    int getMaximumGold(vector<vector<int>>& grid) 
    {
        result = 0;
        m = grid.size();
        n = grid.front().size();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] != 0) dfs(i, j, 0, grid);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0, m, n, d[][] = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    
    public int getMaximumGold(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] != 0) dfs(i, j, 0, grid);
        return result;
    }
    
    private void dfs(int x, int y, int gold, int[][] grid) {
        gold += grid[x][y];
        result = Math.max(result, gold);
        int record = grid[x][y];
        grid[x][y] = 0;
        for (int[] dir : d) if (x + dir[0] < m && x + dir[0] > -1 && y + dir[1] < n && y + dir[1] > -1 && grid[x + dir[0]][y + dir[1]] > 0) dfs(x + dir[0], y + dir[1], gold, grid);
        grid[x][y] = record;
    }
}
```

__Python__:

```Python
class Solution:
    def getMaximumGold(self, grid: List[List[int]]) -> int:
        m, n, result, d = len(grid), len(grid[0]), 0, [(-1, 0), (1, 0), (0, -1), (0, 1)]

        def dfs(x: int, y: int, gold: int) -> None:
            gold += grid[x][y]
            nonlocal result
            result, record, grid[x][y] = max(result, gold), grid[x][y], 0
            for dx, dy in d:
                if -1 < x + dx < m and -1 < y + dy < n and grid[x + dx][y + dy] > 0:
                    dfs(x + dx, y + dy, gold)
            grid[x][y] = record
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    dfs(i, j, 0)
        return result
```
