# 463 Island Perimeter 岛屿的周长

__Description__:
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

__Example:__

Input:

```text
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
```

Output: 16

__Explanation:__ The perimeter is the 16 yellow stripes in the image below:

![Map](https://upload-images.jianshu.io/upload_images/16639143-169a44ba3698852b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__题目描述__:
给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

__示例 :__

输入:

```text
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
```

输出: 16

__解释__: 它的周长是下面图片中的 16 个黄色的边：
![地图](https://upload-images.jianshu.io/upload_images/16639143-169a44ba3698852b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__思路__:

注意到岛屿是封闭的

1. 每次遍历到岛屿就加 4, 然后除去中间的岛屿
2. 岛屿的周长是对称的, 只计算左边和上边的岛屿再乘 2
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int islandPerimeter(vector<vector<int>>& grid) 
    {
       int result = 0;
       for (int i = 0; i < grid.size(); i++) for (int j = 0; j < grid[0].size(); j++) 
       {
            if (grid[i][j]) 
            {
                if (!i or !grid[i - 1][j]) result++;
                if (!j or !grid[i][j - 1]) result++;
            }
       }
       return result << 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int islandPerimeter(int[][] grid) {
       int result = 0;
       for (int i = 0; i < grid.length; i++) {
           for (int j = 0; j < grid[0].length; j++) {
               if (grid[i][j] == 1) {
                   result += 4;
                   if (i > 0 && grid[i - 1][j] == 1) result--;
                   if (i < grid.length - 1 && grid[i + 1][j] == 1) result--;
                   if (j > 0 && grid[i][j - 1] == 1) result--;
                   if (j < grid[0].length - 1 && grid[i][j + 1] == 1) result--;
               }
           }
       }
       return result;
    }
}
```

__Python__:

```Python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        result = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j]:
                    if not j or not grid[i][j - 1]:
                        result += 1
                    if not i or not grid[i - 1][j]:
                        result += 1
        return result * 2
```
