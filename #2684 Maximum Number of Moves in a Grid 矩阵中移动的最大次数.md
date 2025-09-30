# 2684 Maximum Number of Moves in a Grid 矩阵中移动的最大次数

__Description:__

You are given a __0-indexed__ `m x n` matrix `grid` consisting of __positive__ integers.

You can start at any cell in the first column of the matrix, and traverse the grid in the following way:

- From a cell `(row, col)`, you can move to any of the cells: `(row - 1, col + 1)`, `(row, col + 1)` and `(row + 1, col + 1)` such that the value of the cell you move to, should be __strictly__ bigger than the value of the current cell.

Return the maximum number of moves that you can perform.

__Example:__

Example 1:

![2684-1](https://assets.leetcode.com/uploads/2023/04/11/yetgriddrawio-10.png)

```text
Input: grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]
Output: 3
Explanation: We can start at the cell (0, 0) and make the following moves:
- (0, 0) -> (0, 1).
- (0, 1) -> (1, 2).
- (1, 2) -> (2, 3).
It can be shown that it is the maximum number of moves that can be made.
```

Example 2:

```text
Input: grid = [[3,2,4],[2,1,9],[1,1,7]]
Output: 0
Explanation: Starting from any cell in the first column we cannot perform any moves.
```

![2684-2](https://assets.leetcode.com/uploads/2023/04/12/yetgrid4drawio.png)

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `m x n` 的矩阵 `grid` ，矩阵由若干 __正__ 整数组成。

你可以从矩阵第一列中的 __任一__ 单元格出发，按以下方式遍历 `grid` :

- 从单元格 `(row, col)` 可以移动到 `(row - 1, col + 1)`、 `(row, col + 1)` 和 `(row + 1, col + 1)` 三个单元格中任一满足值 __严格__ 大于当前单元格的单元格。

返回你在矩阵中能够 移动 的 最大 次数。

__示例:__

示例 1：

![2684-3](https://assets.leetcode.com/uploads/2023/04/11/yetgriddrawio-10.png)

```text
输入：grid = [[2,4,3,5],[5,4,9,3],[3,4,2,11],[10,9,13,15]]
输出：3
解释：可以从单元格 (0, 0) 开始并且按下面的路径移动：
- (0, 0) -> (0, 1).
- (0, 1) -> (1, 2).
- (1, 2) -> (2, 3).
可以证明这是能够移动的最大次数。
```

示例 2：

```text
输入：grid = [[3,2,4],[2,1,9],[1,1,7]]
输出：0
解释：从第一列的任一单元格开始都无法移动。
```

![2684-4](https://assets.leetcode.com/uploads/2023/04/12/yetgrid4drawio.png)

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 6`

__思路:__

```text
DFS
从第一列任意一个出发
如果下一列的相邻可到达格比该格大继续搜索
如果到达最后一个就终止搜索
然后将搜索过的格子置 0 表示已经访问过
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxMoves(vector<vector<int>>& grid) 
    {
        int result = 0, m = grid.size(), n = grid.front().size();
        auto dfs = [&](auto&& dfs, int i, int j) -> void
        {
            result = max(result, j);
            if (result == n - 1) return;
            for (int k = max(i - 1, 0); k < min(i + 2, m); k++) if (grid[k][j + 1] > grid[i][j]) dfs(dfs, k, j + 1);
            grid[i][j] = 0;
        };
        for (int i = 0; i < m; i++) dfs(dfs, i, 0);
        return result;    
    }
};
```

__Java__:

```Java
class Solution {
    private int result;

    public int maxMoves(int[][] grid) {
        for (int i = 0, m = grid.length; i < m; i++) dfs(i, 0, grid);
        return result;    
    }

    private void dfs(int i, int j, int[][] grid) {
        result = Math.max(result, j);
        if (result == grid[0].length - 1) return;
        for (int k = Math.max(i - 1, 0); k < Math.min(i + 2, grid.length); k++) if (grid[k][j + 1] > grid[i][j]) dfs(k, j + 1, grid);
        grid[i][j] = 0;
    }
}
```

__Python__:

```Python
class Solution:
    def maxMoves(self, grid: List[List[int]]) -> int:
        m, n, result = len(grid), len(grid[0]), 0

        def dfs(i: int, j: int) -> None:
            nonlocal result
            result = max(result, j)
            if result == n - 1:
                return
            for k in (i - 1, i, i + 1):
                if -1 < k < m and grid[k][j + 1] > grid[i][j]:
                    dfs(k, j + 1)
            grid[i][j] = 0
        for i in range(m):
            dfs(i, 0)
        return result
```
