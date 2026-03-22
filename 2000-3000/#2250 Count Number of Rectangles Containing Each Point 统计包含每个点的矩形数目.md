# 2250 Count Number of Rectangles Containing Each Point 统计包含每个点的矩形数目

__Description:__

You are given a 2D integer array `rectangles` where `rectangles[i] = [li, hi]` indicates that `i ^ th` rectangle has a length of `li` and a height of `hi`. You are also given a 2D integer array `points` where `points[j] = [xj, yj]` is a point with coordinates `(xj, yj)`.

The `i ^ th` rectangle has its __bottom-left corner__ point at the coordinates `(0, 0)` and its __top-right corner__ point at `(li, hi)`.

Return _an integer array_ `count` _of length_ `points.length` _where_ `count[j]` _is the number of rectangles that __contain__ the_ `j ^ th` _point._

The `i ^ th` rectangle __contains__ the `j ^ th` point if `0 <= xj <= li` and `0 <= yj <= hi`. Note that points that lie on the __edges__ of a rectangle are also considered to be contained by that rectangle.

__Example:__

Example 1:

![2250-1](https://assets.leetcode.com/uploads/2022/03/02/example1.png)

```text
Input: rectangles = [[1,2],[2,3],[2,5]], points = [[2,1],[1,4]]
Output: [2,1]
Explanation: 
The first rectangle contains no points.
The second rectangle contains only the point (2, 1).
The third rectangle contains the points (2, 1) and (1, 4).
The number of rectangles that contain the point (2, 1) is 2.
The number of rectangles that contain the point (1, 4) is 1.
Therefore, we return [2, 1].
```

Example 2:

![2250-2](https://assets.leetcode.com/uploads/2022/03/02/example2.png)

```text
Input: rectangles = [[1,1],[2,2],[3,3]], points = [[1,3],[1,1]]
Output: [1,3]
Explanation:
The first rectangle contains only the point (1, 1).
The second rectangle contains only the point (1, 1).
The third rectangle contains the points (1, 3) and (1, 1).
The number of rectangles that contain the point (1, 3) is 1.
The number of rectangles that contain the point (1, 1) is 3.
Therefore, we return [1, 3].
```

__Constraints:__

- `1 <= rectangles.length, points.length <= 5 * 10 ^ 4`
- `rectangles[i].length == points[j].length == 2`
- `1 <= li, xj <= 10 ^ 9`
- `1 <= hi, yj <= 100`
- All the `rectangles` are __unique__.
- All the `points` are __unique__.

__题目描述:__

给你一个二维整数数组 `rectangles` ，其中 `rectangles[i] = [li, hi]` 表示第 `i` 个矩形长为 `li` 高为 `hi` 。给你一个二维整数数组 `points` ，其中 `points[j] = [xj, yj]` 是坐标为 `(xj, yj)` 的一个点。

第 `i` 个矩形的 __左下角__ 在 `(0, 0)` 处，__右上角__ 在 `(li, hi)` 。

请你返回一个整数数组 `count` ，长度为 `points.length`，其中 `count[j]`是 __包含__ 第 `j` 个点的矩形数目。

如果 `0 <= xj <= li` 且 `0 <= yj <= hi` ，那么我们说第 `i` 个矩形包含第 `j` 个点。如果一个点刚好在矩形的 __边上__ ，这个点也被视为被矩形包含。

__示例:__

示例 1：

![2250-3](https://assets.leetcode.com/uploads/2022/03/02/example1.png)

```text
输入：rectangles = [[1,2],[2,3],[2,5]], points = [[2,1],[1,4]]
输出：[2,1]
解释：
第一个矩形不包含任何点。
第二个矩形只包含一个点 (2, 1) 。
第三个矩形包含点 (2, 1) 和 (1, 4) 。
包含点 (2, 1) 的矩形数目为 2 。
包含点 (1, 4) 的矩形数目为 1 。
所以，我们返回 [2, 1] 。
```

示例 2：

![2250-4](https://assets.leetcode.com/uploads/2022/03/02/example2.png)

```text
输入：rectangles = [[1,1],[2,2],[3,3]], points = [[1,3],[1,1]]
输出：[1,3]
解释：
第一个矩形只包含点 (1, 1) 。
第二个矩形只包含点 (1, 1) 。
第三个矩形包含点 (1, 3) 和 (1, 1) 。
包含点 (1, 3) 的矩形数目为 1 。
包含点 (1, 1) 的矩形数目为 3 。
所以，我们返回 [1, 3] 。
```

__提示：__

- `1 <= rectangles.length, points.length <= 5 * 10 ^ 4`
- `rectangles[i].length == points[j].length == 2`
- `1 <= li, xj <= 10 ^ 9`
- `1 <= hi, yj <= 100`
- 所有 `rectangles` __互不相同__ 。
- 所有 `points` __互不相同__ 。

__思路:__

```text
1. 按行统计
由于 y 的范围为 [0, 100], 因此可以按行统计每个 y 坐标上的矩形的 x 坐标
将每个 x 映射到对应的 y 的数组中, 并按照 x 坐标排序
对于每个点, 从 y 坐标开始遍历
使用二分查找统计包含该点的矩形数目
时间复杂度为 O(NlogN + MlogN), 空间复杂度为 O(N)
2. 按横坐标排序
按照横坐标降序排序矩形
对于点 (x, y) 统计横坐标大于等于 x 的矩形
按照横坐标降序遍历点
累计高度大于等于 y 的矩形数目
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(M)
3. 按纵坐标排序
按照纵坐标降序排序矩形和点
对于每个点, 统计高度大于等于 y 的矩形
并把横坐标加入有序列表 xs 中
也可以加入列表之后再排序
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countRectangles(vector<vector<int>>& rectangles, vector<vector<int>>& points) 
    {
        sort(rectangles.begin(), rectangles.end(), [](auto &a, auto &b) { return a.back() > b.back(); });
        int n = points.size(), m = rectangles.size(),i = 0;
        vector<int> ids(n), result(n), xs;
        iota(ids.begin(), ids.end(), 0);
        sort(ids.begin(), ids.end(), [&](int a, int b) { return points[a].back() > points[b].back(); });
        for (int id : ids) 
        {
            int start = i;
            while (i < m and rectangles[i].back() >= points[id].back()) xs.push_back(rectangles[i++].front());
            if (start < i) sort(xs.begin(), xs.end());
            result[id] = xs.end() - lower_bound(xs.begin(), xs.end(), points[id].front());
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countRectangles(int[][] rectangles, int[][] points) {
        Arrays.sort(rectangles, (a, b) -> b[0] - a[0]);
        int n = points.length, m = rectangles.length, result[] = new int[n], count[] = new int[101], i = 0;
        var ids = IntStream.range(0, n).boxed().toArray(Integer[]::new);
        Arrays.sort(ids, (a, b) -> points[b][0] - points[a][0]);
        for (int id : ids) {
            while (i < m && rectangles[i][0] >= points[id][0]) ++count[rectangles[i++][1]];
            for (int j = points[id][1]; j < 101; j++) result[id] += count[j];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countRectangles(self, rectangles: List[List[int]], points: List[List[int]]) -> List[int]:
        max_y = max(y for _, y in rectangles)
        xs = [[] for _ in range(max_y + 1)]
        for x, y in rectangles:
            xs[y].append(x)
        for x in xs:
            x.sort()
        return [sum(len(x) - bisect_left(x, px) for x in xs[py:]) for px, py in points]
```
