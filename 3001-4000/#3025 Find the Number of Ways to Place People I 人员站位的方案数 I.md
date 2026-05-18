# 3025 Find the Number of Ways to Place People I 人员站位的方案数 I

__Description:__

You are given a 2D array `points` of size `n x 2` representing integer coordinates of some points on a 2D plane, where `points[i] = [xi, yi]`.

Count the number of pairs of points `(A, B)`, where

- `A` is on the __upper left__ side of `B`, and
- there are no other points in the rectangle (or line) they make (__including the border__), except for the points `A` and `B`.

Return the count.

__Example:__

Example 1:

```text
Input: points = [[1,1],[2,2],[3,3]]

Output: 0

Explanation:
```

![3025-1](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

There is no way to choose `A` and `B` such that `A` is on the upper left side of `B`.

Example 2:

```text
Input: points = [[6,2],[4,4],[2,6]]

Output: 2

Explanation:
```

![3025-2](https://assets.leetcode.com/uploads/2024/06/25/t2.jpg)

- The left one is the pair `(points[1], points[0])`, where `points[1]` is on the upper left side of `points[0]` and the rectangle is empty.
- The middle one is the pair `(points[2], points[1])`, same as the left one it is a valid pair.
- The right one is the pair `(points[2], points[0])`, where `points[2]` is on the upper left side of `points[0]`, but `points[1]` is inside the rectangle so it's not a valid pair.

Example 3:

```text
Input: points = [[3,1],[1,3],[1,1]]

Output: 2

Explanation:
```

![3025-3](https://assets.leetcode.com/uploads/2024/06/25/t3.jpg)

- The left one is the pair `(points[2], points[0])`, where `points[2]` is on the upper left side of `points[0]` and there are no other points on the line they form. Note that it is a valid state when the two points form a line.
- The middle one is the pair `(points[1], points[2])`, it is a valid pair same as the left one.
- The right one is the pair `(points[1], points[0])`, it is not a valid pair as `points[2]` is on the border of the rectangle.

__Constraints:__

- `2 <= n <= 50`
- `points[i].length == 2`
- `0 <= points[i][0], points[i][1] <= 50`
- All `points[i]` are distinct.

__题目描述:__

给你一个  `n x 2` 的二维数组 `points` ，它表示二维平面上的一些点坐标，其中 `points[i] = [xi, yi]` 。

计算点对 `(A, B)` 的数量，其中

- `A` 在 `B` 的左上角，并且
- 它们形成的长方形中（或直线上）没有其它点（__包括边界__），除了点 `A` 和点 `B`。

返回数量。

__示例:__

示例 1：

```text
输入：points = [[1,1],[2,2],[3,3]]

输出：0

解释：
```

![3025-4](https://assets.leetcode.com/uploads/2024/01/04/example1alicebob.png)

没有办法选择 `A` 和 `B`，使得 `A` 在 `B` 的左上角。

示例 2：

```text
输入：points = [[6,2],[4,4],[2,6]]

输出：2

解释：
```

![3025-5](https://assets.leetcode.com/uploads/2024/06/25/t2.jpg)

- 左边的是点对 `(points[1], points[0])`，其中 `points[1]` 在 `points[0]` 的左上角，并且形成的长方形内部是空的。
- 中间的是点对 `(points[2], points[1])`，和左边的一样是合法的点对。
- 右边的是点对 `(points[2], points[0])`，其中 `points[2]` 在 `points[0]` 的左上角，但 `points[1]` 在长方形内部，所以不是一个合法的点对。

示例 3：

```text
输入：points = [[3,1],[1,3],[1,1]]

输出：2

解释：
```

![3025-6](https://assets.leetcode.com/uploads/2024/06/25/t3.jpg)

- 左边的是点对 `(points[2], points[0])`，其中 `points[2]` 在 `points[0]` 的左上角并且在它们形成的直线上没有其它点。注意两个点形成一条线的情况是合法的。
- 中间的是点对 `(points[1], points[2])`，和左边一样也是合法的点对。
- 右边的是点对 `(points[1], points[0])`，它不是合法的点对，因为 `points[2]` 在长方形的边上。

__提示：__

- `2 <= n <= 50`
- `points[i].length == 2`
- `0 <= points[i][0], points[i][1] <= 50`
- `points[i]` 点对两两不同。

__思路:__

```text
暴力法
先将所有坐标按照 x 升序 y 降序的方式排序
然后检查每一对坐标
这些坐标一定满足横坐标在前
所以只需要检查纵坐标是否满足左上的顺序
如果其他坐标没有落在两个点形成的矩形中累加答案
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPairs(vector<vector<int>>& points) 
    {
        int result = 0, n = points.size();
        sort(points.begin(), points.end(), [](auto& a, auto& b) { return a.front() != b.front() ? a.front() < b.front() : a.back() > b.back(); });
        auto check = [&](int i, int j) -> bool
        {
            for (int k = 0; k < n; k++) if (k != i && k != j) if (points[i][0] <= points[k][0] && points[k][0] <= points[j][0] && points[j][1] <= points[k][1] && points[k][1] <= points[i][1]) return false;
            return true;
        };
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (points[i].back() >= points[j].back()) result += check(i, j);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPairs(int[][] points) {
        int result = 0, n = points.length;
        Arrays.sort(points, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (points[i][1] >= points[j][1]) result += check(points, i, j);
        return result;
    }

    private int check(int[][] points, int i, int j) {
        for (int k = 0, n = points.length; k < n; k++) if (k != i && k != j) if (points[i][0] <= points[k][0] && points[k][0] <= points[j][0] && points[j][1] <= points[k][1] && points[k][1] <= points[i][1]) return 0;
        return 1;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPairs(self, points: List[List[int]]) -> int:
        return sum(all(not (points[i][0] <= x <= points[j][0] and points[j][1] <= y <= points[i][1]) for k, (x, y) in enumerate(points) if k != i and k != j) for i in range(len(points)) for j in range(i + 1, len(points)) if points[i][1] >= points[j][1]) if (points := sorted(points, key=lambda p: (p[0], -p[1]))) else 0
```
