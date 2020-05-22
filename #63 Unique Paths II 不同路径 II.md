__Description__:
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?
![Paths II](https://upload-images.jianshu.io/upload_images/16639143-3c0db7062884e067.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
An obstacle and empty space is marked as 1 and 0 respectively in the grid.

__Note:__
m and n will be at most 100.

__Example:__
Example 1:

Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right

__题目描述__:
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？
![路径 II](https://upload-images.jianshu.io/upload_images/16639143-76549f8d8151e909.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
网格中的障碍物和空位置分别用 1 和 0 来表示。

__说明：__
m 和 n 的值均不超过 100。

__示例 :__
示例 1:

输入:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
输出: 2
解释:
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右

__思路__:
动态规划
1. 边界处理, 如果入口就有障碍, 直接返回
2. 第一个元素设置为 1, 表示可达, 初始化第一行/列, 如果可达, 标记为 1否则标记为 0
3. 对除第一行/列进行遍历, 如果该元素没有障碍,则 dp[i][j] = dp[i - 1][j] + dp[i][j - 1], 否则标记为 0表示不可达
4. 返回最后一个元素, dp数组可以直接用 obstacleGrid数组代替
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) 
    {
        if (obstacleGrid.empty() || obstacleGrid[0].empty() || obstacleGrid[0][0] == 1) return 0;
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        obstacleGrid[0][0] = 1;
        for (int i = 1; i < m; i++) obstacleGrid[i][0] = (obstacleGrid[i - 1][0] == 1 && obstacleGrid[i][0] == 0) ? 1 : 0;
        for (int i = 1; i < n; i++) obstacleGrid[0][i] = (obstacleGrid[0][i - 1] == 1 && obstacleGrid[0][i] == 0) ? 1 : 0;
        for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) obstacleGrid[i][j] = obstacleGrid[i][j] == 0 ? obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1] : 0;
        return obstacleGrid[m - 1][n - 1];
    }
};
```

__Java__:
```Java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        if (obstacleGrid.length == 0 || obstacleGrid[0].length == 0 || obstacleGrid[0][0] == 1) return 0;
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        obstacleGrid[0][0] = 1;
        for (int i = 1; i < m; i++) obstacleGrid[i][0] = (obstacleGrid[i - 1][0] == 1 && obstacleGrid[i][0] == 0) ? 1 : 0;
        for (int i = 1; i < n; i++) obstacleGrid[0][i] = (obstacleGrid[0][i - 1] == 1 && obstacleGrid[0][i] == 0) ? 1 : 0;
        for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) obstacleGrid[i][j] = obstacleGrid[i][j] == 0 ? obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1] : 0;
        return obstacleGrid[m - 1][n - 1];
    }
}
```

__Python__:
```Python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if not obstacleGrid or not obstacleGrid[0] or obstacleGrid[0][0] == 1:
            return 0
        m, n, obstacleGrid[0][0] = len(obstacleGrid), len(obstacleGrid[0]), 1
        for i in range(1, m):
            obstacleGrid[i][0] = 1 if obstacleGrid[i][0] == 0 and obstacleGrid[i - 1][0] == 1 else 0
        for i in range(1, n):
            obstacleGrid[0][i] = 1 if obstacleGrid[0][i] == 0 and obstacleGrid[0][i - 1] == 1 else 0
        for i in range(1, m):
            for j in range(1, n):
                obstacleGrid[i][j] = obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1] if not obstacleGrid[i][j] else 0
        return obstacleGrid[-1][-1]
```