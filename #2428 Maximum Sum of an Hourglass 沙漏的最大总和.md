# 2428 Maximum Sum of an Hourglass 沙漏的最大总和

__Description:__

You are given an `m x n` integer matrix `grid`.

We define an hourglass as a part of the matrix with the following form:

![2428-1](https://assets.leetcode.com/uploads/2022/08/21/img.jpg)

Return the maximum sum of the elements of an hourglass.

Note that an hourglass cannot be rotated and must be entirely contained within the matrix.

__Example:__

Example 1:

![2428-2](https://assets.leetcode.com/uploads/2022/08/21/1.jpg)

```text
Input: grid = [[6,2,1,3],[4,2,1,5],[9,2,8,7],[4,1,2,9]]
Output: 30
Explanation: The cells shown above represent the hourglass with the maximum sum: 6 + 2 + 1 + 2 + 9 + 2 + 8 = 30.
```

Example 2:

![2428-3](https://assets.leetcode.com/uploads/2022/08/21/2.jpg)

```text
Input: grid = [[1,2,3],[4,5,6],[7,8,9]]
Output: 35
Explanation: There is only one hourglass in the matrix, with the sum: 1 + 2 + 3 + 5 + 7 + 8 + 9 = 35.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `3 <= m, n <= 150`
- `0 <= grid[i][j] <= 10 ^ 6`

__题目描述:__

给你一个大小为 `m x n` 的整数矩阵 `grid` 。

按以下形式将矩阵的一部分定义为一个 沙漏 ：

![2428-4](https://assets.leetcode.com/uploads/2022/08/21/img.jpg)

返回沙漏中元素的 最大 总和。

注意：沙漏无法旋转且必须整个包含在矩阵中。

__示例:__

示例 1：

![2428-5](https://assets.leetcode.com/uploads/2022/08/21/1.jpg)

```text
输入：grid = [[6,2,1,3],[4,2,1,5],[9,2,8,7],[4,1,2,9]]
输出：30
解释：上图中的单元格表示元素总和最大的沙漏：6 + 2 + 1 + 2 + 9 + 2 + 8 = 30 。
```

示例 2：

![2428-6](https://assets.leetcode.com/uploads/2022/08/21/2.jpg)

```text
输入：grid = [[1,2,3],[4,5,6],[7,8,9]]
输出：35
解释：上图中的单元格表示元素总和最大的沙漏：1 + 2 + 3 + 5 + 7 + 8 + 9 = 35 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `3 <= m, n <= 150`
- `0 <= grid[i][j] <= 10 ^ 6`

__思路:__

```text
1. 暴力法
遍历矩阵中所有可能的沙漏
时间复杂度为 O(MN), 空间复杂度为 O(1)
2. 前缀和
计算矩阵的前缀和
遍历矩阵中所有可能的沙漏
用前缀和快速计算 3 * 3 的矩阵和
再减去沙漏中间一行的左右 2 个元素
求最大值
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSum(vector<vector<int>>& grid) 
    {
        int result = 0, m = grid.size(), n = grid.front().size();
        vector<vector<long long>> pre(m + 1, vector<long long>(n + 1));
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + grid[i - 1][j - 1];
        for (int i = 0; i < m - 2; i++) for (int j = 0; j < n - 2; j++) result = max(result, (int)(pre[i + 3][j + 3] - pre[i][j + 3] - pre[i + 3][j] + pre[i][j]  - grid[i + 1][j] - grid[i + 1][j + 2]));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSum(int[][] grid) {
        int result = 0, m = grid.length, n = grid[0].length;
        for (int i = 1; i < m - 1; i++) for (int j = 1; j < n - 1; j++) result = Math.max(grid[i - 1][j - 1] + grid[i - 1][j] + grid[i - 1][j + 1] + grid[i][j] + grid[i + 1][j - 1] + grid[i + 1][j] + grid[i + 1][j + 1], result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSum(self, grid: List[List[int]]) -> int:
        return max(grid[i - 1][j - 1] + grid[i - 1][j] + grid[i - 1][j + 1] + grid[i][j] + grid[i + 1][j - 1] + grid[i + 1][j] + grid[i + 1][j + 1] for i in range(1, len(grid) - 1) for j in range(1, len(grid[i]) - 1))
```
