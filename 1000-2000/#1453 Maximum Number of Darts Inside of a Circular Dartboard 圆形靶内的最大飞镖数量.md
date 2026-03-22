# 1453 Maximum Number of Darts Inside of a Circular Dartboard 圆形靶内的最大飞镖数量

__Description:__

Alice is throwing  `n` darts on a very large wall. You are given an array  `darts` where  `darts[i] = [xi, yi]` is the position of the  `i ^ th` dart that Alice threw on the wall.

Bob knows the positions of the  `n` darts on the wall. He wants to place a dartboard of radius  `r` on the wall so that the maximum number of darts that Alice throws lies on the dartboard.

Given the integer  `r`, return _the maximum number of darts that can lie on the dartboard_.

__Example:__

Example 1:

![1453-1](https://assets.leetcode.com/uploads/2020/04/29/sample_1_1806.png)

```text
Input: darts = [[-2,0],[2,0],[0,2],[0,-2]], r = 2
Output: 4
Explanation: Circle dartboard with center in (0,0) and radius = 2 contain all points.
```

Example 2:

![1453-2](https://assets.leetcode.com/uploads/2020/04/29/sample_2_1806.png)

```text
Input: darts = [[-3,0],[3,0],[2,6],[5,4],[0,9],[7,8]], r = 5
Output: 5
Explanation: Circle dartboard with center in (0,4) and radius = 5 contain all points except the point (7,8).
```

__Constraints:__

- `1 <= darts.length <= 100`
- `darts[i].length == 2`
- `-10 ^ 4 <= xi, yi <= 10 ^ 4`
- `1 <= r <= 5000`

__题目描述:__

墙壁上挂着一个圆形的飞镖靶。现在请你蒙着眼睛向靶上投掷飞镖。

投掷到墙上的飞镖用二维平面上的点坐标数组表示。飞镖靶的半径为  `r` 。

请返回能够落在 __任意__ 半径为  `r` 的圆形靶内或靶上的最大飞镖数。

__示例:__

示例 1：

![1453-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/sample_1_1806.png)

```text
输入：points = [[-2,0],[2,0],[0,2],[0,-2]], r = 2
输出：4
解释：如果圆形的飞镖靶的圆心为 (0,0) ，半径为 2 ，所有的飞镖都落在靶上，此时落在靶上的飞镖数最大，值为 4 。
```

示例 2：

![1453-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/16/sample_2_1806.png)

```text
输入：points = [[-3,0],[3,0],[2,6],[5,4],[0,9],[7,8]], r = 5
输出：5
解释：如果圆形的飞镖靶的圆心为 (0,4) ，半径为 5 ，则除了 (7,8) 之外的飞镖都落在靶上，此时落在靶上的飞镖数最大，值为 5 。
```

示例 3：

```text
输入：points = [[-2,0],[2,0],[0,2],[0,-2]], r = 1
输出：1
```

示例 4：

```text
输入：points = [[1,2],[3,5],[1,-1],[2,3],[4,1],[1,3]], r = 2
输出：4
```

__提示：__

- `1 <= points.length <= 100`
- `points[i].length == 2`
- `-10 ^ 4 <= points[i][0], points[i][1] <= 10 ^ 4`
- `1 <= r <= 5000`

__思路:__

```text
数学
给定半径和圆上两点求圆心坐标
连接两点, 圆心一定在两点的中垂线上
先求出两点的连线的中点, 在利用向量点乘为 0 以及半径求出两个圆心
分别遍历两个圆内的点的数量求最大值
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    const double EPS = 1e-8;
    
    int count(double x, double y, double r, vector<vector<int>>& points) 
    {
        int result = 0;
        for (const auto& point: points) result += ((point.front() - x) * (point.front() - x) + (point.back() - y) * (point.back() - y) <= r * r + EPS);
        return result;
    }
public:
    int numPoints(vector<vector<int>>& darts, int r) 
    {
        int result = 1, n = darts.size();
        for (int i = 0; i < n; i++) 
        {
            for (int j = i + 1; j < n; j++) 
            {
                double x1 = darts[i].front(), y1 = darts[i].back(), x2 = darts[j].front(), y2 = darts[j].back();
                double d = sqrt(pow((x2 - x1), 2) + pow((y2 - y1), 2));
                double mid1 = (x1 + x2) / 2, mid2 = (y1 + y2) / 2;
                double s = sqrt(r * r - d * d / 4);
                double p = s * (y1 - y2) / d, q = s * (x2 - x1) / d;
                double c1 = mid1 + p, c2 = mid2 + q, c3 = mid1 - p, c4 = mid2 - q;
                result = (c1 != c3 or c2 != c4) ? max(result, max(count(c1, c2, r, darts), count(c3, c4, r, darts))) : max(result, count(c1, c2, r, darts));
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private final static double EPS = 1e-8;
    
    public int numPoints(int[][] darts, int r) {
        int result = 1, n = darts.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                double x1 = darts[i][0], y1 = darts[i][1], x2 = darts[j][0], y2 = darts[j][1];
                double d = Math.sqrt(Math.pow((x2 - x1), 2) + Math.pow((y2 - y1), 2));
                double mid1 = (x1 + x2) / 2, mid2 = (y1 + y2) / 2;
                double s = Math.sqrt(r * r - d * d / 4);
                double p = s * (y1 - y2) / d, q = s * (x2 - x1) / d;
                double c1 = mid1 + p, c2 = mid2 + q, c3 = mid1 - p, c4 = mid2 - q;
                result = (c1 != c3 || c2 != c4) ? Math.max(result, Math.max(count(c1, c2, r, darts), count(c3, c4, r, darts))) : Math.max(result, count(c1, c2, r, darts));
            }
        }
        return result;
    }
    
    private int count(double x, double y, double r, int[][] points) {
        int result = 0;
        for (int[] point: points) if ((point[0] - x) * (point[0] - x) + (point[1] - y) * (point[1] - y) <= r * r + EPS) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numPoints(self, darts: List[List[int]], r: int) -> int:
        f, result = lambda o: sum(dist(p, o) <= r for p in darts), 1
        for p1 in darts:
            for p2 in darts:
                if 0 < dist(p1, p2) <= 2 * r:
                    result = max(result, f([(s := [(p1[0] + p2[0]) / 2, (p1[1] + p2[1]) / 2])[0] + (d := [(p1[0] - p2[0]) / 2, (p1[1] - p2[1]) / 2])[1] * (z := sqrt(r ** 2 - d[0] ** 2 - d[1] ** 2) / hypot(*d)), s[1] - d[0] * z]))
        return result
```
