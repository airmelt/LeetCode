# 1594 Maximum Non Negative Product in a Matrix 矩阵的最大非负积

__Description:__

You are given a `m x n` matrix `grid`. Initially, you are located at the top-left corner `(0, 0)`, and in each step, you can only __move right or down__ in the matrix.

Among all possible paths starting from the top-left corner `(0, 0)` and ending in the bottom-right corner `(m - 1, n - 1)`, find the path with the __maximum non-negative product__. The product of a path is the product of all integers in the grid cells visited along the path.

Return the _maximum non-negative product __modulo___ `10 ^ 9 + 7`. _If the maximum product is __negative__, return_ `-1`.

Notice that the modulo is performed after getting the maximum product.

__Example:__

Example 1:

![1594-1](https://assets.leetcode.com/uploads/2021/12/23/product1.jpg)

```text
Input: grid = [[-1,-2,-3],[-2,-3,-3],[-3,-3,-2]]
Output: -1
Explanation: It is not possible to get non-negative product in the path from (0, 0) to (2, 2), so return -1.
```

Example 2:

![1594-2](https://assets.leetcode.com/uploads/2021/12/23/product2.jpg)

```text
Input: grid = [[1,-2,1],[1,-2,1],[3,-4,1]]
Output: 8
Explanation: Maximum non-negative product is shown (1 * 1 * -2 * -4 * 1 = 8).
```

Example 3:

![1594-3](https://assets.leetcode.com/uploads/2021/12/23/product3.jpg)

```text
Input: grid = [[1,3],[0,-4]]
Output: 0
Explanation: Maximum non-negative product is shown (1 * 0 * -4 = 0).
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 15`
- `-4 <= grid[i][j] <= 4`

__题目描述:__

给你一个大小为 `rows x cols` 的矩阵 `grid` 。最初，你位于左上角 `(0, 0)` ，每一步，你可以在矩阵中 __向右__ 或 __向下__ 移动。

在从左上角 `(0, 0)` 开始到右下角 `(rows - 1, cols - 1)` 结束的所有路径中，找出具有 __最大非负积__ 的路径。路径的积是沿路径访问的单元格中所有整数的乘积。

返回 __最大非负积__ 对 __`10 ^ 9 + 7`__ __取余__ 的结果。如果最大积为负数，则返回 `-1` 。

注意，取余是在得到最大积之后执行的。

__示例:__

示例 1：

```text
输入：grid = [[-1,-2,-3],
             [-2,-3,-3],
             [-3,-3,-2]]
输出：-1
解释：从 (0, 0) 到 (2, 2) 的路径中无法得到非负积，所以返回 -1
```

示例 2：

```text
输入：grid = [[1,-2,1],
             [1,-2,1],
             [3,-4,1]]
输出：8
解释：最大非负积对应的路径已经用粗体标出 (1 * 1 * -2 * -4 * 1 = 8)
```

示例 3：

```text
输入：grid = [[1, 3],
             [0,-4]]
输出：0
解释：最大非负积对应的路径已经用粗体标出 (1 * 0 * -4 = 0)
```

示例 4：

```text
输入：grid = [[ 1, 4,4,0],
             [-2, 0,0,1],
             [ 1,-1,1,1]]
输出：2
解释：最大非负积对应的路径已经用粗体标出 (1 * -2 * 1 * -1 * 1 * 1 = 2)
```

__提示：__

- `1 <= rows, cols <= 15`
- `-4 <= grid[i][j] <= 4`

__思路:__

```text
动态规划
设 max_values[i][j] 表示到 grid[i][j] 为止最大乘积, min_values[i][j] 表示到 grid[i][j] 为止最小乘积
当 grid[i][j] < 0 时 max_values[i][j] = min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j], min_values[i][j] = max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j], 因为 grid[i][j] 为负数, 乘积会变号
否则 max_values[i][j] = max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j], min_values[i][j] = min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j]
最后取 max_values[-1][-1], 讨论 max_values[-1][-1] 的正负即可
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxProductPath(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), MOD = 1e9 + 7;
        vector<vector<long long>> max_values(m, vector<long long>(n, grid.front().front()));
        vector<vector<long long>> min_values(m, vector<long long>(n, grid.front().front()));
        for (int i = 1; i < n; i++) max_values.front()[i] = min_values.front()[i] = max_values.front()[i - 1] * grid.front()[i];
        for (int i = 1; i < m; i++) max_values[i].front() = min_values[i].front() = max_values[i - 1].front() * grid[i].front();
        for (int i = 1; i < m; i++) 
        {
            for (int j = 1; j < n; j++) 
            {
                if (grid[i][j] < 0) 
                {
                    max_values[i][j] = min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j];
                    min_values[i][j] = max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j];
                } else {
                    max_values[i][j] = max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j];
                    min_values[i][j] = min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j];
                }
            }
        }
        return max_values.back().back() < 0 ? -1 : max_values.back().back() % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProductPath(int[][] grid) {
        int m = grid.length, n = grid[0].length, MOD = 1_000_000_007;
        long[][][] dp = new long[m][n][2];
        dp[0][0][0] = dp[0][0][1] = grid[0][0];
        for (int i = 1; i < n; i++) {
            if (grid[0][i] < 0) {
                dp[0][i][0] = dp[0][i - 1][1] * grid[0][i];
                dp[0][i][1] = dp[0][i - 1][0] * grid[0][i];
            } else {
                dp[0][i][0] = dp[0][i - 1][0] * grid[0][i];
                dp[0][i][1] = dp[0][i - 1][1] * grid[0][i];
            }
        }
        for (int i = 1; i < m; i++) {
            if (grid[i][0] < 0) {
                dp[i][0][0] = dp[i - 1][0][1] * grid[i][0];
                dp[i][0][1] = dp[i - 1][0][0] * grid[i][0];
            } else {
                dp[i][0][0] = dp[i - 1][0][0] * grid[i][0];
                dp[i][0][1] = dp[i - 1][0][1] * grid[i][0];
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (grid[i][j] < 0) {
                    dp[i][j][0] = Math.min(dp[i][j - 1][1], dp[i - 1][j][1]) * grid[i][j];
                    dp[i][j][1] = Math.max(dp[i][j - 1][0], dp[i - 1][j][0]) * grid[i][j];
                } else {
                    dp[i][j][1] = Math.min(dp[i][j - 1][1],dp[i - 1][j][1]) * grid[i][j];
                    dp[i][j][0] = Math.max(dp[i][j - 1][0],dp[i - 1][j][0]) * grid[i][j];
                }
            }
        }
        return dp[m - 1][n - 1][0] < 0 ? -1 : (int)(dp[m - 1][n - 1][0] % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def maxProductPath(self, grid: List[List[int]]) -> int:
        m, n, MOD = len(grid), len(grid[0]), 10 ** 9 + 7
        max_values, min_values = [[grid[0][0]] * n for _ in range(m)], [[grid[0][0]] * n for _ in range(m)]
        for i in range(1, n):
            max_values[0][i] = min_values[0][i] = max_values[0][i - 1] * grid[0][i]
        for i in range(1, m):
            max_values[i][0] = min_values[i][0] = max_values[i - 1][0] * grid[i][0]
        for i in range(1, m):
            for j in range(1, n):
                if grid[i][j] < 0:
                    max_values[i][j], min_values[i][j] = min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j], max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j]
                else:
                    max_values[i][j], min_values[i][j] = max(max_values[i][j - 1], max_values[i - 1][j]) * grid[i][j], min(min_values[i][j - 1], min_values[i - 1][j]) * grid[i][j]
        return -1 if max_values[-1][-1] < 0 else max_values[-1][-1] % MOD
```
