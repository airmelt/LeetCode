__Description__:
Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

__Example:__
Example 1:

Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].

Example 2:

Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.

__题目描述__:
给定一个整数矩阵，找出最长递增路径的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你不能在对角线方向上移动或移动到边界外（即不允许环绕）。

__示例 :__
示例 1:

输入: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
输出: 4 
解释: 最长递增路径为 [1, 2, 6, 9]。

示例 2:

输入: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
输出: 4 
解释: 最长递增路径是 [3, 4, 5, 6]。注意不允许在对角线方向上移动。

__思路__:
动态规划 ➕ dfs
记忆化搜索
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) 
    {
        if (matrix.empty() or matrix[0].empty()) return 0;
        int row = matrix.size(), col = matrix[0].size(), result = 0;
        vector<vector<int>> dp(row, vector<int>(col));
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) result = max(result, helper(matrix, dp, INT_MIN, i, j));
        return result;
    }
private:
    int helper(vector<vector<int>>& matrix, vector<vector<int>>& dp, int cur, int i, int j)
    {
        int row = matrix.size(), col = matrix[0].size(), result = 0;
        if (i < 0 or i > row - 1 or j > col - 1 or j < 0 or matrix[i][j] <= cur) return 0;
        if (dp[i][j]) return dp[i][j];
        result = max(result, helper(matrix, dp, matrix[i][j], i - 1, j));
        result = max(result, helper(matrix, dp, matrix[i][j], i + 1, j));
        result = max(result, helper(matrix, dp, matrix[i][j], i, j + 1));
        result = max(result, helper(matrix, dp, matrix[i][j], i, j - 1));
        dp[i][j] = result + 1;
        return result + 1;
    }
};
```

__Java__:
```Java
class Solution {
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0) return 0;
        int dp[][] = new int[matrix.length][matrix[0].length], row = matrix.length, col = matrix[0].length, result = 0;
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) result = Math.max(result, helper(matrix, dp, Integer.MIN_VALUE, i, j));
        return result;
    }
    
    private int helper(int[][] matrix, int[][] dp, int cur, int i, int j) {
        int row = matrix.length, col = matrix[0].length, result = 0;
        if (i < 0 || i > row - 1 || j > col - 1 || j < 0 || matrix[i][j] <= cur) return 0;
        if (dp[i][j] != 0) return dp[i][j];
        result = Math.max(result, helper(matrix, dp, matrix[i][j], i - 1, j));
        result = Math.max(result, helper(matrix, dp, matrix[i][j], i + 1, j));
        result = Math.max(result, helper(matrix, dp, matrix[i][j], i, j + 1));
        result = Math.max(result, helper(matrix, dp, matrix[i][j], i, j - 1));
        dp[i][j] = result + 1;
        return result + 1;
    }
}
```

__Python__:
```Python
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        if not matrix or not matrix[0]:
            return 0
        dp, result = [[0] * len(matrix[0]) for _ in range(len(matrix))], 0
        def helper(matrix: List[List[int]], dp: List[List[int]], cur: int, i: int, j: int) -> int:
            if i < 0 or i > len(matrix) - 1 or j < 0 or j > len(matrix[0]) - 1 or matrix[i][j] <= cur:
                return 0
            if dp[i][j]:
                return dp[i][j]
            result = max(helper(matrix, dp, matrix[i][j], i + x, j + y) for x, y in zip([0, 1, 0, -1], [1, 0, -1, 0]))
            dp[i][j] = result + 1
            return result + 1
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                result = max(result, helper(matrix, dp, -float('inf'), i, j))
        return result
```