# 3111 Minimum Rectangles to Cover Points 覆盖所有点的最少矩形数目

__Description:__

You are given a 2D integer array `points`, where `points[i] = [xi, yi]`. You are also given an integer `w`. Your task is to __cover__ __all__ the given points with rectangles.

Each rectangle has its lower end at some point `(x1, 0)` and its upper end at some point `(x2, y2)`, where `x1 <= x2`, `y2 >= 0`, and the condition `x2 - x1 <= w` __must__ be satisfied for each rectangle.

A point is considered covered by a rectangle if it lies within or on the boundary of the rectangle.

Return an integer denoting the minimum number of rectangles needed so that each point is covered by at least one rectangle.

Note: A point may be covered by more than one rectangle.

__Example:__

Example 1:

![3111-1](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-20-33-05.png)

```text
Input: points = [[2,1],[1,0],[1,4],[1,8],[3,5],[4,6]], w = 1

Output: 2

Explanation:

The image above shows one possible placement of rectangles to cover the points:
```

- A rectangle with a lower end at `(1, 0)` and its upper end at `(2, 8)`
- A rectangle with a lower end at `(3, 0)` and its upper end at `(4, 8)`

Example 2:

![3111-2](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-18-59-12.png)

```text
Input: points = [[0,0],[1,1],[2,2],[3,3],[4,4],[5,5],[6,6]], w = 2

Output: 3

Explanation:

The image above shows one possible placement of rectangles to cover the points:
```

- A rectangle with a lower end at `(0, 0)` and its upper end at `(2, 2)`
- A rectangle with a lower end at `(3, 0)` and its upper end at `(5, 5)`
- A rectangle with a lower end at `(6, 0)` and its upper end at `(6, 6)`

Example 3:

![3111-3](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-20-24-03.png)

```text
Input: points = [[2,3],[1,2]], w = 0

Output: 2

Explanation:

The image above shows one possible placement of rectangles to cover the points:
```

- A rectangle with a lower end at `(1, 0)` and its upper end at `(1, 2)`
- A rectangle with a lower end at `(2, 0)` and its upper end at `(2, 3)`

__Constraints:__

- `1 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `0 <= xi == points[i][0] <= 10 ^ 9`
- `0 <= yi == points[i][1] <= 10 ^ 9`
- `0 <= w <= 10 ^ 9`
- All pairs `(xi, yi)` are distinct.

__题目描述:__

给你一个二维整数数组 `point` ，其中 `points[i] = [xi, yi]` 表示二维平面内的一个点。同时给你一个整数 `w` 。你需要用矩形 __覆盖所有__ 点。

每个矩形的左下角在某个点 `(x1, 0)` 处，且右上角在某个点 `(x2, y2)` 处，其中 `x1 <= x2` 且 `y2 >= 0` ，同时对于每个矩形都 __必须__ 满足 `x2 - x1 <= w` 。

如果一个点在矩形内或者在边上，我们说这个点被矩形覆盖了。

请你在确保每个点都 至少 被一个矩形覆盖的前提下，最少 需要多少个矩形。

注意：一个点可以被多个矩形覆盖。

__示例:__

示例 1：

![3111-4](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-20-33-05.png)

```text
输入：points = [[2,1],[1,0],[1,4],[1,8],[3,5],[4,6]], w = 1

输出：2

解释：

上图展示了一种可行的矩形放置方案：
```

- 一个矩形的左下角在 `(1, 0)` ，右上角在 `(2, 8)` 。
- 一个矩形的左下角在 `(3, 0)` ，右上角在 `(4, 8)` 。

示例 2：

![3111-5](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-18-59-12.png)

```text
输入：points = [[0,0],[1,1],[2,2],[3,3],[4,4],[5,5],[6,6]], w = 2

输出：3

解释：

上图展示了一种可行的矩形放置方案：
```

- 一个矩形的左下角在 `(0, 0)` ，右上角在 `(2, 2)` 。
- 一个矩形的左下角在 `(3, 0)` ，右上角在 `(5, 5)` 。
- 一个矩形的左下角在 `(6, 0)` ，右上角在 `(6, 6)` 。

示例 3：

![3111-6](https://assets.leetcode.com/uploads/2024/03/04/screenshot-from-2024-03-04-20-24-03.png)

```text
输入：points = [[2,3],[1,2]], w = 0

输出：2

解释：

上图展示了一种可行的矩形放置方案：
```

- 一个矩形的左下角在 `(1, 0)` ，右上角在 `(1, 2)` 。
- 一个矩形的左下角在 `(2, 0)` ，右上角在 `(2, 3)` 。

__提示：__

- `1 <= points.length <= 10 ^ 5`
- `points[i].length == 2`
- `0 <= xi == points[i][0] <= 10 ^ 9`
- `0 <= yi == points[i][1] <= 10 ^ 9`
- `0 <= w <= 10 ^ 9`
- 所有点坐标 `(xi, yi)` 互不相同。

__思路:__

```text
贪心
按照 x 的大小进行排序
先将第一个点分配给第一个矩形
并记录上一个矩形开始的横坐标 pre
如果当前 x - pre > w
说明需要一个新的矩形
更新 pre 和结果
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int minRectanglesToCoverPoints(vector<vector<int>>& points, int w) {
        sort(points.begin(), points.end());
        int result = 1, pre = points[0][0], n = points.size();
        for (int i = 1; i < n; i++) {
            if (points[i][0] - pre > w) {
                pre = points[i][0];
                ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minRectanglesToCoverPoints(int[][] points, int w) {
        Arrays.sort(points, (a, b) -> (a[0] - b[0]));
        int result = 1, pre = points[0][0], n = points.length;
        for (int i = 1; i < n; i++) {
            if (points[i][0] - pre > w) {
                pre = points[i][0];
                ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minRectanglesToCoverPoints(self, points: List[List[int]], w: int) -> int:
        points.sort()
        result, pre = 1, points[0][0]
        for x, y in points[1:]:
            if x - pre > w:
                pre = x
                result += 1
        return result
```
