# 2711 Difference of Number of Distinct Values on Diagonals 对角线上不同值的数量差

__Description:__

Given a 2D `grid` of size `m x n`, you should find the matrix `answer` of size `m x n`.

The cell `answer[r][c]` is calculated by looking at the diagonal values of the cell `grid[r][c]`:

- Let `leftAbove[r][c]` be the number of __distinct__ values on the diagonal to the left and above the cell `grid[r][c]` not including the cell `grid[r][c]` itself.
- Let `rightBelow[r][c]` be the number of __distinct__ values on the diagonal to the right and below the cell `grid[r][c]`, not including the cell `grid[r][c]` itself.
- Then `answer[r][c] = |leftAbove[r][c] - rightBelow[r][c]|`.

A matrix diagonal is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until the end of the matrix is reached.

- For example, in the below diagram the diagonal is highlighted using the cell with indices `(2, 3)` colored gray:

- Red-colored cells are left and above the cell.
- Blue-colored cells are right and below the cell.

![2711-1](https://assets.leetcode.com/uploads/2024/05/26/diagonal.png)

Return the matrix `answer`.

__Example:__

Example 1:

```text
Input: grid = [[1,2,3],[3,1,5],[3,2,1]]

Output: Output: [[1,1,0],[1,0,1],[0,1,1]]

Explanation:

To calculate the answer cells:

Output: [[1,1,0],[1,0,1],[0,1,1]]

Explanation:

To calculate the answer cells:
```

| answer      | left-above elements      | leftAbove | right-below elements      | rightBelow | \|leftAbove - rightBelow\|      |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| \[0]\[0] | [] | 0 | \[grid\[1]\[1], grid\[2]\[2]] | \|{1, 1}\| = 1| 1 |
| \[0]\[1] | [] | 0 | \[grid\[1]\[2]] | \|{5}\| = 1 | 1 |
| \[0]\[2] | [] | 0 | [] | 0 | 0 |
| \[1]\[0] | [] | 0 |  \[grid\[2]\[1]] | {2} = 1 | 1 |
| \[1]\[1] | \[grid\[0]\[0]] | \|{1}\| = 1| \[grid\[2]\[2]] | \|{1}\| = 1 | 0 |
| \[1]\[2] | \[grid\[0]\[1]] | \|{2}\| = 1| [] | 0 | 1 |
| \[2]\[0] | [] | 0 | [] | 0 | 0 |
| \[2]\[1] | \[grid\[1]\[0]] | \|{3}\| = 1 | [] | 0 | 1 |
| \[2]\[2] | \[grid\[0]\[0], grid\[1]\[1]] | \|{1, 1}\| = 1 | [] | 0 | 1 |

Example 2:

```text
Input: grid = [[1]]

Output: Output: [[0]]
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n, grid[i][j] <= 50`

__题目描述:__

给你一个下标从 `0` 开始、大小为 `m x n` 的二维矩阵 `grid` ，请你求解大小同样为 `m x n` 的答案矩阵 `answer` 。

矩阵 `answer` 中每个单元格 `(r, c)` 的值可以按下述方式进行计算:

- 令 `topLeft[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 左上角对角线上 __不同值__ 的数量。
- 令 `bottomRight[r][c]` 为矩阵 `grid` 中单元格 `(r, c)` 右下角对角线上 __不同值__ 的数量。

然后 `answer[r][c] = |topLeft[r][c] - bottomRight[r][c]|` 。

返回矩阵 `answer` 。

矩阵对角线 是从最顶行或最左列的某个单元格开始，向右下方向走到矩阵末尾的对角线。

如果单元格 `(r1, c1)` 和单元格 `(r, c)` 属于同一条对角线且 `r1 < r` ，则单元格 `(r1, c1)` 属于单元格 `(r, c)` 的左上对角线。类似地，可以定义右下对角线。

__示例:__

示例 1：

![2711-2](https://assets.leetcode.com/uploads/2023/04/19/ex2.png)

```text
输入：grid = [[1,2,3],[3,1,5],[3,2,1]]
输出：[[1,1,0],[1,0,1],[0,1,1]]
解释：第 1 个图表示最初的矩阵 grid 。 
第 2 个图表示对单元格 (0,0) 计算，其中蓝色单元格是位于右下对角线的单元格。
第 3 个图表示对单元格 (1,2) 计算，其中红色单元格是位于左上对角线的单元格。
第 4 个图表示对单元格 (1,1) 计算，其中蓝色单元格是位于右下对角线的单元格，红色单元格是位于左上对角线的单元格。
```

- 单元格 (0,0) 的右下对角线包含 [1,1] ，而左上对角线包含 [] 。对应答案是 |1 - 0| = 1 。
- 单元格 (1,2) 的右下对角线包含 [] ，而左上对角线包含 [2] 。对应答案是 |0 - 1| = 1 。
- 单元格 (1,1) 的右下对角线包含 [1] ，而左上对角线包含 [1] 。对应答案是 |1 - 1| = 0 。

其他单元格的对应答案也可以按照这样的流程进行计算。

示例 2：

```text
输入：grid = [[1]]
输出：[[0]]
解释：
```

- 单元格 (0,0) 的右下对角线包含 [] ，左上对角线包含 [] 。对应答案是 |0 - 0| = 0 。

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n, grid[i][j] <= 50`

__思路:__

```text
模拟
从一个对角线开始
沿着左上角遍历并记录出现数字的个数, 将这个个数记录到结果中
再沿着右下角遍历并用之前的结果减去这个数并求绝对值
由于数字的个数不超过 50
可以用一个 Long 型二进制表示
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> differenceOfDistinctValues(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> result(m, vector<int>(n));
        for (int i = 1; i < m + n; i++) 
        {
            uint64_t cur = 0;
            for (int j = max(n - i, 0); j <= min(m + n - 1 - i, n - 1); j++) 
            {
                result[i + j - n][j] = popcount(cur); 
                cur |= 1L << grid[i + j - n][j];
            }

            cur = 0;
            for (int j = min(m + n - 1 - i, n - 1); j >= max(n - i, 0); j--) 
            {
                result[i + j - n][j] = abs(result[i + j - n][j] - popcount(cur)); 
                cur |= 1L << grid[i + j - n][j];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] differenceOfDistinctValues(int[][] grid) {
        int m = grid.length, n =\ grid\[0]\.length, result[][] = new int[m][n];
        for (int i = 1; i < m + n; i++) {
            long cur = 0;
            for (int j = Math.max(n - i, 0); j <= Math.min(m + n - 1 - i, n - 1); j++) {
                result[i + j - n][j] = Long.bitCount(cur); 
                cur |= 1L << grid[i + j - n][j];
            }

            cur = 0;
            for (int j = Math.min(m + n - 1 - i, n - 1); j >= Math.max(n - i, 0); j--) {
                result[i + j - n][j] = Math.abs(result[i + j - n][j] - Long.bitCount(cur)); 
                cur |= 1L << grid[i + j - n][j];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def differenceOfDistinctValues(self, grid: List[List[int]]) -> List[List[int]]:
        m, n, result = len(grid), len(grid[0]), copy.deepcopy(grid)
        for i in range(1, m + n):
            cur = 0
            for j in range(max(n - i, 0), min(m + n - 1 - i, n - 1) + 1):
                result[i + j - n][j] = cur.bit_count()
                cur |= 1 << grid[i + j - n][j]
            cur = 0
            for j in range(min(m + n - 1 - i, n - 1), max(n - i, 0) - 1, -1):
                result[i + j - n][j] = abs(result[i + j - n][j] - cur.bit_count())
                cur |= 1 << grid[i + j - n][j]
        return result
```
