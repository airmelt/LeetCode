# 221 Maximal Square 最大正方形

__Description__:
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

__Example:__

Input:

1 0 1 0 0

1 0 __1__ __1__ 0

1 1 __1__ __1__ 1

1 0 0 1 0

Output: 4

__题目描述__:
在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

__示例 :__

输入:

1 0 1 0 0

1 0 __1__ __1__ 0

1 1 __1__ __1__ 1

1 0 0 1 0

输出: 4

__思路__:

动态规划
一个正方形只用考虑其边长
dp[i][j]表示 matrix[i - 1][j - 1]能够得到的最大的正方形的边长
由于 dp[i][j]受限于左边, 左上角和上方的边长, 可以得到状态转移方程:
dp[i][j] = 1 + min(dp[i][j - 1], dp[i - 1][j - 1], dp[i - 1][j]), matrix[i - 1][j - 1] == '1'
dp[i][j] = 0, matrix[i - 1][j - 1] == '0'
可以看到, 每次更新只需要临近两行, 可以将左上角的记录下来, 这样每次只需要一行就可以更新, 空间复杂度可以压缩到 O(n)
时间复杂度O(mn), 空间复杂度O(n), 其中 m为 matrix的大小, n为 matrix[0]的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maximalSquare(vector<vector<char>>& matrix) 
    {
        int result = 0, temp = 0;
        if (matrix.empty()) return result;
        vector<int> dp(matrix[0].size() + 1, 0);
        for (auto &row : matrix)
        {
            temp = 0;
            for (int j = 1; j < matrix[0].size() + 1; j++)
            {
                int next = dp[j];
                if (row[j - 1] == '1')
                {
                    dp[j] = 1 + min({dp[j - 1], temp, dp[j]});
                    result = max(result, dp[j]);
                }
                else dp[j] = 0;
                temp = next;
            }
        }
        return result * result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximalSquare(char[][] matrix) {
        if (matrix.length == 0) return 0;
        int dp[][] = new int[matrix.length + 1][matrix[0].length + 1], result = 0;
        for (int i = 1; i < matrix.length + 1; i++) for (int j = 1; j < matrix[0].length + 1; j++) {
            if (matrix[i - 1][j - 1] == '1') {
                dp[i][j] = 1 + Math.min(dp[i - 1][j], Math.min(dp[i - 1][j - 1], dp[i][j - 1]));
                result = Math.max(result, dp[i][j]);
            }
        }
        return result * result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        dp, result = [[0] * (len(matrix[0]) + 1) for _ in range(len(matrix) + 1)], 0
        for i in range(1, len(matrix) + 1):
            for j in range(1, len(matrix[0]) + 1):
                if matrix[i - 1][j - 1] == '1':
                    dp[i][j] = 1 + min(dp[i][j - 1], dp[i - 1][j - 1], dp[i - 1][j])
                    result = max(result, dp[i][j])
        return result ** 2
```
