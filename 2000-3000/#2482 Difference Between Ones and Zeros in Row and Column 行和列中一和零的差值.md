# 2482 Difference Between Ones and Zeros in Row and Column 行和列中一和零的差值

__Description:__

You are given a __0-indexed__ `m x n` binary matrix `grid`.

A __0-indexed__ `m x n` difference matrix `diff` is created with the following procedure:

- Let the number of ones in the `i ^ th` row be `onesRow_i`.
- Let the number of ones in the `j ^ th` column be `onesCol_j`.
- Let the number of zeros in the `i ^ th` row be `zerosRow_i`.
- Let the number of zeros in the `j ^ th` column be `zerosCol_j`.
- `diff[i][j] = onesRow_i + onesCol_j - zerosRow_i - zerosCol_j`

Return _the difference matrix_ `diff`.

__Example:__

Example 1:

![2482-1](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171729-5.png)

```text
Input:  grid = [[0,1,1],[1,0,1],[0,0,1]]
Output:  [[0,0,4],[0,0,4],[-2,-2,2]]
Explanation: 
```

- diff\[0]\[0] = `onesRow0 + onesCol0 - zerosRow0 - zerosCol0` = 2 + 1 - 1 - 2 = 0
- diff\[0]\[1] = `onesRow0 + onesCol1 - zerosRow0 - zerosCol1` = 2 + 1 - 1 - 2 = 0
- diff\[0]\[2] = `onesRow0 + onesCol2 - zerosRow0 - zerosCol2` = 2 + 3 - 1 - 0 = 4
- diff\[1]\[0] = `onesRow1 + onesCol0 - zerosRow1 - zerosCol0` = 2 + 1 - 1 - 2 = 0
- diff\[1]\[1] = `onesRow1 + onesCol1 - zerosRow1 - zerosCol1` = 2 + 1 - 1 - 2 = 0
- diff\[1]\[2] = `onesRow1 + onesCol2 - zerosRow1 - zerosCol2` = 2 + 3 - 1 - 0 = 4
- diff\[2]\[0] = `onesRow2 + onesCol0 - zerosRow2 - zerosCol0` = 1 + 1 - 2 - 2 = -2
- diff\[2]\[1] = `onesRow2 + onesCol1 - zerosRow2 - zerosCol1` = 1 + 1 - 2 - 2 = -2
- diff\[2]\[2] = `onesRow2 + onesCol2 - zerosRow2 - zerosCol2` = 1 + 3 - 2 - 0 = 2

Example 2:

![2482-2](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171747-6.png)

```text
Input: grid = [[1,1,1],[1,1,1]]
Output: [[5,5,5],[5,5,5]]
Explanation:
```

- diff\[0]\[0] = onesRow0 + onesCol0 - zerosRow0 - zerosCol0 = 3 + 2 - 0 - 0 = 5
- diff\[0]\[1] = onesRow0 + onesCol1 - zerosRow0 - zerosCol1 = 3 + 2 - 0 - 0 = 5
- diff\[0]\[2] = onesRow0 + onesCol2 - zerosRow0 - zerosCol2 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[0] = onesRow1 + onesCol0 - zerosRow1 - zerosCol0 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[1] = onesRow1 + onesCol1 - zerosRow1 - zerosCol1 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[2] = onesRow1 + onesCol2 - zerosRow1 - zerosCol2 = 3 + 2 - 0 - 0 = 5

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `grid[i][j]` is either `0` or `1`.

__题目描述:__

给你一个下标从 __0__ 开始的 `m x n` 二进制矩阵 `grid` 。

我们按照如下过程，定义一个下标从 __0__ 开始的 `m x n` 差值矩阵 `diff` :

- 令第 `i` 行一的数目为 `onesRow_i` 。
- 令第 `j` 列一的数目为 `onesCol_j` 。
- 令第 `i` 行零的数目为 `zerosRow_i` 。
- 令第 `j` 列零的数目为 `zerosCol_j` 。
- `diff[i][j] = onesRow_i + onesCol_j - zerosRow_i - zerosCol_j`

请你返回差值矩阵 `diff` 。

__示例:__

示例 1：

![2482-3](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171729-5.png)

```text
输入: grid = [[0,1,1],[1,0,1],[0,0,1]]
输出: [[0,0,4],[0,0,4],[-2,-2,2]]
解释: 
```

- diff\[0]\[0] = `onesRow0 + onesCol0 - zerosRow0 - zerosCol0` = 2 + 1 - 1 - 2 = 0
- diff\[0]\[1] = `onesRow0 + onesCol1 - zerosRow0 - zerosCol1` = 2 + 1 - 1 - 2 = 0
- diff\[0]\[2] = `onesRow0 + onesCol2 - zerosRow0 - zerosCol2` = 2 + 3 - 1 - 0 = 4
- diff\[1]\[0] = `onesRow1 + onesCol0 - zerosRow1 - zerosCol0` = 2 + 1 - 1 - 2 = 0
- diff\[1]\[1] = `onesRow1 + onesCol1 - zerosRow1 - zerosCol1` = 2 + 1 - 1 - 2 = 0
- diff\[1]\[2] = `onesRow1 + onesCol2 - zerosRow1 - zerosCol2` = 2 + 3 - 1 - 0 = 4
- diff\[2]\[0] = `onesRow2 + onesCol0 - zerosRow2 - zerosCol0` = 1 + 1 - 2 - 2 = -2
- diff\[2]\[1] = `onesRow2 + onesCol1 - zerosRow2 - zerosCol1` = 1 + 1 - 2 - 2 = -2
- diff\[2]\[2] = `onesRow2 + onesCol2 - zerosRow2 - zerosCol2` = 1 + 3 - 2 - 0 = 2

示例 2：

![2482-4](https://assets.leetcode.com/uploads/2022/11/06/image-20221106171747-6.png)

```text
输入：grid = [[1,1,1],[1,1,1]]
输出：[[5,5,5],[5,5,5]]
解释：
```

- diff\[0]\[0] = onesRow0 + onesCol0 - zerosRow0 - zerosCol0 = 3 + 2 - 0 - 0 = 5
- diff\[0]\[1] = onesRow0 + onesCol1 - zerosRow0 - zerosCol1 = 3 + 2 - 0 - 0 = 5
- diff\[0]\[2] = onesRow0 + onesCol2 - zerosRow0 - zerosCol2 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[0] = onesRow1 + onesCol0 - zerosRow1 - zerosCol0 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[1] = onesRow1 + onesCol1 - zerosRow1 - zerosCol1 = 3 + 2 - 0 - 0 = 5
- diff\[1]\[2] = onesRow1 + onesCol2 - zerosRow1 - zerosCol2 = 3 + 2 - 0 - 0 = 5

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `grid[i][j]` 要么是 `0` ，要么是 `1` 。

__思路:__

```text
记录每一行和每一列的 1/0 的个数
0 可以看作 -1
这样计算的时候直接相加即可
为了把 0 映射到 -1, 把 1 映射到 1
可以用 2 * grid[i][j] - 1 来实现
row[i] = 2 * grid[i][j] - 1
col[j] = 2 * grid[i][j] - 1
则结果 result[i][j] = row[i] + col[j]
可以直接原地计算
时间复杂度为 O(MN), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> onesMinusZeros(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<int> row(m), col(n);
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                row[i] += (grid[i][j] << 1) - 1;
                col[j] += (grid[i][j] << 1) - 1;
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) grid[i][j] = row[i] + col[j];
        return grid;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] onesMinusZeros(int[][] grid) {
        int m = grid.length, n = grid[0].length, row[] = new int[m], col[] = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                row[i] += (grid[i][j] << 1) - 1;
                col[j] += (grid[i][j] << 1) - 1;
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) grid[i][j] = row[i] + col[j];
        return grid;
    }
}
```

__Python__:

```Python
class Solution:
    def onesMinusZeros(self, grid: List[List[int]]) -> List[List[int]]:
        row, col = [0] * len(grid), [0] * len(grid[0])
        for i, r in enumerate(grid):
            for j, val in enumerate(r):
                row[i] += (val << 1) - 1
                col[j] += (val << 1) - 1
        for i, x in enumerate(row):
            for j, y in enumerate(col):
                grid[i][j] = x + y
        return grid
```
