# 1401 Circle and Rectangle Overlapping 圆和矩形是否有重叠

__Description:__

You are given a circle represented as (radius, xCenter, yCenter) and an axis-aligned rectangle represented as (x1, y1, x2, y2), where (x1, y1) are the coordinates of the bottom-left corner, and (x2, y2) are the coordinates of the top-right corner of the rectangle.

Return true if the circle and rectangle are overlapped otherwise return false. In other words, check if there is any point (xi, yi) that belongs to the circle and the rectangle at the same time.

__Example:__

Example 1:

![1401-1](https://assets.leetcode.com/uploads/2020/02/20/sample_4_1728.png)

Input: radius = 1, xCenter = 0, yCenter = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1
Output: true
Explanation: Circle and rectangle share the point (1,0).

Example 2:

Input: radius = 1, xCenter = 1, yCenter = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1
Output: false

Example 3:

![1401-2](https://assets.leetcode.com/uploads/2020/02/20/sample_2_1728.png)

Input: radius = 1, xCenter = 0, yCenter = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1
Output: true

__Constraints:__

1 <= radius <= 2000
-10^4 <= xCenter, yCenter <= 10^4
-10^4 <= x1 < x2 <= 10^4
-10^4 <= y1 < y2 <= 10^4

__题目描述:__

给你一个以 (radius, x_center, y_center) 表示的圆和一个与坐标轴平行的矩形 (x1, y1, x2, y2)，其中 (x1, y1) 是矩形左下角的坐标，(x2, y2) 是右上角的坐标。

如果圆和矩形有重叠的部分，请你返回 True ，否则返回 False 。

换句话说，请你检测是否 存在 点 (xi, yi) ，它既在圆上也在矩形上（两者都包括点落在边界上的情况）。

__示例:__

示例 1：

![1401-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/04/sample_4_1728.png)

输入：radius = 1, x_center = 0, y_center = 0, x1 = 1, y1 = -1, x2 = 3, y2 = 1
输出：true
解释：圆和矩形有公共点 (1,0)

示例 2：

![1401-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/04/sample_2_1728.png)

输入：radius = 1, x_center = 0, y_center = 0, x1 = -1, y1 = 0, x2 = 0, y2 = 1
输出：true

示例 3：

![1401-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/04/04/sample_6_1728.png)

输入：radius = 1, x_center = 1, y_center = 1, x1 = -3, y1 = -3, x2 = 3, y2 = 3
输出：true

示例 4：

输入：radius = 1, x_center = 1, y_center = 1, x1 = 1, y1 = -3, x2 = 2, y2 = -1
输出：false

__提示：__

1 <= radius <= 2000
-10^4 <= x_center, y_center, x1, y1, x2, y2 <= 10^4
x1 < x2
y1 < y2

__思路:__

```text
数学
以矩形中点为原点建立直角坐标系, 第一象限的顶点为 p = (x0, y0)
其中 x0 = abs(x2 - x1) / 2, y0 = abs(y2 - y1) / 2
由于圆相对于矩形的四个角都是对称的
不妨将圆映射到第一象限方便计算 c = (xp, yp) 为圆的圆心
xp = abs(x_center), yp = abs(y_center)
那么圆心到矩形的最近距离可以用 p - c 向量减法表示
为了方便计算最短距离, 上述向量的负部都设为 0
最后判断向量的长度与圆的半径大小关系即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkOverlap(int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) 
    {
        return (x1 = max({0, x1 - xCenter, xCenter - x2})) * x1 + (y1 = max({0, y1 - yCenter, yCenter - y2})) * y1 <= radius * radius;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkOverlap(int radius, int xCenter, int yCenter, int x1, int y1, int x2, int y2) {
        return Math.max(0, Math.max(x1 - xCenter, xCenter - x2)) * Math.max(0, Math.max(x1 - xCenter, xCenter - x2)) + Math.max(0, Math.max(y1 - yCenter, yCenter - y2)) * Math.max(0, Math.max(y1 - yCenter, yCenter - y2)) <= radius * radius;
    }
}
```

__Python__:

```Python
class Solution:
    def checkOverlap(self, radius: int, xCenter: int, yCenter: int, x1: int, y1: int, x2: int, y2: int) -> bool:
        return (dx := max(0, x1 - xCenter, xCenter - x2)) * dx + (dy := max(0, y1 - yCenter, yCenter - y2)) * dy <= radius * radius
```
