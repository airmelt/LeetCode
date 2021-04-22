# 391 Perfect Rectangle 完美矩形

__Description__:
Given N axis-aligned rectangles where N > 0, determine if they all together form an exact cover of a rectangular region.

Each rectangle is represented as a bottom-left point and a top-right point. For example, a unit square is represented as [1,1,2,2]. (coordinate of bottom-left point is (1, 1) and top-right point is (2, 2)).

__Example:__

Example 1:

![rectangle_perfect](https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [3,2,4,4],
  [1,3,2,4],
  [2,3,3,4]
]

Return true. All 5 rectangles together form an exact cover of a rectangular region.

Example 2:

![rectangle_separated](https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg)

rectangles = [
  [1,1,2,3],
  [1,3,2,4],
  [3,1,4,2],
  [3,2,4,4]
]

Return false. Because there is a gap between the two rectangular regions.

Example 3:

![rectangle_hole](https://assets.leetcode.com/uploads/2021/03/27/perfectrec3-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [3,2,4,4]
]

Return false. Because there is a gap in the top center.

Example 4:

![rectangle_intersect](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [2,2,4,4]
]

Return false. Because two of the rectangles overlap with each other.

__题目描述__:
我们有 N 个与坐标轴对齐的矩形, 其中 N > 0, 判断它们是否能精确地覆盖一个矩形区域。

每个矩形用左下角的点和右上角的点的坐标来表示。例如， 一个单位正方形可以表示为 [1,1,2,2]。 ( 左下角的点的坐标为 (1, 1) 以及右上角的点的坐标为 (2, 2) )。

__示例 :__

示例 1:

![完美矩形](https://assets.leetcode.com/uploads/2021/03/27/perectrec1-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [3,2,4,4],
  [1,3,2,4],
  [2,3,3,4]
]

返回 true。5个矩形一起可以精确地覆盖一个矩形区域。

示例 2:

![分隔矩形](https://assets.leetcode.com/uploads/2021/03/27/perfectrec2-plane.jpg)

rectangles = [
  [1,1,2,3],
  [1,3,2,4],
  [3,1,4,2],
  [3,2,4,4]
]

返回 false。两个矩形之间有间隔，无法覆盖成一个矩形。

示例 3:

![间隔矩形](https://assets.leetcode.com/uploads/2021/03/27/perfectrec3-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [3,2,4,4]
]

返回 false。图形顶端留有间隔，无法覆盖成一个矩形。

示例 4:

![相交矩形](https://assets.leetcode.com/uploads/2021/03/27/perfecrrec4-plane.jpg)

rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [2,2,4,4]
]

返回 false。因为中间有相交区域，虽然形成了矩形，但不是精确覆盖。

__思路__:

如果是完美矩形, 则四个角只出现一次, 其他的点成对出现
用一个哈希表记录 4个顶点的位置, 记录所有的矩形的面积和
最后要留下 4个顶点, 且面积和等于最后形成的矩形
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) 
    {
        set<pair<int,int>> s;
        int left = INT_MAX, bottom = INT_MAX, top = INT_MIN, right = INT_MIN, area = 0;
        for (auto &p : rectangles)
        {
            left = min(p[0], left);
            bottom = min(p[1], bottom);
            right = max(p[2], right);
            top = max(p[3],top);
            area += (p[2] - p[0]) * (p[3] - p[1]);
            vector<pair<int,int>> temp({make_pair(p[0], p[1]), make_pair(p[0], p[3]), make_pair(p[2], p[1]), make_pair(p[2], p[3])});
            for (auto point : temp)
            {
                if (s.count(point)) s.erase(point);
                else s.insert(point);
            }
        }
        if (s.size() == 4 and s.count(make_pair(left,bottom)) and s.count(make_pair(left,top)) and s.count(make_pair(right,top)) and s.count(make_pair(right,bottom)))
           return area == (right - left) * (top - bottom);
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        int left = Integer.MAX_VALUE, right = Integer.MIN_VALUE, top = Integer.MIN_VALUE, bottom = Integer.MAX_VALUE, n = rectangles.length, area = 0;
        Set<String> set = new HashSet<>();
        for (int i = 0; i < n; i++) {
            left = Math.min(left, rectangles[i][0]);
            bottom = Math.min(bottom, rectangles[i][1]);
            right = Math.max(right, rectangles[i][2]);
            top = Math.max(top, rectangles[i][3]);
            area += (rectangles[i][3] - rectangles[i][1]) * (rectangles[i][2] - rectangles[i][0]);
            String a = rectangles[i][0] + " " + rectangles[i][3], b = rectangles[i][0] + " " + rectangles[i][1], c = rectangles[i][2] + " " + rectangles[i][3], d = rectangles[i][2] + " " + rectangles[i][1];
            if (set.contains(a)) set.remove(a);
            else set.add(a);
            if (set.contains(b)) set.remove(b);
            else set.add(b);
            if (set.contains(c)) set.remove(c);
            else set.add(c);
            if (set.contains(d)) set.remove(d);
            else set.add(d);
        }
        if (set.size() == 4 && set.contains(left + " " + top) && set.contains(left + " " + bottom) && set.contains(right + " " + bottom) && set.contains(right + " " + top)) return area == (right - left) * (top - bottom);
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isRectangleCover(self, rectangles: List[List[int]]) -> bool:
        left, right, bottom, top, area, s = float('inf'), -float('inf'), float('inf'), -float('inf'), 0, set()
        for p in rectangles:
            left, right, bottom, top = min(left, p[0]), max(right, p[2]), min(bottom, p[1]), max(top, p[3])
            area += (p[2] - p[0]) * (p[3] - p[1])
            temp = [(p[0], p[1]), (p[0], p[3]), (p[2], p[1]), (p[2], p[3])]
            for point in temp:
                if point in s:
                    s.remove(point)
                else:
                    s.add(point)
        if len(s) == 4 and (left, bottom) in s and (left, top) in s and (right, top) in s and (right, bottom) in s:
            return area == (right - left) * (top - bottom)
        return False
```
