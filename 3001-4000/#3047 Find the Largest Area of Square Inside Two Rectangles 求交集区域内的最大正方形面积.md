# 3047 Find the Largest Area of Square Inside Two Rectangles 求交集区域内的最大正方形面积

__Description:__

There exist `n` rectangles in a 2D plane with edges parallel to the x and y axis. You are given two 2D integer arrays `bottomLeft` and `topRight` where `bottomLeft[i] = [a_i, b_i]` and `topRight[i] = [c_i, d_i]` represent the __bottom-left__ and __top-right__ coordinates of the `i ^ th` rectangle, respectively.

You need to find the __maximum__ area of a __square__ that can fit inside the intersecting region of at least two rectangles. Return `0` if such a square does not exist.

__Example:__

Example 1:

![3047-1](https://assets.leetcode.com/uploads/2024/01/05/example12.png)

```text
Input: bottomLeft = [[1,1],[2,2],[3,1]], topRight = [[3,3],[4,4],[6,6]]

Output: 1

Explanation:

A square with side length 1 can fit inside either the intersecting region of rectangles 0 and 1 or the intersecting region of rectangles 1 and 2. Hence the maximum area is 1. It can be shown that a square with a greater side length can not fit inside any intersecting region of two rectangles.
```

Example 2:

![3047-2](https://assets.leetcode.com/uploads/2024/07/15/diag.png)

```text
Input: bottomLeft = [[1,1],[1,3],[1,5]], topRight = [[5,5],[5,7],[5,9]]

Output: 4

Explanation:

A square with side length 2 can fit inside either the intersecting region of rectangles 0 and 1 or the intersecting region of rectangles 1 and 2. Hence the maximum area is `2 * 2 = 4`. It can be shown that a square with a greater side length can not fit inside any intersecting region of two rectangles.
```

Example 3:

![3047-3](https://assets.leetcode.com/uploads/2024/01/04/rectanglesexample2.png)

```text
Input: bottomLeft = [[1,1],[2,2],[1,2]], topRight = [[3,3],[4,4],[3,4]]

Output: 1

Explanation:

A square with side length 1 can fit inside the intersecting region of any two rectangles. Also, no larger square can, so the maximum area is 1. Note that the region can be formed by the intersection of more than 2 rectangles.
```

Example 4:

![3047-4](https://assets.leetcode.com/uploads/2024/01/04/rectanglesexample3.png)

```text
Input: bottomLeft = [[1,1],[3,3],[3,1]], topRight = [[2,2],[4,4],[4,2]]

Output: 0

Explanation:

No pair of rectangles intersect, hence, the answer is 0.
```

__Constraints:__

- `n == bottomLeft.length == topRight.length`
- `2 <= n <= 10 ^ 3`
- `bottomLeft[i].length == topRight[i].length == 2`
- `1 <= bottomLeft[i][0], bottomLeft[i][1] <= 10 ^ 7`
- `1 <= topRight[i][0], topRight[i][1] <= 10 ^ 7`
- `bottomLeft[i][0] < topRight[i][0]`
- `bottomLeft[i][1] < topRight[i][1]`

__题目描述:__

在二维平面上存在 `n` 个矩形。给你两个下标从 __0__ 开始的二维整数数组 `bottomLeft` 和 `topRight`，两个数组的大小都是 `n x 2` ，其中 `bottomLeft[i]` 和 `topRight[i]` 分别代表第 `i` 个矩形的 __左下角__ 和 __右上角__ 坐标。

我们定义 向右 的方向为 x 轴正半轴（x 坐标增加），向左 的方向为 x 轴负半轴（x 坐标减少）。同样地，定义 向上 的方向为 y 轴正半轴（y 坐标增加），向下 的方向为 y 轴负半轴（y 坐标减少）。

你可以选择一个区域，该区域由两个矩形的 交集 形成。你需要找出能够放入该区域 内 的 最大 正方形面积，并选择最优解。

返回能够放入交集区域的正方形的 __最大__ 可能面积，如果矩形之间不存在任何交集区域，则返回 `0`。

__示例:__

示例 1：

![3047-5](https://assets.leetcode.com/uploads/2024/01/05/example12.png)

```text
输入：bottomLeft = [[1,1],[2,2],[3,1]], topRight = [[3,3],[4,4],[6,6]]
输出：1
解释：边长为 1 的正方形可以放入矩形 0 和矩形 1 的交集区域，或矩形 1 和矩形 2 的交集区域。因此最大面积是边长 * 边长，即 1 * 1 = 1。
可以证明，边长更大的正方形无法放入任何交集区域。
```

示例 2：

![3047-6](https://assets.leetcode.com/uploads/2024/01/04/rectanglesexample2.png)

```text
输入：bottomLeft = [[1,1],[2,2],[1,2]], topRight = [[3,3],[4,4],[3,4]]
输出：1
解释：边长为 1 的正方形可以放入矩形 0 和矩形 1，矩形 1 和矩形 2，或所有三个矩形的交集区域。因此最大面积是边长 * 边长，即 1 * 1 = 1。
可以证明，边长更大的正方形无法放入任何交集区域。
请注意，区域可以由多于两个矩形的交集构成。
```

示例 3：

![3047-7](https://assets.leetcode.com/uploads/2024/01/04/rectanglesexample3.png)

```text
输入：bottomLeft = [[1,1],[3,3],[3,1]], topRight = [[2,2],[4,4],[4,2]]
输出：0
解释：不存在相交的矩形，因此，返回 0 。
```

__提示：__

- `n == bottomLeft.length == topRight.length`
- `2 <= n <= 10 ^ 3`
- `bottomLeft[i].length == topRight[i].length == 2`
- `1 <= bottomLeft[i][0], bottomLeft[i][1] <= 10 ^ 7`
- `1 <= topRight[i][0], topRight[i][1] <= 10 ^ 7`
- `bottomLeft[i][0] < topRight[i][0]`
- `bottomLeft[i][1] < topRight[i][1]`

__思路:__

```text
枚举
枚举所有的矩形对
设矩形 1 为 (x1, y1), (x2, y2)
矩形 2 为 (x3, y3), (x4, y4)
那么正方形的边长为 min(x2, x4) - max(x1, x3) 和 min(y2, y4) - max(y1, y3) 的较小值
初始化结果为 0 表示没有合适的正方形
最后返回边长的平方即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long largestSquareArea(vector<vector<int>>& bottomLeft, vector<vector<int>>& topRight) 
    {
        int result = 0, n = bottomLeft.size();
        for (int i = 0; i < n; i++) for (int x1 = bottomLeft[i].front(), x2 = topRight[i].front(), y1 = bottomLeft[i].back(), y2 = topRight[i].back(), j = i + 1; j < n; j++) result = max(result, min(min(x2, topRight[j].front()) - max(x1, bottomLeft[j].front()), min(y2, topRight[j].back()) - max(y1, bottomLeft[j].back())));
        return (long long)result * result;
    }
};
```

__Java__:

```Java
class Solution {
    public long largestSquareArea(int[][] bottomLeft, int[][] topRight) {
        int result = 0, n = bottomLeft.length;
        for (int i = 0; i < n; i++) for (int x1 = bottomLeft[i][0], x2 = topRight[i][0], y1 = bottomLeft[i][1], y2 = topRight[i][1], j = i + 1; j < n; j++) result = Math.max(result, Math.min(Math.min(x2, topRight[j][0]) - Math.max(x1, bottomLeft[j][0]), Math.min(y2, topRight[j][1]) - Math.max(y1, bottomLeft[j][1])));
        return (long)result * result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestSquareArea(self, bottomLeft: List[List[int]], topRight: List[List[int]]) -> int:
        return max(max(min(min(x2, x4) - max(x1, x3), min(y2, y4) - max(y1, y3)) for i, ((x1, y1), (x2, y2)) in enumerate(zip(bottomLeft, topRight)) for ((x3, y3), (x4, y4)) in zip(bottomLeft[i + 1:], topRight[i + 1:])), 0) ** 2
```
