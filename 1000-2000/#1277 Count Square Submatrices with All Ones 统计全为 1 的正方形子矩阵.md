# 1277 Count Square Submatrices with All Ones 统计全为 1 的正方形子矩阵

__Description:__

Given a m * n matrix of ones and zeros, return how many square submatrices have all ones.

__Example:__

Example 1:

Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
Output: 15
Explanation:
There are 10 squares of side 1.
There are 4 squares of side 2.
There is  1 square of side 3.
Total number of squares = 10 + 4 + 1 = 15.
Example 2:

Input: matrix =
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
Output: 7
Explanation:
There are 6 squares of side 1.  
There is 1 square of side 2.
Total number of squares = 6 + 1 = 7.

__Constraints:__

1 <= arr.length <= 300
1 <= arr[0].length <= 300
0 <= arr[i][j] <= 1

__题目描述：__

给你一个 m * n 的矩阵，矩阵中的元素不是 0 就是 1，请你统计并返回其中完全由 1 组成的 正方形 子矩阵的个数。

__示例：__

示例 1：

输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释：
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15.

示例 2：

输入：matrix =
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
输出：7
解释：
边长为 1 的正方形有 6 个。
边长为 2 的正方形有 1 个。
正方形的总数 = 6 + 1 = 7.

__提示：__

1 <= arr.length <= 300
1 <= arr[0].length <= 300
0 <= arr[i][j] <= 1

__思路：__

动态规划
令 dp[i + 1][j + 1] 表示以 matrix[i][j] 为右下角的最大正方形的边长
dp[i + 1][j + 1] = 0, if matrix[i][j] == 0
dp[i + 1][j + 1] = min(dp[i + 1][j], dp[i][j], dp[i][j + 1]) + 1, if matrix[i][j] == 1
也就是说右下角的值取决于左上, 上方和左边的最小值加上自身的长度
将 dp 求和即为结果
注意到, dp[i + 1] 只和当前行和前一行有关, 可以将空间复杂度压缩到 O(n)
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int countSquares(vector<vector<int>>& matrix)
    {
        int m = matrix.size(), n = matrix.front().size(), result = 0;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (i and j and matrix[i][j]) matrix[i][j] += min({matrix[i - 1][j - 1], matrix[i - 1][j], matrix[i][j - 1]});
                result += matrix[i][j];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countSquares(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length, dp[][] = new int[m + 1][n + 1], result = 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (matrix[i][j] == 1) dp[i + 1][j + 1] = Math.min(dp[i][j + 1], Math.min(dp[i][j], dp[i + 1][j])) + 1;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result += dp[i + 1][j + 1];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, dp, result = len(matrix), [0] * ((n := len(matrix[0])) + 1), 0
        for i in range(m):
            cur = [0] * (n + 1)
            for j in range(n):
                if matrix[i][j]:
                    cur[j + 1] = min(dp[j], cur[j], dp[j + 1]) + 1
                    result += cur[j + 1]
            dp = cur
        return result
```
