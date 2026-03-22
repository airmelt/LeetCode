# 861 Score After Flipping Matrix 翻转矩阵后的得分

__Description__:
You are given an m x n binary matrix grid.

A move consists of choosing any row or column and toggling each value in that row or column (i.e., changing all 0's to 1's, and all 1's to 0's).

Every row of the matrix is interpreted as a binary number, and the score of the matrix is the sum of these numbers.

Return the highest possible score after making any number of moves (including zero moves).

__Example:__

Example 1:

![Matrix](https://assets.leetcode.com/uploads/2021/07/23/lc-toogle1.jpg)

Input: grid = [[0,0,1,1],[1,0,1,0],[1,1,0,0]]
Output: 39
Explanation: 0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39

Example 2:

Input: grid = [[0]]
Output: 1

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 20
grid[i][j] is either 0 or 1.

__题目描述__:
有一个二维矩阵 A 其中每个元素的值为 0 或 1 。

移动是指选择任一行或列，并转换该行或列中的每一个值：将所有 0 都更改为 1，将所有 1 都更改为 0。

在做出任意次数的移动后，将该矩阵的每一行都按照二进制数来解释，矩阵的得分就是这些数字的总和。

返回尽可能高的分数。

__示例 :__

输入：[[0,0,1,1],[1,0,1,0],[1,1,0,0]]
输出：39
解释：
转换为 [[1,1,1,1],[1,0,0,1],[1,1,1,1]]
0b1111 + 0b1001 + 0b1111 = 15 + 9 + 15 = 39

__提示:__

1 <= A.length <= 20
1 <= A[0].length <= 20
A[i][j] 是 0 或 1

__思路__:

数学
先翻转保证第一列全部都是 1
其他列分别求出 0 和 1 的最大个数
用二进制位运算求出和即可
时间复杂度为 O(mn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int matrixScore(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid[0].size(), result = 0, k = n;
        for (int i = 0; i < m; i++) if (!grid[i][0]) for (int j = 0; j < n; j++) grid[i][j] = grid[i][j] ^ 1;
        for (int i = 0, count = 0; i < n; i++, count = 0) 
        {
            for (int j = 0; j < m; j++) if (grid[j][i] == 1) ++count;
            result += (int)(pow(2, --k)) * max(count, m - count);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int matrixScore(int[][] grid) {
        int m = grid.length, n = grid[0].length, result = 0, k = n;
        for (int i = 0; i < m; i++) if (grid[i][0] == 0) for (int j = 0; j < n; j++) grid[i][j] = grid[i][j] ^ 1;
        for (int i = 0, count = 0; i < n; i++, count = 0) {
            for (int j = 0; j < m; j++) if (grid[j][i] == 1) ++count;
            result += (int)(Math.pow(2, --k)) * Math.max(count, m - count);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def matrixScore(self, grid: List[List[int]]) -> int:
        n, m, result = len(grid), len(grid[0]), 0
        for i in range(n):
            if not grid[i][0]:
                for j in range(m):
                    grid[i][j] = 1 ^ grid[i][j]
        for i in zip(*grid):
            m -= 1
            result += 2 ** m * max(i.count(1), i.count(0))
        return result
```
