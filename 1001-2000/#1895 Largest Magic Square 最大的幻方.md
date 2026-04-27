# 1895 Largest Magic Square 最大的幻方

__Description:__

A `k x k` __magic square__ is a `k x k` grid filled with integers such that every row sum, every column sum, and both diagonal sums are __all equal__. The integers in the magic square __do not have to be distinct__. Every `1 x 1` grid is trivially a __magic square__.

Given an `m x n` integer `grid`, return _the __size__ (i.e., the side length_ `k`_) of the __largest magic square__ that can be found within this grid_.

__Example:__

Example 1:

![1895-1](https://assets.leetcode.com/uploads/2021/05/29/magicsquare-grid.jpg)

```text
Input: grid = [[7,1,4,5,6],[2,5,1,6,4],[1,5,4,3,2],[1,2,7,3,4]]
Output: 3
Explanation: The largest magic square has a size of 3.
Every row sum, column sum, and diagonal sum of this magic square is equal to 12.
- Row sums: 5+1+6 = 5+4+3 = 2+7+3 = 12
- Column sums: 5+5+2 = 1+4+7 = 6+3+3 = 12
- Diagonal sums: 5+4+3 = 6+4+2 = 12
```

Example 2:

![1895-2](https://assets.leetcode.com/uploads/2021/05/29/magicsquare2-grid.jpg)

```text
Input: grid = [[5,1,3,1],[9,3,3,1],[1,3,3,8]]
Output: 2
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `1 <= grid[i][j] <= 10 ^ 6`

__题目描述:__

一个 `k x k` 的 __幻方__ 指的是一个 `k x k` 填满整数的方格阵，且每一行、每一列以及两条对角线的和 __全部____相等__ 。幻方中的整数 __不需要互不相同__ 。显然，每个 `1 x 1` 的方格都是一个幻方。

给你一个 `m x n` 的整数矩阵 `grid` ，请你返回矩阵中 __最大幻方__ 的 __尺寸__ （即边长 `k`）。

__示例:__

示例 1：

![1895-3](https://assets.leetcode.com/uploads/2021/05/29/magicsquare-grid.jpg)

```text
输入：grid = [[7,1,4,5,6],[2,5,1,6,4],[1,5,4,3,2],[1,2,7,3,4]]
输出：3
解释：最大幻方尺寸为 3 。
每一行，每一列以及两条对角线的和都等于 12 。
- 每一行的和：5+1+6 = 5+4+3 = 2+7+3 = 12
- 每一列的和：5+5+2 = 1+4+7 = 6+3+3 = 12
- 对角线的和：5+4+3 = 6+4+2 = 12
```

示例 2：

![1895-4](https://assets.leetcode.com/uploads/2021/05/29/magicsquare2-grid.jpg)

```text
输入：grid = [[5,1,3,1],[9,3,3,1],[1,3,3,8]]
输出：2
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `1 <= grid[i][j] <= 10 ^ 6`

__思路:__

```text
前缀和
利用前缀和分别求出行前缀和、列前缀和、主对角线前缀和、副对角线前缀和
从大到小依次枚举幻方的边长 k, 判断是否存在边长为 k 的幻方
遍历每一个位置, 判断以该位置为左上角的边长为 k 的正方形是否为幻方
时间复杂度为 O(M ^ 2 * N ^ 2), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestMagicSquare(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> pre_row(m, vector<int>(n + 1)), pre_col(m + 1, vector<int>(n)), main_diag(m + 1, vector<int>(n + 1)), sub_diag(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) 
        {
            pre_row[i][j + 1] = pre_row[i][j] + grid[i][j];
            pre_col[i + 1][j] = pre_col[i][j] + grid[i][j];
            main_diag[i + 1][j] = main_diag[i][j + 1] + grid[i][j];
            sub_diag[i + 1][j + 1] = sub_diag[i][j] + grid[i][j];
        }
        for (int k = min(m, n); k >= 2; k--) for (int i = 0; i <= m - k; i++) for (int j = 0; j <= n - k; j++) if (check(i, j, k, pre_row, pre_col, main_diag, sub_diag)) return k;
        return 1;
    }
private:
    bool check(int x, int y, int k, const vector<vector<int>>& pre_row, const vector<vector<int>>& pre_col, const vector<vector<int>>& main_diag, const vector<vector<int>>& sub_diag) 
    {
        for (int i = x; i < x + k; i++) if (pre_row[i][y + k] - pre_row[i][y] != main_diag[x + k][y] - main_diag[x][y + k]) return false;
        for (int j = y; j < y + k; j++) if (pre_col[x + k][j] - pre_col[x][j] != main_diag[x + k][y] - main_diag[x][y + k]) return false;
        return main_diag[x + k][y] - main_diag[x][y + k] == sub_diag[x + k][y + k] - sub_diag[x][y];
    }
};
```

__Java__:

```Java
class Solution {
    public int largestMagicSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length, preRow[][] = new int[m][n + 1], preCol[][] = new int[m + 1][n], mainDiag[][] = new int[m + 1][n + 1], subDiag[][] = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            preRow[i][j + 1] = preRow[i][j] + grid[i][j];
            preCol[i + 1][j] = preCol[i][j] + grid[i][j];
            mainDiag[i + 1][j] = mainDiag[i][j + 1] + grid[i][j];
            subDiag[i + 1][j + 1] = subDiag[i][j] + grid[i][j];
        }
        for (int k = Math.min(m, n); k >= 2; k--) for (int i = 0; i <= m - k; i++) for (int j = 0; j <= n - k; j++) if (check(i, j, k, preRow, preCol, mainDiag, subDiag)) return k;
        return 1;
    }

    private boolean check(int x, int y, int k, int[][] preRow, int[][] preCol, int[][] mainDiag, int[][] subDiag) {
        for (int i = x; i < x + k; i++) if (preRow[i][y + k] - preRow[i][y] != mainDiag[x + k][y] - mainDiag[x][y + k]) return false;
        for (int j = y; j < y + k; j++) if (preCol[x + k][j] - preCol[x][j] != mainDiag[x + k][y] - mainDiag[x][y + k]) return false;
        return mainDiag[x + k][y] - mainDiag[x][y + k] == subDiag[x + k][y + k] - subDiag[x][y];
    }
}
```

__Python__:

```Python
class Solution:
    def largestMagicSquare(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        pre_row, pre_col, main_diag, sub_diag = [[0] * (n + 1) for _ in range(m)], [[0] * n for _ in range(m + 1)], [[0] * (n + 1) for _ in range(m + 1)], [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(m):
            for j in range(n):
                pre_row[i][j + 1], pre_col[i + 1][j], main_diag[i + 1][j], sub_diag[i + 1][j + 1] = pre_row[i][j] + grid[i][j], pre_col[i][j] + grid[i][j], main_diag[i][j + 1] + grid[i][j], sub_diag[i][j] + grid[i][j]
        for k in range(min(m, n), 1, -1):
            for i in range(m + 1 - k):
                for j in range(n + 1 - k):
                    if all(pre_row[ii][j + k] - pre_row[ii][j] == main_diag[i + k][j] - main_diag[i][j + k] for ii in range(i, i + k)) and all(pre_col[i + k][jj] - pre_col[i][jj] == main_diag[i + k][j] - main_diag[i][j + k] for jj in range(j, j + k)) and main_diag[i + k][j] - main_diag[i][j + k] == sub_diag[i + k][j + k] - sub_diag[i][j]:
                        return k
        return 1
```
