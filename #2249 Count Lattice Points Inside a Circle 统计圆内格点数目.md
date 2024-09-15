# 2249 Count Lattice Points Inside a Circle 统计圆内格点数目

__Description:__

Given a 2D integer array `circles` where `circles[i] = [xi, yi, ri]` represents the center `(xi, yi)` and radius `ri` of the `i ^ th` circle drawn on a grid, return _the __number of lattice points___ _that are present inside __at least one__ circle_.

Note:

- A __lattice point__ is a point with integer coordinates.
- Points that lie __on the circumference of a circle__ are also considered to be inside it.

__Example:__

Example 1:

![2249-1](https://assets.leetcode.com/uploads/2022/03/02/exa-11.png)

```text
Input: circles = [[2,2,1]]
Output: 5
Explanation:
The figure above shows the given circle.
The lattice points present inside the circle are (1, 2), (2, 1), (2, 2), (2, 3), and (3, 2) and are shown in green.
Other points such as (1, 1) and (1, 3), which are shown in red, are not considered inside the circle.
Hence, the number of lattice points present inside at least one circle is 5.
```

Example 2:

![2249-2](https://assets.leetcode.com/uploads/2022/03/02/exa-22.png)

```text
Input: circles = [[2,2,2],[3,4,1]]
Output: 16
Explanation:
The figure above shows the given circles.
There are exactly 16 lattice points which are present inside at least one circle. 
Some of them are (0, 2), (2, 0), (2, 4), (3, 2), and (4, 4).
```

__Constraints:__

- `1 <= circles.length <= 200`
- `circles[i].length == 3`
- `1 <= xi, yi <= 100`
- `1 <= ri <= min(xi, yi)`

__题目描述:__

给你一个二维整数数组 `circles` ，其中 `circles[i] = [xi, yi, ri]` 表示网格上圆心为 `(xi, yi)` 且半径为 `ri` 的第 `i` 个圆，返回出现在 __至少一个__ 圆内的 __格点数目__ 。

注意：

- __格点__ 是指整数坐标对应的点。
- __圆周上的点__ 也被视为出现在圆内的点。

__示例:__

示例 1：

![2249-3](https://assets.leetcode.com/uploads/2022/03/02/exa-11.png)

```text
输入：circles = [[2,2,1]]
输出：5
解释：
给定的圆如上图所示。
出现在圆内的格点为 (1, 2)、(2, 1)、(2, 2)、(2, 3) 和 (3, 2)，在图中用绿色标识。
像 (1, 1) 和 (1, 3) 这样用红色标识的点，并未出现在圆内。
因此，出现在至少一个圆内的格点数目是 5 。
```

示例 2：

![2249-4](https://assets.leetcode.com/uploads/2022/03/02/exa-22.png)

```text
输入：circles = [[2,2,2],[3,4,1]]
输出：16
解释：
给定的圆如上图所示。
共有 16 个格点出现在至少一个圆内。
其中部分点的坐标是 (0, 2)、(2, 0)、(2, 4)、(3, 2) 和 (4, 4) 。
```

__提示：__

- `1 <= circles.length <= 200`
- `circles[i].length == 3`
- `1 <= xi, yi <= 100`
- `1 <= ri <= min(xi, yi)`

__思路:__

```text
1. 暴力法
对圆进行遍历找到 x 和 y 的最大值
题目保证了圆的半径不会超过 x 和 y 的最小值
所以所有格点的坐标范围为 [0, max(x + r)], [0, max(y + r)]
将圆按照半径降序, 可以剪枝
对于每个格点, 判断是否在圆内
使用距离公式 x ** 2 + y ** 2 <= r ** 2 即可
时间复杂度为 O(NM ^ 2), 空间复杂度为 O(1), M 为格点的最大值
2. 差分数组
对于每个圆
格点的范围为 [0, 200]
按照纵坐标 y + r 和 y - r 进行差分
进入圆的格点时加 1, 离开圆的格点时减 1
最后统计差分数组中大于 0 的格点数即可
时间复杂度为 O(M ^ 2), 空间复杂度为 O(M ^ 2), M 为格点的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countLatticePoints(vector<vector<int>>& circles) 
    {
        int n = circles.size(), result = 0, cur = 0;
        vector<vector<int>> d(202, vector<int>(202));
        for (int i = 0; i < n; i++) 
        {
            int x = circles[i][0], y = circles[i][1], r = circles[i][2], len = r;
            for (int up = 0; up <= r; up++) 
            {
                while (len * len + up * up > r * r) --len;
                ++d[y + up][x - len];
                --d[y + up][x + len + 1];
                ++d[y - up][x - len];
                --d[y - up][x + len + 1];
            }
        }
        for (int y = 0; y < 201; y++, cur = 0) 
        {
            for (int x = 0; x < 201; x++) 
            {
                cur += d[y][x];
                if (cur > 0) ++result;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countLatticePoints(int[][] circles) {
        int n = circles.length, result = 0, cur = 0, d[][] = new int[202][202];
        for (int i = 0; i < n; i++) {
            int x = circles[i][0], y = circles[i][1], r = circles[i][2], len = r;
            for (int up = 0; up <= r; up++) {
                while (len * len + up * up > r * r) --len;
                ++d[y + up][x - len];
                --d[y + up][x + len + 1];
                ++d[y - up][x - len];
                --d[y - up][x + len + 1];
            }
        }
        for (int y = 0; y < 201; y++, cur = 0) {
            for (int x = 0; x < 201; x++) {
                cur += d[y][x];
                if (cur > 0) ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countLatticePoints(self, circles: List[List[int]]) -> int:
        return sum(any((x - i) ** 2 + (y - j) ** 2 <= r ** 2 for x, y, r in circles) for i in range(max(x + r for x, _, r in circles) + 1) for j in range(max(y + r for x, y, r in circles) + 1))
```
