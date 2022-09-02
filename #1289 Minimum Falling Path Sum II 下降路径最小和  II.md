# 1289 Minimum Falling Path Sum II 下降路径最小和  II

__Description:__

Given an n x n integer matrix grid, return the minimum sum of a falling path with non-zero shifts.

A falling path with non-zero shifts is a choice of exactly one element from each row of grid such that no two elements chosen in adjacent rows are in the same column.

__Example:__

Example 1:

![Falling Path](https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg)

Input: arr = [[1,2,3],[4,5,6],[7,8,9]]
Output: 13
Explanation:
The possible falling paths are:
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
The falling path with the smallest sum is [1,5,7], so the answer is 13.

Example 2:

Input: grid = [[7]]
Output: 7

__Constraints:__

n == grid.length == grid[i].length
1 <= n <= 200
-99 <= grid\[i][j] <= 99

__题目描述：__

给你一个 n x n 整数矩阵 arr ，请你返回 非零偏移下降路径 数字和的最小值。

非零偏移下降路径 定义为：从 arr 数组中的每一行选择一个数字，且按顺序选出来的数字中，相邻数字不在原数组的同一列。

__示例：__

示例 1：

![下降路径](https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg)

输入：arr = [[1,2,3],[4,5,6],[7,8,9]]
输出：13
解释：
所有非零偏移下降路径包括：
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
下降路径中数字和最小的是 [1,5,7] ，所以答案是 13 。

示例 2：

输入：grid = [[7]]
输出：7

__提示：__

n == grid.length == grid[i].length
1 <= n <= 200
-99 <= grid\[i][j] <= 99

__思路：__

动态规划
dp[j] 表示当前行第 j 个元素能取到的最小值
dp[j] = min(pre[k]) + grid\[i][j], 其中 pre 和 dp 的下标不能取相同值
可以用原地更新数组的方式
时间复杂度为 O(mn ^ 2), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minFallingPathSum(vector<vector<int>>& grid) 
    {
        for (int i = 1, m = grid.size(); i < m; i++) 
        {
            for (int j = 0, n = grid[0].size(); j < n; j++) 
            {
                int min_value = INT_MAX;
                for (int k = 0; k < n; k++) if (k != j) min_value = min(min_value, grid[i - 1][k]);
                grid[i][j] += min_value;
            }
        }
        return *min_element(grid.back().begin(), grid.back().end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minFallingPathSum(int[][] grid) {
        for (int i = 1, m = grid.length; i < m; i++) {
            for (int j = 0, n = grid[0].length; j < n; j++) {
                int minValue = Integer.MAX_VALUE;
                for (int k = 0; k < n; k++) if (k != j) minValue = Math.min(minValue, grid[i - 1][k]);
                grid[i][j] += minValue;
            }
        }
        int result = Integer.MAX_VALUE;
        for (int k = 0, m = grid.length, n = grid[0].length; k < n; k++) result = Math.min(result, grid[m - 1][k]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minFallingPathSum(self, grid: List[List[int]]) -> int:
        for i in range(1, len(grid)):
            grid[i] = [grid[i][j] + min(grid[i - 1][:j] + grid[i - 1][j + 1:]) for j in range(len(grid[0]))]
        return min(grid[-1])
```
