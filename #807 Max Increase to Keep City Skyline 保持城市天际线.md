# 807 Max Increase to Keep City Skyline 保持城市天际线

__Description__:
There is a city composed of n x n blocks, where each block contains a single building shaped like a vertical square prism. You are given a 0-indexed n x n integer matrix grid where grid[r][c] represents the height of the building located in the block at row r and column c.

A city's skyline is the the outer contour formed by all the building when viewing the side of the city from a distance. The skyline from each cardinal direction north, east, south, and west may be different.

We are allowed to increase the height of any number of buildings by any amount (the amount can be different per building). The height of a 0-height building can also be increased. However, increasing the height of a building should not affect the city's skyline from any cardinal direction.

Return the maximum total sum that the height of the buildings can be increased by without changing the city's skyline from any cardinal direction.

__Example:__

Example 1:

![City Skyline](https://assets.leetcode.com/uploads/2021/06/21/807-ex1.png)

Input: grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
Output: 35
Explanation: The building heights are shown in the center of the above image.
The skylines when viewed from each cardinal direction are drawn in red.
The grid after increasing the height of buildings without affecting skylines is:
gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]

Example 2:

Input: grid = [[0,0,0],[0,0,0],[0,0,0]]
Output: 0
Explanation: Increasing the height of any building will result in the skyline changing.

__Constraints:__

n == grid.length
n == grid[r].length
2 <= n <= 50
0 <= grid[r][c] <= 100

__题目描述__:
在二维数组grid中，grid[i][j]代表位于某处的建筑物的高度。 我们被允许增加任何数量（不同建筑物的数量可能不同）的建筑物的高度。 高度 0 也被认为是建筑物。

最后，从新数组的所有四个方向（即顶部，底部，左侧和右侧）观看的“天际线”必须与原始数组的天际线相同。 城市的天际线是从远处观看时，由所有建筑物形成的矩形的外部轮廓。 请看下面的例子。

建筑物高度可以增加的最大总和是多少？

__示例 :__

输入： grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
输出： 35
解释：
The grid is:
[ [3, 0, 8, 4],
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

从数组竖直方向（即顶部，底部）看“天际线”是：[9, 4, 8, 7]
从水平水平方向（即左侧，右侧）看“天际线”是：[8, 7, 9, 3]

在不影响天际线的情况下对建筑物进行增高后，新数组如下：

gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]

__说明:__

1 < grid.length = grid[0].length <= 50。
 grid[i][j] 的高度范围是： [0, 100]。
一座建筑物占据一个grid[i][j]：换言之，它们是 1 x 1 x grid[i][j] 的长方体。

__思路__:

模拟
记录下每一行每一列的最大值
取每个元素对应列对应行的最大值的较小值减去自身就是能增加的高度
时间复杂度为 O(mn), 空间复杂度为 O(max(m, n))

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) 
    {
        int result = 0, m = grid.size(), n = grid[0].size();
        vector<int> r(m), c(n);
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                r[i] = max(r[i], grid[i][j]);
                c[j] = max(c[j], grid[i][j]);
            }
        } 
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result += min(r[i], c[j]) - grid[i][j];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxIncreaseKeepingSkyline(int[][] grid) {
        int result = 0, m = grid.length, n = grid[0].length, r[] = new int[m], c[] = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                r[i] = Math.max(r[i], grid[i][j]);
                c[j] = Math.max(c[j], grid[i][j]);
            }
        } 
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result += Math.min(r[i], c[j]) - grid[i][j];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxIncreaseKeepingSkyline(self, grid: List[List[int]]) -> int:
        return sum(min((r := [max(i) for i in grid])[i], (c := [max(i) for i in zip(*grid)])[j]) - grid[i][j] for i in range(len(grid)) for j in range(len(grid)))
```
