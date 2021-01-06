# 64 Minimum Path Sum 最小路径和

__Description__:
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

__Note:__
You can only move either down or right at any point in time.

__Example:__

Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.

__题目描述__:
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

__说明：__
每次只能向下或者向右移动一步。

__示例 :__

输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

__思路__:

动态规划

1. 边界 dp[i][0] = dp[i][0] + dp[i - 1][0], dp[0][i] = dp[0][i] + dp[0][i - 1]
2. 内部 dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + dp[i][j]
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minPathSum(vector<vector<int>>& grid) 
    {
        if (grid.empty()) return 0;
        for (int i = 1; i < grid.size(); i++) grid[i][0] += grid[i - 1][0];
        for (int i = 1; i < grid[0].size(); i++) grid[0][i] += grid[0][i - 1];
        for (int i = 1; i < grid.size(); i++) for (int j = 1; j < grid[0].size(); j++) grid[i][j] += min(grid[i - 1][j], grid[i][j - 1]);
        return grid.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minPathSum(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) return 0;
        for (int i = 1; i < grid.length; i++) grid[i][0] += grid[i - 1][0];
        for (int i = 1; i < grid[0].length; i++) grid[0][i] += grid[0][i - 1];
        for (int i = 1; i < grid.length; i++) for (int j = 1; j < grid[0].length; j++) grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
        return grid[grid.length - 1][grid[0].length - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return 0
        for i in range(1, len(grid)):
            grid[i][0] += grid[i - 1][0]
        for j in range(1, len(grid[0])):
            grid[0][j] += grid[0][j - 1]
        for i in range(1, len(grid)):
            for j in range(1, len(grid[0])):
                grid[i][j] += min(grid[i - 1][j], grid[i][j - 1])
        return grid[-1][-1]
```
