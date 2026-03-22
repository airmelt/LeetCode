# 930 Binary Subarrays With Sum 和相同的二元子数组

__Description__:
Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix.

A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position (row, col) will be (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).

__Example:__

Example 1:

![failing1-grid](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)

Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]
Output: 13
Explanation: There are two falling paths with a minimum sum as shown.
Example 2:

![failing2-grid](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

Input: matrix = [[-19,57],[-40,-5]]
Output: -59
Explanation: The falling path with a minimum sum is shown.

__Constraints:__

n == matrix.length == matrix[i].length
1 <= n <= 100
-100 <= matrix[i][j] <= 100

__题目描述__:
给你一个 n x n 的 方形 整数数组 matrix ，请你找出并返回通过 matrix 的下降路径 的 最小和 。

下降路径 可以从第一行中的任何元素开始，并从每一行中选择一个元素。在下一行选择的元素和当前行所选元素最多相隔一列（即位于正下方或者沿对角线向左或者向右的第一个元素）。具体来说，位置 (row, col) 的下一个元素应当是 (row + 1, col - 1)、(row + 1, col) 或者 (row + 1, col + 1) 。

__示例 :__

示例 1：

输入：matrix = [[2,1,3],[6,5,4],[7,8,9]]
输出：13
解释：下面是两条和最小的下降路径，用加粗+斜体标注：
[[2,1,3],      [[2,1,3],
 [6,5,4],       [6,5,4],
 [7,8,9]]       [7,8,9]]

示例 2：

输入：matrix = [[-19,57],[-40,-5]]
输出：-59
解释：下面是一条和最小的下降路径，用加粗+斜体标注：
[[-19,57],
 [-40,-5]]

示例 3：

输入：matrix = [[-48]]
输出：-48

__提示:__

n == matrix.length
n == matrix[i].length
1 <= n <= 100
-100 <= matrix[i][j] <= 100

__思路__:

动态规划
设 dp[i][j] 表示走到 matrix[i][j] 时的最小代价
动态转移方程 dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i - 1][j + 1]) + matrix[i][j], 边界单独考虑
注意到只与上一行有关, 可以用滚动数组将空间复杂度压缩为 O(n)
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minFallingPathSum(vector<vector<int>>& matrix) 
    {
        vector<int> dp(matrix.front());
        for (int i = 1, n = matrix.size(); i < n; i++) 
        {
            vector<int> cur(dp);
            for (int j = 0; j < n; j++) cur[j] = (j == 0 ? min(dp[j], dp[j + 1]) : (j == n - 1 ? min(dp[j], dp[j - 1]) : min({ dp[j], dp[j + 1], dp[j - 1] }))) + matrix[i][j];
            dp = cur;
        }
        return *min_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length, dp[] = new int[n], result = Integer.MAX_VALUE;
        for (int i = 0; i < n; i++) dp[i] = matrix[0][i];
        for (int i = 1; i < n; i++) {
            int[] cur = new int[n];
            for (int j = 0; j < n; j++) cur[j] = dp[j];
            for (int j = 0; j < n; j++) cur[j] = (j == 0 ? Math.min(dp[j], dp[j + 1]) : (j == n - 1 ? Math.min(dp[j], dp[j - 1]) : Math.min(Math.min(dp[j], dp[j + 1]), dp[j - 1]))) + matrix[i][j];
            dp = cur;
        }
        for (int i = 0; i < n; i++) result = Math.min(result, dp[i]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        dp, n = matrix[0], len(matrix)
        for i in range(1, n):
            cur = dp[:]
            for j in range(n):
                cur[j] = (min(dp[j], dp[j - 1], dp[j + 1]) if 0 < j < n - 1 else min(dp[j], dp[j + 1]) if not j else min(dp[j], dp[j - 1])) + matrix[i][j]
            dp = cur
        return min(dp)
```
