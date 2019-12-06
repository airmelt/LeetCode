__Description__:
A boomerang is a set of 3 points that are all distinct and not in a straight line.

Given a list of three points in the plane, return whether these points are a boomerang.

__Example:__
Example 1:

Input: [[1,1],[2,3],[3,2]]
Output: true
Example 2:

Input: [[1,1],[2,2],[3,3]]
Output: false
 
__Note:__

points.length == 3
points[i].length == 2
0 <= points[i][j] <= 100

__题目描述__:
回旋镖定义为一组三个点，这些点各不相同且不在一条直线上。

给出平面上三个点组成的列表，判断这些点是否可以构成回旋镖。

__示例 :__
示例 1：

输入：[[1,1],[2,3],[3,2]]
输出：true

示例 2：

输入：[[1,1],[2,2],[3,3]]
输出：false
 
__提示：__

points.length == 3
points[i].length == 2
0 <= points[i][j] <= 100

__思路__:
本质上就是求斜率
直线斜率 k = (y1 - y0) / (x1 - x0) = (yi - y0) / (xi - x0)转化为乘法
(y1 - y0) * (xi - x0) = (yi - y0) * (x1 - x0)
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool isBoomerang(vector<vector<int>>& points) 
    {
        return (points[1][1] - points[0][1]) * (points[2][0] - points[0][0]) != (points[2][1] - points[0][1]) * (points[1][0] - points[0][0]);
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isBoomerang(int[][] points) {
        return (points[1][1] - points[0][1]) * (points[2][0] - points[0][0]) != (points[2][1] - points[0][1]) * (points[1][0] - points[0][0]);
    }
}
```

__Python__:
```Python
class Solution:
    def isBoomerang(self, points: List[List[int]]) -> bool:
        return (points[1][1] - points[0][1]) * (points[2][0] - points[0][0]) != (points[2][1] - points[0][1]) * (points[1][0] - points[0][0])
```