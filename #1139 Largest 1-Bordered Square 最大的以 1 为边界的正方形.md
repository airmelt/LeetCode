# 1139 Largest 1-Bordered Square 最大的以 1 为边界的正方形

__Description__:
Given a 2D grid of 0s and 1s, return the number of elements in the largest square subgrid that has all 1s on its border, or 0 if such a subgrid doesn't exist in the grid.

__Example:__

Example 1:

Input: grid = [[1,1,1],[1,0,1],[1,1,1]]
Output: 9

Example 2:

Input: grid = [[1,1,0,0]]
Output: 1

__Constraints:__

1 <= grid.length <= 100
1 <= grid[0].length <= 100
grid[i][j] is 0 or 1

__题目描述__:
给你一个由若干 0 和 1 组成的二维网格 grid，请你找出边界全部由 1 组成的最大 正方形 子网格，并返回该子网格中的元素数量。如果不存在，则返回 0。

__示例 :__

示例 1：

输入：grid = [[1,1,1],[1,0,1],[1,1,1]]
输出：9

示例 2：

输入：grid = [[1,1,0,0]]
输出：1

__提示:__

1 <= grid.length <= 100
1 <= grid[0].length <= 100
grid[i][j] 为 0 或 1

__思路__:

动态规划
用 up 和 left 数组分别记录到 grid[i][j] 的左边及上边的最长连续 1 的长度
up[i][j] = up[i - 1][j] + 1 if grid[i - 1][j - 1] == 1 else 0
left[i][j] = up[i][j - 1] + 1 if grid[i - 1][j - 1] == 1 else 0
此时正方形边长最长为 min(up[i][j], left[i][j])
遍历上述长度, 找正方形下边和右边的最长边长, 更新到结果中
返回边长的平方即为正方形面积
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int largest1BorderedSquare(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<vector<int>> up(m + 1, vector<int>(n + 1)), left(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; i++) 
        {
            for (int j = 1; j <= n; j++) 
            {
                if (!grid[i - 1][j - 1]) continue;
                up[i][j] = up[i - 1][j] + 1;
                left[i][j] = left[i][j - 1] + 1;
                for (int k = 0, l = min(up[i][j], left[i][j]); k < l; k++) if (up[i][j - k] >= k + 1 and left[i - k][j] >= k + 1) result = max(result, k + 1);
            }
        }
        return result * result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largest1BorderedSquare(int[][] grid) {
        int m = grid.length, n = grid[0].length, up[][] = new int[m + 1][n + 1], left[][] = new int[m + 1][n + 1], result = 0;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (grid[i - 1][j - 1] == 0) continue;
                up[i][j] = up[i - 1][j] + 1;
                left[i][j] = left[i][j - 1] + 1;
                for (int k = 0, l = Math.min(up[i][j], left[i][j]); k < l; k++) if (up[i][j - k] >= k + 1 && left[i - k][j] >= k + 1) result = Math.max(result, k + 1);
            }
        }
        return result * result;
    }
}
```

__Python__:

```Python
class Solution:
    def largest1BorderedSquare(self, grid: List[List[int]]) -> int:
        result, m, n = 0, len(grid), len(grid[0])
        up, left = [[0] * (n + 1) for _ in range(m + 1)], [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if not grid[i - 1][j - 1]:
                    continue
                up[i][j], left[i][j] = up[i - 1][j] + 1, left[i][j - 1] + 1
                for k in range(min(up[i][j], left[i][j])):
                    if up[i][j - k] >= k + 1 and left[i - k][j] >= k + 1:
                        result = max(result, k + 1)
        return result ** 2
```
