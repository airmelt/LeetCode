# 62 Unique Paths 不同路径

__Description__:
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![Above is a 7 x 3 grid. How many possible unique paths are there?](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

__Example:__

Example 1:

Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:

1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right

Example 2:

Input: m = 7, n = 3
Output: 28

__Constraints:__

1 <= m, n <= 100
It's guaranteed that the answer will be less than or equal to 2 * 10 ^ 9.

__题目描述__:
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

![例如，上图是一个7 x 3 的网格。有多少可能的路径？](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

__示例 :__

示例 1:

输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。

1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右

示例 2:

输入: m = 7, n = 3
输出: 28

__提示：__

1 <= m, n <= 100
题目数据保证答案小于等于 2 * 10 ^ 9

__思路__:

1. 数学法
本质上是从 m + n - 2步中选出 m - 1(n - 1)步
时间复杂度O(1), 空间复杂度O(1)
2. 动态规划
每一格的步数等于左边加上边的步数
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int uniquePaths(int m, int n) 
    {
        vector<vector<int>> dp(m, vector<int>(n, 1));
        for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int uniquePaths(int m, int n) {
        int dp[][] = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || j == 0) dp[i][j] = 1;
                else dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        return math.factorial(m + n - 2) // (math.factorial(m - 1) * math.factorial( n - 1))
```
