# 3102 Minimize Manhattan Distances 最小化曼哈顿距离

__Description:__

You are given an array `points` representing integer coordinates of some points on a 2D plane, where `points[i] = [xi, yi]`.

The distance between two points is defined as their Manhattan distance.

Return the minimum possible value for maximum distance between any two points by removing exactly one point.

__Example:__

Example 1:

```text
Input: points = [[3,10],[5,15],[10,2],[4,4]]

Output: 12

Explanation:

The maximum distance after removing each point is the following:
```

- After removing the 0 ^ th point the maximum distance is between points (5, 15) and (10, 2), which is `|5 - 10| + |15 - 2| = 18`.
- After removing the 1 ^ st point the maximum distance is between points (3, 10) and (10, 2), which is `|3 - 10| + |10 - 2| = 15`.
- After removing the 2 ^ nd point the maximum distance is between points (5, 15) and (4, 4), which is `|5 - 4| + |15 - 4| = 12`.
- After removing the 3 ^ rd point the maximum distance is between points (5, 15) and (10, 2), which is `|5 - 10| + |15 - 2| = 18`.

12 is the minimum possible maximum distance between any two points after removing exactly one point.

Example 2:

```text
Input: points = [[1,1],[1,1],[1,1]]

Output: 0

Explanation:

Removing any of the points results in the maximum distance between any two points of 0.
```

__Constraints:__

- `3 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `1 <= points[i][0], points[i][1] <= 10 ^ 8`

__题目描述:__

给你一个下标从 __0__ 开始的数组 `points` ，它表示二维平面上一些点的整数坐标，其中 `points[i] = [xi, yi]` 。

两点之间的距离定义为它们的曼哈顿距离。

请你恰好移除一个点，返回移除后任意两点之间的 最大 距离可能的 最小 值。

__示例:__

示例 1：

```text
输入：points = [[3,10],[5,15],[10,2],[4,4]]
输出：12
解释：移除每个点后的最大距离如下所示：
```

- 移除第 0 个点后，最大距离在点 (5, 15) 和 (10, 2) 之间，为 |5 - 10| + |15 - 2| = 18 。
- 移除第 1 个点后，最大距离在点 (3, 10) 和 (10, 2) 之间，为 |3 - 10| + |10 - 2| = 15 。
- 移除第 2 个点后，最大距离在点 (5, 15) 和 (4, 4) 之间，为 |5 - 4| + |15 - 4| = 12 。
- 移除第 3 个点后，最大距离在点 (5, 15) 和 (10, 2) 之间的，为 |5 - 10| + |15 - 2| = 18 。

在恰好移除一个点后，任意两点之间的最大距离可能的最小值是 12 。

示例 2：

```text
输入：points = [[1,1],[1,1],[1,1]]
输出：0
解释：移除任一点后，任意两点之间的最大距离都是 0 。
```

__提示：__

- `3 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `1 <= points[i][0], points[i][1] <= 10 ^ 8`

__思路:__

```text
数学
需要将曼哈顿距离转换为切比雪夫距离
即 (x, y) -> (x + y, y - x) 
令新坐标为 x', y'
这样求曼哈顿距离 |x1 - x2| + |y1 - y2| 就转化为求切比雪夫距离 max(|x'1 - x'2|, |y'1 - y'2|)
即 max(max(x') - min(x'), max(y') - min(y'))
所以只需要维护 x’ 的最大值和次大值, 最小值和次小值以及 y' 对应的 4 个值
遍历两次 points 计算上述值即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDistance(vector<vector<int>>& points) 
    {
        int inf = 0x3f3f3f3f, result = inf, max_x1 = -inf, max_x2 = -inf, min_x1 = inf, min_x2 = inf, max_y1 = -inf, max_y2 = -inf, min_y1 = inf, min_y2 = inf, x = 0, y = 0;
        for (const auto& point : points)
        {
            update(point.front() + point.back(), max_x1, max_x2, 1);
            update(point.front() + point.back(), min_x1, min_x2, 0);
            update(point.back() - point.front(), max_y1, max_y2, 1);
            update(point.back() - point.front(), min_y1, min_y2, 0);
        }
        for (const auto& point : points) result = min(result, max(((x = point.front() + point.back()) == max_x1 ? max_x2 : max_x1) - (x == min_x1 ? min_x2 : min_x1), ((y = point.back() - point.front()) == max_y1 ? max_y2 : max_y1) - (y == min_y1 ? min_y2 : min_y1)));
        return result;
    }
private:
    void update(int cur, int& m1, int& m2, bool larger)
    {
        if (larger ? cur > m1 : cur < m1)
        {
            m2 = m1;
            m1 = cur;
        }
        else if (larger ? cur > m2 : cur < m2)
        {
            m2 = cur;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDistance(int[][] points) {
        int inf = 0x3f3f3f3f, maxX1 = -inf, maxX2 = -inf, maxY1 = -inf, maxY2 = -inf, minX1 = inf, minX2 = inf, minY1 = inf, minY2 = inf, result = inf, x = 0, y = 0;
        for (var point : points) {
            if ((x = point[0] + point[1]) > maxX1) {
                maxX2 = maxX1;
                maxX1 = x;
            } else if (x > maxX2) {
                maxX2 = x;
            }
            if (x < minX1) {
                minX2 = minX1;
                minX1 = x;
            } else if (x < minX2) {
                minX2 = x;
            }
            if ((y = point[1] - point[0]) > maxY1) {
                maxY2 = maxY1;
                maxY1 = y;
            } else if (y > maxY2) {
                maxY2 = y;
            }
            if (y < minY1) {
                minY2 = minY1;
                minY1 = y;
            } else if (y < minY2) {
                minY2 = y;
            }
        }
        for (var point : points) result = Math.min(result, Math.max(((x = point[0] + point[1]) == maxX1 ? maxX2 : maxX1) - (x == minX1 ? minX2 : minX1), ((y = point[1] - point[0]) == maxY1 ? maxY2 : maxY1) - (y == minY1 ? minY2 : minY1)));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDistance(self, points: List[List[int]]) -> int:
        return min(max((max_x2 if (x + y) == max_x1 else max_x1) - (min_x2 if (x + y) == min_x1 else min_x1), (max_y2 if (y - x) == max_y1 else max_y1) - (min_y2 if (y - x) == min_y1 else min_y1)) for x, y in points) if (max_x1 := nlargest(2, (x + y for x, y in points))[0]) is not None and (max_x2 := nlargest(2, (x + y for x, y in points))[1]) and (min_x1 := nsmallest(2, (x + y for x, y in points))[0]) is not None and (min_x2 := nsmallest(2, (x + y for x, y in points))[1]) is not None and (max_y1 := nlargest(2, (y - x for x, y in points))[0]) is not None and (max_y2 := nlargest(2, (y - x for x, y in points))[1]) is not None and (min_y1 := nsmallest(2, (y - x for x, y in points))[0]) is not None and (min_y2 := nsmallest(2, (y - x for x, y in points))[1]) is not None else 0
```
