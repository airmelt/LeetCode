# 3195 Find the Minimum Area to Cover All Ones I 包含所有 1 的最小矩形面积 I

__Description:__

You are given a 2D __binary__ array `grid`. Find a rectangle with horizontal and vertical sides with the __smallest__ area, such that all the 1's in `grid` lie inside this rectangle.

Return the __minimum__ possible area of the rectangle.

__Example:__

Example 1:

```text
Input: grid = [[0,1,0],[1,0,1]]

Output: 6

Explanation:
```

![3195-1](https://assets.leetcode.com/uploads/2024/05/08/examplerect0.png)

The smallest rectangle has a height of 2 and a width of 3, so it has an area of 2 * 3 = 6.

Example 2:

```text
Input: grid = [[1,0],[0,0]]

Output: 1

Explanation:
```

![3195-2](https://assets.leetcode.com/uploads/2024/05/08/examplerect1.png)

The smallest rectangle has both height and width 1, so its area is 1 * 1 = 1.

__Constraints:__

- `1 <= grid.length, grid[i].length <= 1000`
- `grid[i][j]` is either 0 or 1.
- The input is generated such that there is at least one 1 in `grid`.

__题目描述:__

给你一个二维 __二进制__ 数组 `grid`。请你找出一个边在水平方向和竖直方向上、面积 __最小__ 的矩形，并且满足 `grid` 中所有的 1 都在矩形的内部。

返回这个矩形可能的 __最小__ 面积。

__示例:__

示例 1：

```text
输入： grid = [[0,1,0],[1,0,1]]

输出： 6

解释：
```

![3195-3](https://assets.leetcode.com/uploads/2024/05/08/examplerect0.png)

这个最小矩形的高度为 2，宽度为 3，因此面积为 2 * 3 = 6。

示例 2：

```text
输入： grid = [[0,0],[1,0]]

输出： 1

解释：
```

![3195-4](https://assets.leetcode.com/uploads/2024/05/08/examplerect1.png)

这个最小矩形的高度和宽度都是 1，因此面积为 1 * 1 = 1。

__提示：__

- `1 <= grid.length, grid[i].length <= 1000`
- `grid[i][j]` 是 0 或 1。
- 输入保证 `grid` 中至少有一个 1 。

__思路:__

```text
贪心
记录这个 1 边界的
上下左右
上为所有 1 出现的最小行
其他以此类推
最后返回 (right - left + 1) * (down - up + 1)
时间复杂度为 O(NM), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumArea(vector<vector<int>>& grid) 
    {
        int left = INT_MAX, right = 0, up = INT_MAX, down = 0, m = grid.size(), n = grid.front().size();
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] == 1) 
                {
                    left = min(left, j);
                    right = max(right, j);
                    up = min(up, i);
                    down = max(down, i);
                }
            }
        }
        return (right - left + 1) * (down - up + 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumArea(int[][] grid) {
        int left = Integer.MAX_VALUE, right = 0, up = Integer.MAX_VALUE, down = 0, m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    left = Math.min(left, j);
                    right = Math.max(right, j);
                    up = Math.min(up, i);
                    down = Math.max(down, i);
                }
            }
        }
        return (right - left + 1) * (down - up + 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumArea(self, grid: List[List[int]]) -> int:
        left, up, right, down = inf, inf, -1, -1
        for i, row in enumerate(grid):
            for j, val in enumerate(row):
                if val:
                    left, up, right, down = min(left, j), min(up, i), max(right, j), max(i, down)
        return (right - left + 1) * (down - up + 1)
```
