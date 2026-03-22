# 452 Minimum Number of Arrows to Burst Balloons 用最少数量的箭引爆气球

__Description__:
There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.

__Example:__

Example 1:

Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).

Example 2:

Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4

Example 3:

Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2

Example 4:

Input: points = [[1,2]]
Output: 1

Example 5:

Input: points = [[2,3],[2,3]]
Output: 1

__Constraints:__

0 <= points.length <= 10^4
points[i].length == 2
-2^31 <= xstart < xend <= 2^31 - 1

__题目描述__:
在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。

__示例 :__

示例 1：

输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球

示例 2：

输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4

示例 3：

输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2

示例 4：

输入：points = [[1,2]]
输出：1

示例 5：

输入：points = [[2,3],[2,3]]
输出：1

__提示：__

0 <= points.length <= 10^4
points[i].length == 2
-2^31 <= xstart < xend <= 2^31 - 1

__思路__:

贪心算法
对区间的右端点排序
至少需要一支箭
每次从最右射出一支箭, 如果区间超出当前区间, 更新区间, 加一支箭
时间复杂度O(nlgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMinArrowShots(vector<vector<int>>& points) 
    {
        if (points.empty()) return 0;
        sort(points.begin(), points.end(), [](const vector<int>& a, const vector<int>& b) { return a[0] < b[0]; });
        int result = 1;
        for (int i = 1; i < points.size(); i++) 
        {
            if (points[i][0] > points[i - 1][1]) ++result;
            else points[i][1] = min(points[i - 1][1], points[i][1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) return 0;
        Arrays.sort(points, (a, b) -> a[1] < b[1] ? -1 : 1);
        int result = 1, cur = points[0][1];
        for (int i = 1; i < points.length; i++) if (cur < points[i][0]) {
            ++result;
            cur = points[i][1];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if not points: 
            return 0
        points.sort(key=lambda p: p[1])
        result, cur = 1, points[0][1]
        for point in points[1:]:
            if cur < point[0]:
                result += 1
                cur = point[1]
        return result
```
