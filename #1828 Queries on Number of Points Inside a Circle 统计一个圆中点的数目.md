# 1828 Queries on Number of Points Inside a Circle 统计一个圆中点的数目

__Description:__

You are given an array `points` where `points[i] = [xi, yi]` is the coordinates of the `i ^ th` point on a 2D plane. Multiple points can have the __same__ coordinates.

You are also given an array `queries` where `queries[j] = [xj, yj, rj]` describes a circle centered at `(xj, yj)` with a radius of `rj`.

For each query `queries[j]`, compute the number of points __inside__ the `j ^ th` circle. Points __on the border__ of the circle are considered __inside__.

Return _an array_ `answer`_, where_ `answer[j]` _is the answer to the_ `j ^ th` _query_.

__Example:__

Example 1:

![1828-1](https://assets.leetcode.com/uploads/2021/03/25/chrome_2021-03-25_22-34-16.png)

```text
Input: points = [[1,3],[3,3],[5,3],[2,2]], queries = [[2,3,1],[4,3,1],[1,1,2]]
Output: [3,2,2]
Explanation: The points and circles are shown above.
queries[0] is the green circle, queries[1] is the red circle, and queries[2] is the blue circle.
```

Example 2:

![1828-2](https://assets.leetcode.com/uploads/2021/03/25/chrome_2021-03-25_22-42-07.png)

```text
Input: points = [[1,1],[2,2],[3,3],[4,4],[5,5]], queries = [[1,2,2],[2,2,2],[4,3,2],[4,3,3]]
Output: [2,3,2,4]
Explanation: The points and circles are shown above.
queries[0] is green, queries[1] is red, queries[2] is blue, and queries[3] is purple.
```

__Constraints:__

- `1 <= points.length <= 500`
- `points[i].length == 2`
- `0 <= x​​​​​​i, y​​​​​​i <= 500`
- `1 <= queries.length <= 500`
- `queries[j].length == 3`
- `0 <= xj, yj <= 500`
- `1 <= rj <= 500`
- All coordinates are integers.

__Follow up:__ Could you find the answer for each query in better complexity than `O(n)`?

__题目描述:__

给你一个数组 `points` ，其中 `points[i] = [xi, yi]` ，表示第 `i` 个点在二维平面上的坐标。多个点可能会有 __相同__ 的坐标。

同时给你一个数组 `queries` ，其中 `queries[j] = [xj, yj, rj]` ，表示一个圆心在 `(xj, yj)` 且半径为 `rj` 的圆。

对于每一个查询 `queries[j]` ，计算在第 `j` 个圆 __内__ 点的数目。如果一个点在圆的 __边界上__ ，我们同样认为它在圆 __内__ 。

请你返回一个数组 `answer` ，其中 `answer[j]`是第 `j` 个查询的答案。

__示例:__

示例 1：

![1828-3](https://assets.leetcode.com/uploads/2021/03/25/chrome_2021-03-25_22-34-16.png)

```text
输入：points = [[1,3],[3,3],[5,3],[2,2]], queries = [[2,3,1],[4,3,1],[1,1,2]]
输出：[3,2,2]
解释：所有的点和圆如上图所示。
queries[0] 是绿色的圆，queries[1] 是红色的圆，queries[2] 是蓝色的圆。
```

示例 2：

![1828-4](https://assets.leetcode.com/uploads/2021/03/25/chrome_2021-03-25_22-42-07.png)

```text
输入：points = [[1,1],[2,2],[3,3],[4,4],[5,5]], queries = [[1,2,2],[2,2,2],[4,3,2],[4,3,3]]
输出：[2,3,2,4]
解释：所有的点和圆如上图所示。
queries[0] 是绿色的圆，queries[1] 是红色的圆，queries[2] 是蓝色的圆，queries[3] 是紫色的圆。
```

__提示：__

- `1 <= points.length <= 500`
- `points[i].length == 2`
- `0 <= x​​​​​​i, y​​​​​​i <= 500`
- `1 <= queries.length <= 500`
- `queries[j].length == 3`
- `0 <= xj, yj <= 500`
- `1 <= rj <= 500`
- 所有的坐标都是整数。

__思路:__

```text
枚举
枚举所有的圆
利用公式 x^2 + y^2 <= r^2 判断点是否在圆内
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countPoints(vector<vector<int>>& points, vector<vector<int>>& queries) 
    {
        int n = queries.size();
        vector<int> result(n);
        for (int i = 0, cur = 0; i < n; i++, cur = 0) 
        {
            for (const auto& point : points) cur += (point[0] - queries[i][0]) * (point[0] - queries[i][0]) + (point[1] - queries[i][1]) * (point[1] - queries[i][1]) <= queries[i][2] * queries[i][2];
            result[i] += cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countPoints(int[][] points, int[][] queries) {
        int n = queries.length,result[] = new int[n];
        for (int i = 0, cur = 0; i < n; i++, cur = 0) {
            for (int[] point : points) if ((point[0] - queries[i][0]) * (point[0] - queries[i][0]) + (point[1] - queries[i][1]) * (point[1] - queries[i][1]) <= queries[i][2] * queries[i][2]) ++cur;
            result[i] += cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPoints(self, points: List[List[int]], queries: List[List[int]]) -> List[int]:
        return [sum((x0 - x1) ** 2 + (y0 - y1) ** 2 <= r1 ** 2 for x0, y0 in points) for x1, y1, r1 in queries]
```
