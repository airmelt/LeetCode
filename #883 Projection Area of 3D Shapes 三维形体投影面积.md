__Description__:
On a N * N grid, we place some 1 * 1 * 1 cubes that are axis-aligned with the x, y, and z axes.

Each value v = grid[i][j] represents a tower of v cubes placed on top of grid cell (i, j).

Now we view the projection of these cubes onto the xy, yz, and zx planes.

A projection is like a shadow, that maps our 3 dimensional figure to a 2 dimensional plane. 

Here, we are viewing the "shadow" when looking at the cubes from the top, the front, and the side.

Return the total area of all three projections.

__Example:__
Example 1:

Input: [[2]]
Output: 5

Example 2:

Input: [[1,2],[3,4]]
Output: 17
Explanation: 
Here are the three projections ("shadows") of the shape made with each axis-aligned plane.
![projection](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/02/shadow.png)

Example 3:

Input: [[1,0],[0,2]]
Output: 8

Example 4:

Input: [[1,1,1],[1,0,1],[1,1,1]]
Output: 14

Example 5:

Input: [[2,2,2],[2,1,2],[2,2,2]]
Output: 21
 
__Note:__

1 <= grid.length = grid[0].length <= 50
0 <= grid[i][j] <= 50

__题目描述__:
在 N * N 的网格中，我们放置了一些与 x，y，z 三轴对齐的 1 * 1 * 1 立方体。

每个值 v = grid[i][j] 表示 v 个正方体叠放在单元格 (i, j) 上。

现在，我们查看这些立方体在 xy、yz 和 zx 平面上的投影。

投影就像影子，将三维形体映射到一个二维平面上。

在这里，从顶部、前面和侧面看立方体时，我们会看到“影子”。

返回所有三个投影的总面积。

 __示例 :__
示例 1：

输入：[[2]]
输出：5

示例 2：

输入：[[1,2],[3,4]]
输出：17
解释：
这里有该形体在三个轴对齐平面上的三个投影(“阴影部分”)。
![投影](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/02/shadow.png)

示例 3：

输入：[[1,0],[0,2]]
输出：8

示例 4：

输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：14

示例 5：

输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：21
 
__提示：__

1 <= grid.length = grid[0].length <= 50
0 <= grid[i][j] <= 50

__思路__:
x方向上的投影是每个分量的最大值, y方向上的投影是转置之后的每个分量的最大值, z方向上的投影是该位置有方块( > 0), 记为 1
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution {
public:
    int projectionArea(vector<vector<int>>& grid) {
        int result = 0;
        for (int i = 0; i < grid.size(); i++) 
        {
            int r = 0, c = 0;
            for (int j = 0; j < grid[i].size(); j++) 
            {
                if (grid[i][j]) result++;
                r = max(r, grid[i][j]);
                c = max(c, grid[j][i]);
            }
            result += r + c;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int projectionArea(int[][] grid) {
        int result = 0;
        for (int i = 0; i < grid.length; i++) {
            int r = 0, c = 0;
            for (int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] > 0) result++;
                r = Math.max(r, grid[i][j]);
                c = Math.max(c, grid[j][i]);
            }
            result += r + c;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def projectionArea(self, grid: List[List[int]]) -> int:
        return sum((max(i) for i in grid)) + sum((1 for i in grid for j in i if j)) + sum((max(i) for i in zip(*grid)))
        # 评论解答
        # return sum([sum(map(max, grid)), sum(map(max, zip(*grid))), sum(v > 0 for row in grid for v in row)])
```