__Description__:
A 3 x 3 magic square is a 3 x 3 grid filled with distinct numbers from 1 to 9 such that each row, column, and both diagonals all have the same sum.

Given an grid of integers, how many 3 x 3 "magic square" subgrids are there?  (Each subgrid is contiguous).

__Example:__
Example 1:

Input: [[4,3,8,4],
        [9,5,1,9],
        [2,7,6,2]]
Output: 1
Explanation: 
The following subgrid is a 3 x 3 magic square:
438
951
276

while this one is not:
384
519
762

In total, there is only one magic square inside the given grid.

__Notes:__

1 <= grid.length <= 10
1 <= grid[0].length <= 10
0 <= grid[i][j] <= 15

__题目描述__:
3 x 3 的幻方是一个填充有从 1 到 9 的不同数字的 3 x 3 矩阵，其中每行，每列以及两条对角线上的各数之和都相等。

给定一个由整数组成的 grid，其中有多少个 3 × 3 的 “幻方” 子矩阵？（每个子矩阵都是连续的）。

__示例 :__

输入: [[4,3,8,4],
      [9,5,1,9],
      [2,7,6,2]]
输出: 1
解释: 
下面的子矩阵是一个 3 x 3 的幻方：
438
951
276

而这一个不是：
384
519
762

总的来说，在本示例所给定的矩阵中只有一个 3 x 3 的幻方子矩阵。

__提示:__

1 <= grid.length <= 10
1 <= grid[0].length <= 10
0 <= grid[i][j] <= 15

__思路__:
注意到幻方的正中间那个数一定是 5, 可以找到 5之后扩展为 3 ✖️ 3矩阵再判断是否为幻方
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    int numMagicSquaresInside(vector<vector<int>>& grid) {
        int result = 0;
        for (int i = 1; i < grid.size() - 1; i++) for (int j = 1; j < grid[0].size() - 1; j++) if (grid[i][j] == 5) if (is_magic_square(grid, i, j)) result++;
        return result;
    }
private:
    bool is_magic_square(vector<vector<int>>& grid, int i, int j) {
        if (grid.size() < 3 || grid[0].size() < 3) return false;
        if (!is_valid(grid, i, j)) return false;
        if (grid[i - 1][j - 1] + grid[i - 1][j] + grid[i - 1][j + 1] == 15 && grid[i][j - 1] + grid[i][j] + grid[i][j + 1] == 15 && grid[i + 1][j - 1] + grid[i + 1][j] + grid[i + 1][j + 1] == 15 && grid[i - 1][j - 1] + grid[i][j - 1] + grid[i + 1][j - 1] == 15 && grid[i - 1][j] + grid[i][j] + grid[i + 1][j] == 15 && grid[i - 1][j + 1] + grid[i][j + 1] + grid[i + 1][j + 1] == 15 && grid[i - 1][j - 1] + grid[i][j] + grid[i + 1][j + 1] == 15 && grid[i + 1][j - 1] + grid[i][j] + grid[i - 1][j + 1] == 15) return true;
        return false;
    }
    
    bool is_valid(vector<vector<int>>& grid, int i, int j) {
        int count[16] = {0};
        for (int r = i - 1; r < i + 2; r++) for (int c = j - 1; c < j + 2; c++) count[grid[r][c]]++;
        for (int k = 1; k <= 9; k++) if (!count[k]) return false;
        return true;
    }
};
```

__Java__:
```Java
class Solution {
    public int numMagicSquaresInside(int[][] grid) {
        int result = 0;
        for (int i = 1; i < grid.length - 1; i++) for (int j = 1; j < grid[0].length - 1; j++) if (grid[i][j] == 5) if (isMagicSquare(grid, i, j)) result++;
        return result;
    }
    
    private boolean isMagicSquare(int[][] grid, int i, int j) {
        if (grid.length < 3 || grid[0].length < 3) return false;
        if (!isValid(grid, i, j)) return false;
        if (grid[i - 1][j - 1] + grid[i - 1][j] + grid[i - 1][j + 1] == 15 && grid[i][j - 1] + grid[i][j] + grid[i][j + 1] == 15 && grid[i + 1][j - 1] + grid[i + 1][j] + grid[i + 1][j + 1] == 15 && grid[i - 1][j - 1] + grid[i][j - 1] + grid[i + 1][j - 1] == 15 && grid[i - 1][j] + grid[i][j] + grid[i + 1][j] == 15 && grid[i - 1][j + 1] + grid[i][j + 1] + grid[i + 1][j + 1] == 15 && grid[i - 1][j - 1] + grid[i][j] + grid[i + 1][j + 1] == 15 && grid[i + 1][j - 1] + grid[i][j] + grid[i - 1][j + 1] == 15) return true;
        return false;
    }
    
    private boolean isValid(int[][] grid, int i, int j) {
        int[] count = new int[16];
        for (int r = i - 1; r < i + 2; r++) for (int c = j - 1; c < j + 2; c++) count[grid[r][c]]++;
        for (int k = 1; k <= 9; k++) if (count[k] != 1) return false;
        return true;
    }
}
```

__Python__:
```Python
class Solution:
    def numMagicSquaresInside(self, grid: List[List[int]]) -> int:
        return len([(i, j) for i in range(len(grid) - 2) for j in range(len(grid[0]) - 2) if grid[i][j:j + 3] + grid[i + 1][j:j + 3] + grid[i + 2][j:j + 3] in ([8,1,6,3,5,7,4,9,2],[6,1,8,7,5,3,2,9,4],[4,9,2,3,5,7,8,1,6],[2,9,4,7,5,3,6,1,8],[6,7,2,1,5,9,8,3,4],[8,3,4,1,5,9,6,7,2],[2,7,6,9,5,1,4,3,8],[4,3,8,9,5,1,2,7,6])])
```