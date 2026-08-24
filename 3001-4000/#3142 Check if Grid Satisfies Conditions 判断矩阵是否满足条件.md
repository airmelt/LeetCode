# 3142 Check if Grid Satisfies Conditions 判断矩阵是否满足条件

__Description:__

You are given a 2D matrix `grid` of size `m x n`. You need to check if each cell `grid[i][j]` is:

Equal to the cell below it, i.e. `grid[i][j] == grid[i + 1][j]` (if it exists).
Different from the cell to its right, i.e. `grid[i][j] != grid[i][j + 1]` (if it exists).
Return `true` if __all__ the cells satisfy these conditions, otherwise, return `false`.

__Example:__

Example 1:

![3142-1](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)

```text
Input: grid = [[1,0,2],[1,0,2]]

Output: true

Explanation:

All the cells in the grid satisfy the conditions.
```

![3142-2](https://assets.leetcode.com/uploads/2024/03/27/example21.png)

Example 2:

```text
Input: grid = [[1,1,1],[0,0,0]]

Output: false

Explanation:

All cells in the first row are equal.
```

![3142-3](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

Example 3:

```text
Input: grid = [[1],[2],[3]]

Output: false

Explanation:

Cells in the first column have different values.
```

__Constraints:__

- `1 <= n, m <= 10`
- `0 <= grid[i][j] <= 9`

__题目描述:__

给你一个大小为 `m x n` 的二维矩阵 `grid` 。你需要判断每一个格子 `grid[i][j]` 是否满足：

如果它下面的格子存在，那么它需要等于它下面的格子，也就是 `grid[i][j] == grid[i + 1][j]` 。
如果它右边的格子存在，那么它需要不等于它右边的格子，也就是 `grid[i][j] != grid[i][j + 1]` 。
如果 __所有__ 格子都满足以上条件，那么返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![3142-4](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)

```text
输入：grid = [[1,0,2],[1,0,2]]

输出：true

解释：

网格图中所有格子都符合条件。
```

示例 2：

![3142-5](https://assets.leetcode.com/uploads/2024/03/27/example21.png)

```text
输入：grid = [[1,1,1],[0,0,0]]

输出：false

解释：

同一行中的格子值都相等。
```

示例 3：

![3142-6](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

```text
输入：grid = [[1],[2],[3]]

输出：false

解释：

同一列中的格子值不相等。
```

__提示：__

- `1 <= n, m <= 10`
- `0 <= grid[i][j] <= 9`

__思路:__

```text
模拟
遍历整个矩阵
如果有右边的值比较是否相等, 如果相等返回 false
如果有下边的值比较是否不等, 如果不等返回 false
检查完所有矩阵返回 true, 只需要 1 次遍历
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool satisfiesConditions(vector<vector<int>>& grid) 
    {
        for (int i = 0, m = grid.size(), n = grid.front().size(); i < m; i++) for (int j = 0; j < n; j++) if (j and grid[i][j - 1] == grid[i][j] or i and grid[i - 1][j] != grid[i][j]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean satisfiesConditions(int[][] grid) {
        for (int i = 0, m = grid.length, n = grid[0].length; i < m; i++) for (int j = 0; j < n; j++) if (j > 0 && grid[i][j - 1] == grid[i][j] || i > 0 && grid[i - 1][j] != grid[i][j]) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def satisfiesConditions(self, grid: List[List[int]]) -> bool:
        return all((i < len(grid) - 1 and grid[i][j] == grid[i + 1][j] or i == len(grid) - 1) and (j < len(grid[0]) - 1 and grid[i][j] != grid[i][j + 1] or j == len(grid[0]) - 1) for i in range(len(grid)) for j in range(len(grid[0])))
```
