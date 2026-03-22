# 963 Minimum Area Rectangle II 最小面积矩形 II

__Description__:
You are given an array of points in the X-Y plane points where points[i] = [xi, yi].

Return the minimum area of any rectangle formed from these points, with sides not necessarily parallel to the X and Y axes. If there is not any such rectangle, return 0.

Answers within 10-5 of the actual answer will be accepted.

__Example:__

Example 1:

![Rectangle 1](https://assets.leetcode.com/uploads/2018/12/21/1a.png)

Input: points = [[1,2],[2,1],[1,0],[0,1]]
Output: 2.00000
Explanation: The minimum area rectangle occurs at [1,2],[2,1],[1,0],[0,1], with an area of 2.

Example 2:

![Rectangle 2](https://assets.leetcode.com/uploads/2018/12/22/2.png)

Input: points = [[0,1],[2,1],[1,1],[1,0],[2,0]]
Output: 1.00000
Explanation: The minimum area rectangle occurs at [1,0],[1,1],[2,1],[2,0], with an area of 1.

Example 3:

![Rectangle 3](https://assets.leetcode.com/uploads/2018/12/22/3.png)

Input: points = [[0,3],[1,2],[3,1],[1,3],[2,1]]
Output: 0
Explanation: There is no possible rectangle to form from these points.

Example 4:

![Rectangle 4](https://assets.leetcode.com/uploads/2018/12/21/4c.png)

Input: points = [[3,1],[1,1],[0,1],[2,1],[3,3],[3,2],[0,2],[2,3]]
Output: 2.00000
Explanation: The minimum area rectangle occurs at [2,1],[2,3],[3,3],[3,1], with an area of 2.

__Constraints:__

1 <= points.length <= 50
points[i].length == 2
0 <= xi, yi <= 4 * 10^4
All the given points are unique.

__题目描述__:
给定在 xy 平面上的一组点，确定由这些点组成的任何矩形的最小面积，其中矩形的边不一定平行于 x 轴和 y 轴。

如果没有任何矩形，就返回 0。

__示例 :__

示例 1：

![矩形 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/22/1a.png)

输入：[[1,2],[2,1],[1,0],[0,1]]
输出：2.00000
解释：最小面积的矩形出现在 [1,2],[2,1],[1,0],[0,1] 处，面积为 2。

示例 2：

![矩形 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/23/2.png)

输入：[[0,1],[2,1],[1,1],[1,0],[2,0]]
输出：1.00000
解释：最小面积的矩形出现在 [1,0],[1,1],[2,1],[2,0] 处，面积为 1。

示例 3：

![矩形 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/23/3.png)

输入：[[0,3],[1,2],[3,1],[1,3],[2,1]]
输出：0
解释：没法从这些点中组成任何矩形。

示例 4：

![矩形 4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/21/4c.png)

输入：[[3,1],[1,1],[0,1],[2,1],[3,3],[3,2],[0,2],[2,3]]
输出：2.00000
解释：最小面积的矩形出现在 [2,1],[2,3],[3,3],[3,1] 处，面积为 2。

__提示:__

1 <= points.length <= 50
0 <= points[i][0] <= 40000
0 <= points[i][1] <= 40000
所有的点都是不同的。
与真实值误差不超过 10^-5 的答案将视为正确结果。

__思路__:

查找对角线的交点
矩形的对角线互相平分且长度相等
通过三个点确定第四个点, 然后比较中点及对角线长度
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double minAreaFreeRect(vector<vector<int>>& points) 
    {
        set<long> s;
        double result = 16e8;
        for (const auto& point : points) s.insert(point.front() * 40000 + point.back());
        for (int i = 0, n = points.size(); i < n - 1; i++) 
        {
            int x1 = points[i].front(), y1 = points[i].back();
            for (int j = i + 1; j < n; j++) 
            {
                int x2 = points[j].front(), y2 = points[j].back(), xsum = x1 + x2, ysum = y1 + y2;
                for (int k = 0; k < n; k++) 
                {
                    int x3 = points[k].front(), y3 = points[k].back(), x4 = xsum - x3, y4 = ysum - y3;
                    if (k != i and k != j) if (s.count((long)x4 * 40000 + y4) and pow(x1 - x2, 2) + pow(y1 - y2, 2) == pow(x3 - x4, 2) + pow(y3 - y4, 2)) result = min(result, sqrt(pow(x1 - x3, 2) + pow(y1 - y3, 2)) * sqrt(pow(x2 - x3, 2) + pow(y2 - y3, 2)));
                }
            }
        }
        return result == 16e8 ? 0 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public double minAreaFreeRect(int[][] points) {
        Set<Integer> set = new HashSet<>();
        double result = Double.MAX_VALUE;
        for (int[] point : points) set.add(point[0] * 40000 + point[1]);
        for (int i = 0, n = points.length; i < n - 1; i++) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < n; j++) {
                int x2 = points[j][0], y2 = points[j][1], xsum = x1 + x2, ysum = y1 + y2;
                for (int k = 0; k < n; k++) {
                    int x3 = points[k][0], y3 = points[k][1], x4 = xsum - x3, y4 = ysum - y3;
                    if (k != i && k != j) if (set.contains(x4 * 40000 + y4) && Math.pow(x1 - x2, 2) + Math.pow(y1 - y2, 2) == Math.pow(x3 - x4, 2) + Math.pow(y3 - y4, 2)) result = Math.min(result, Math.sqrt(Math.pow(x1 - x3, 2) + Math.pow(y1 - y3, 2)) * Math.sqrt(Math.pow(x2 - x3, 2) + Math.pow(y2 - y3, 2)));
                }
            }
        }
        return result == Double.MAX_VALUE ? 0 : result;
    }
}
```

__Python__:

```Python
import numpy as np
class Solution:
    def minAreaFreeRect(self, points: List[List[int]]) -> float:
        points, result, d = [*map(np.array, points)], float('inf'), defaultdict(list)
        for x, y in combinations(points, 2):
            d[tuple(x + y), np.linalg.norm(x - y)] += [x - y]
        for idxs in d.values():
            for x, y in combinations(idxs, 2):
                result = min(result, abs(np.cross(x, y)))
        return result / 2 if result < float('inf') else 0
```
