# 1637 Widest Vertical Area Between Two Points Containing No Points 两点之间不包含任何点的最宽垂直区域

__Description:__

Given `n` `points` on a 2D plane where `points[i] = [xi, yi]`, Return _the __widest vertical area__ between two points such that no points are inside the area._

A vertical area is an area of fixed-width extending infinitely along the y-axis (i.e., infinite height). The widest vertical area is the one with the maximum width.

Note that points on the edge of a vertical area are not considered included in the area.

__Example:__

Example 1:

![1637-1](https://assets.leetcode.com/uploads/2020/09/19/points3.png)

```text
Input: points = [[8,7],[9,9],[7,4],[9,7]]
Output: 1
Explanation: Both the red and the blue area are optimal.
```

Example 2:

```text
Input: points = [[3,1],[9,0],[1,0],[1,4],[5,3],[8,8]]
Output: 3
```

__Constraints:__

- `n == points.length`
- `2 <= n <= 10 ^ 5`
- `points[i].length == 2`
- `0 <= xi, yi <= 10 ^ 9`

__题目描述:__

给你 `n` 个二维平面上的点 `points` ，其中 `points[i] = [xi, yi]` ，请你返回两点之间内部不包含任何点的 __最宽垂直区域__ 的宽度。

垂直区域 的定义是固定宽度，而 y 轴上无限延伸的一块区域（也就是高度为无穷大）。 最宽垂直区域 为宽度最大的一个垂直区域。

请注意，垂直区域 边上 的点 不在 区域内。

__示例:__

示例 1：

![1637-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/31/points3.png)

```text
输入：points = [[8,7],[9,9],[7,4],[9,7]]
输出：1
解释：红色区域和蓝色区域都是最优区域。
```

示例 2：

```text
输入：points = [[3,1],[9,0],[1,0],[1,4],[5,3],[8,8]]
输出：3
```

__提示：__

- `n == points.length`
- `2 <= n <= 10 ^ 5`
- `points[i].length == 2`
- `0 <= xi, yi <= 10 ^ 9`

__思路:__

```text
排序
先按照横坐标排序
然后依次比较横坐标之间的差值
取差值的最大值
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要是排序的空间和时间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxWidthOfVerticalArea(vector<vector<int>>& points) 
    {
        sort(points.begin(), points.end());
        int result = 0, n = points.size();
        for (int i = 1; i < n; i++) result = max(points[i].front() - points[i - 1].front(), result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxWidthOfVerticalArea(int[][] points) {
        Arrays.sort(points, (a, b) -> a[0] - b[0]);
        int result = 0, n = points.length;
        for (int i = 1; i < n; i++) result = Math.max(points[i][0] - points[i - 1][0], result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxWidthOfVerticalArea(self, points: List[List[int]]) -> int:
        return max((d[i] - d[i - 1] for i in range(1, len(d))), default=0) if (d := sorted({x for x, y in points})) else 0
```
