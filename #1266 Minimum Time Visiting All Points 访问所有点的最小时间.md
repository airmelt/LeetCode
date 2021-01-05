# 1266 Minimum Time Visiting All Points 访问所有点的最小时间

__Description__:
On a plane there are n points with integer coordinates points[i] = [xi, yi]. Your task is to find the minimum time in seconds to visit all points.

You can move according to the next rules:

In one second always you can either move vertically, horizontally by one unit or diagonally (it means to move one unit vertically and one unit horizontally in one second).
You have to visit the points in the same order as they appear in the array.

__Example:__

Example 1:
![Coordinate](https://upload-images.jianshu.io/upload_images/16639143-0716fc9ca81eab25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: points = [[1,1],[3,4],[-1,0]]
Output: 7
Explanation: One optimal path is [1,1] -> [2,2] -> [3,3] -> [3,4] -> [2,3] -> [1,2] -> [0,1] -> [-1,0]
Time from [1,1] to [3,4] = 3 seconds
Time from [3,4] to [-1,0] = 4 seconds
Total time = 7 seconds

Example 2:

Input: points = [[3,2],[-2,2]]
Output: 5

__Constraints:__

points.length == n
1 <= n <= 100
points[i].length == 2
-1000 <= points[i][0], points[i][1] <= 1000

__题目描述__:
平面上有 n 个点，点的位置用整数坐标表示 points[i] = [xi, yi]。请你计算访问所有这些点需要的最小时间（以秒为单位）。

你可以按照下面的规则在平面上移动：

每一秒沿水平或者竖直方向移动一个单位长度，或者跨过对角线（可以看作在一秒内向水平和竖直方向各移动一个单位长度）。
必须按照数组中出现的顺序来访问这些点。

__示例 :__

示例 1：
![坐标系](https://upload-images.jianshu.io/upload_images/16639143-8273fb7cf4e036ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：points = [[1,1],[3,4],[-1,0]]
输出：7
解释：一条最佳的访问路径是： [1,1] -> [2,2] -> [3,3] -> [3,4] -> [2,3] -> [1,2] -> [0,1] -> [-1,0]
从 [1,1] 到 [3,4] 需要 3 秒
从 [3,4] 到 [-1,0] 需要 4 秒
一共需要 7 秒

示例 2：

输入：points = [[3,2],[-2,2]]
输出：5

__提示：__

points.length == n
1 <= n <= 100
points[i].length == 2
-1000 <= points[i][0], points[i][1] <= 1000

__思路__:

必须按照顺序访问, 考察连续的两个点, 只要找到横纵坐标距离差的绝对值较大的就是所需时间
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) 
    {
        int result = 0;
        for (int i = 1; i < points.size(); i++) result += max(abs(points[i - 1][0] - points[i][0]), abs(points[i - 1][1] - points[i][1]));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minTimeToVisitAllPoints(int[][] points) {
        int result = 0;
        for (int i = 1; i < points.length; i++) result += Math.max(Math.abs(points[i - 1][0] - points[i][0]), Math.abs(points[i - 1][1] - points[i][1]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        return sum((max(abs(points[i - 1][0] - points[i][0]), abs(points[i - 1][1] - points[i][1])) for i in range(1, len(points))))
```
