# 812 Largest Triangle Area 最大三角形面积

__Description__:
You have a list of points in the plane. Return the area of the largest triangle that can be formed by any 3 of the points.

__Example:__

Input: points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
Output: 2
Explanation:
The five points are show in the figure below. The red triangle is the largest.

![triangle](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png)

__Notes:__

3 <= points.length <= 50.
No points will be duplicated.
 -50 <= points[i][j] <= 50.
Answers within 10^-6 of the true value will be accepted as correct.

__题目描述__:
给定包含多个点的集合，从其中取三个点组成三角形，返回能组成的最大三角形的面积。

__示例 :__

输入: points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
输出: 2
解释:
这五个点如下图所示。组成的橙色三角形是最大的，面积为2。

![三角形](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png)

__注意：__

3 <= points.length <= 50.
不存在重复的点。
 -50 <= points[i][j] <= 50.
结果误差值在 10^-6 以内都认为是正确答案。

__思路__:

三重循环, 三角形面积计算公式可以用 s = abs((x1 - x3) \* (y2 - y3) - (x2 - x3) \* (y1 - y3)) / 2, 或者用向量的叉积表示, 二维向量的叉积即为两向量的行列式
时间复杂度O(n ^ 3), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double largestTriangleArea(vector<vector<int>>& points) 
    {
        double result = 0, x1 = 0, y1 = 0, x2 = 0, y2 = 0;
        for (int i = 0; i < points.size() - 2; i++) for (int j = i + 1; j < points.size() - 1; j++) 
        {
            x1 = points[i][0] - points[j][0];
            y1 = points[i][1] - points[j][1];
            for (int k = j + 1; k < points.size(); k++) 
            {
                x2 = points[i][0] - points[k][0];
                y2 = points[i][1] - points[k][1];
                result = max(result, fabs(x1 * y2 - x2 * y1));
            }
        }
        return result / 2.0;
    }
};
```

__Java__:

```Java
class Solution {
    public double largestTriangleArea(int[][] points) {
        double result = 0, x1 = 0, y1 = 0, x2 = 0, y2 = 0;
        for (int i = 0; i < points.length - 2; i++) {
            for (int j = i + 1; j < points.length - 1; j++) {
                x1 = points[i][0] - points[j][0];
                y1 = points[i][1] - points[j][1];
                for (int k = j + 1; k < points.length; k++) {
                    x2 = points[i][0] - points[k][0];
                    y2 = points[i][1] - points[k][1];
                    result = Math.max(result, Math.abs(x1 * y2 - x2 * y1));
                }
            }
        }
        return result / 2.0;
    }
}
```

__Python__:

```Python
class Solution:
    def largestTriangleArea(self, points: List[List[int]]) -> float:
        return max([abs((p[0][0] - p[2][0]) * (p[1][1] - p[2][1]) - (p[1][0] -p[2][0]) * (p[0][1] - p[2][1])) for p in itertools.combinations(points, 3)]) / 2.0
```
