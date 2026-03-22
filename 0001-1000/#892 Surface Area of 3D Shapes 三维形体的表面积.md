# 892 Surface Area of 3D Shapes 三维形体的表面积

__Description__:
On a N \* N grid, we place some 1 \* 1 \* 1 cubes.

Each value v = grid[i][j] represents a tower of v cubes placed on top of grid cell (i, j).

Return the total surface area of the resulting shapes.

__Example:__

Example 1:

Input: [[2]]
Output: 10

Example 2:

Input: [[1,2],[3,4]]
Output: 34

Example 3:

Input: [[1,0],[0,2]]
Output: 16

Example 4:

Input: [[1,1,1],[1,0,1],[1,1,1]]
Output: 32

Example 5:

Input: [[2,2,2],[2,1,2],[2,2,2]]
Output: 46

__Note:__

1 <= N <= 50
0 <= grid[i][j] <= 50

__题目描述__:

在 N \* N 的网格上，我们放置一些 1 \* 1 \* 1  的立方体。

每个值 v = grid[i][j] 表示 v 个正方体叠放在对应单元格 (i, j) 上。

请你返回最终形体的表面积。

__示例 :__

示例 1：

输入：[[2]]
输出：10

示例 2：

输入：[[1,2],[3,4]]
输出：34

示例 3：

输入：[[1,0],[0,2]]
输出：16

示例 4：

输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32

示例 5：

输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46

__提示：__

1 <= N <= 50
0 <= grid[i][j] <= 50

__思路__:

单独看每一个立方体表面积是 4 \* grid[i][j] + 2
再减去两个相邻的立方体重叠的部分面积
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int surfaceArea(vector<vector<int>>& grid) 
    {
        int result = 0;
        for (int i = 0; i < grid.size(); i++) for (int j = 0; j < grid[0].size(); j++)
        {
            if (grid[i][j]) result += (grid[i][j] << 2) + 2;
            if (i) result -= (min(grid[i - 1][j], grid[i][j]) << 1);
            if (j) result -= (min(grid[i][j - 1], grid[i][j]) << 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int surfaceArea(int[][] grid) {
        int result = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] != 0) result += (grid[i][j] << 2) + 2;
                if (i > 0) result -= (Math.min(grid[i - 1][j], grid[i][j]) << 1);
                if (j > 0) result -= (Math.min(grid[i][j - 1], grid[i][j]) << 1);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def surfaceArea(self, grid: List[List[int]]) -> int:
        return sum(((item << 2) + 2 for row in grid for item in row if item)) - sum(((min(row[i], row[i + 1]) << 1) for row in grid for i in range(len(row) - 1))) - sum(((min(column[j], column[j + 1]) << 1) for column in zip(*grid) for j in range(len(column) - 1)))
```
