# 936 Stamping The Sequence 戳印序列

__Description__:
You are given an array of points in the X-Y plane points where points[i] = [xi, yi].

Return the minimum area of a rectangle formed from these points, with sides parallel to the X and Y axes. If there is not any such rectangle, return 0.

__Example:__

Example 1:

![rec1](https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG)

Input: points = [[1,1],[1,3],[3,1],[3,3],[2,2]]
Output: 4

Example 2:

![rec2](https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG)

Input: points = [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
Output: 2

__Constraints:__

1 <= points.length <= 500
points[i].length == 2
0 <= xi, yi <= 4 * 10^4
All the given points are unique.

__题目描述__:
给定在 xy 平面上的一组点，确定由这些点组成的矩形的最小面积，其中矩形的边平行于 x 轴和 y 轴。

如果没有任何矩形，就返回 0。

__示例 :__

示例 1：

输入：[[1,1],[1,3],[3,1],[3,3],[2,2]]
输出：4

示例 2：

输入：[[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
输出：2

__提示:__

1 <= points.length <= 500
0 <= points[i][0] <= 40000
0 <= points[i][1] <= 40000
所有的点都是不同的。

__思路__:

按对角线遍历
遍历到 x1, y1 时, 检查 x2 != x1 and y2 != y1
然后检查 (x1, y2) 和 (x2, y1) 是否也存在
因为 x, y < 40001 可以用一维数字存储二维点加快查找速度
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minAreaRect(vector<vector<int>>& points) 
    {
        set<int> s;
        for (const auto& point : points) s.insert(40001 * point.front() + point.back());
        int result = INT_MAX, n = points.size();
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (points[i].front() != points[j].front() and points[i].back() != points[j].back()) if (s.count(40001 * points[i].front() + points[j].back()) and s.count(40001 * points[j].front() + points[i].back())) result = min(result, abs(points[j].front() - points[i].front()) * abs(points[j].back() - points[i].back()));
        return result < INT_MAX ? result : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAreaRect(int[][] points) {
        Set<Integer> set = new HashSet();
        for (int[] point : points) set.add(40001 * point[0] + point[1]);
        int result = Integer.MAX_VALUE, n = points.length;
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (points[i][0] != points[j][0] && points[i][1] != points[j][1]) if (set.contains(40001 * points[i][0] + points[j][1]) && set.contains(40001 * points[j][0] + points[i][1])) result = Math.min(result, Math.abs(points[j][0] - points[i][0]) * Math.abs(points[j][1] - points[i][1]));
        return result < Integer.MAX_VALUE ? result : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def minAreaRect(self, points: List[List[int]]) -> int:
        result, s = float('inf'), set()
        for x1, y1 in points:
            for x2, y2 in s:
                if (x2, y1) in s and (x1, y2) in s:
                    result = min(result, abs(x2 - x1) * abs(y2 - y1))
            s.add((x1, y1))
        return result if result < float('inf') else 0
```
