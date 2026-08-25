# 3143 Maximum Points Inside the Square 正方形中的最多点数

__Description:__

You are given a 2D array `points` and a string `s` where, `points[i]` represents the coordinates of point `i`, and `s[i]` represents the __tag__ of point `i`.

A __valid__ square is a square centered at the origin `(0, 0)`, has edges parallel to the axes, and __does not__ contain two points with the same tag.

Return the __maximum__ number of points contained in a __valid__ square.

__Note:__

- A point is considered to be inside the square if it lies on or within the square's boundaries.
- The side length of the square can be zero.

__Example:__

Example 1:

![3143-1](https://assets.leetcode.com/uploads/2024/03/29/3708-tc1.png)

```text
Input: points = [[2,2],[-1,-2],[-4,4],[-3,1],[3,-3]], s = "abdca"

Output: 2

Explanation:

The square of side length 4 covers two points points[0] and points[1].
```

![3143-2](https://assets.leetcode.com/uploads/2024/03/29/3708-tc2.png)

Example 2:

```text
Input: points = [[1,1],[-2,-2],[-2,2]], s = "abb"

Output: 1

Explanation:

The square of side length 2 covers one point, which is points[0].
```

Example 3:

```text
Input: points = [[1,1],[-1,-1],[2,-2]], s = "ccd"

Output: 0

Explanation:

It's impossible to make any valid squares centered at the origin such that it covers only one point among points[0] and points[1].
```

__Constraints:__

- `1 <= s.length, points.length <= 10 ^ 5`
- `points[i].length == 2`
- `-10 ^ 9 <= points[i][0], points[i][1] <= 10 ^ 9`
- `s.length == points.length`
- `points` consists of distinct coordinates.
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个二维数组 `points` 和一个字符串 `s` ，其中 `points[i]` 表示第 `i` 个点的坐标，`s[i]` 表示第 `i` 个点的 __标签__ 。

如果一个正方形的中心在 `(0, 0)` ，所有边都平行于坐标轴，且正方形内 __不__ 存在标签相同的两个点，那么我们称这个正方形是 __合法__ 的。

请你返回 __合法__ 正方形中可以包含的 __最多__ 点数。

__注意：__

- 如果一个点位于正方形的边上或者在边以内，则认为该点位于正方形内。
- 正方形的边长可以为零。

__示例:__

示例 1：

![3143-3](https://assets.leetcode.com/uploads/2024/03/29/3708-tc1.png)

```text
输入：points = [[2,2],[-1,-2],[-4,4],[-3,1],[3,-3]], s = "abdca"

输出：2

解释：

边长为 4 的正方形包含两个点 points[0] 和 points[1] 。
```

示例 2：

![3143-4](https://assets.leetcode.com/uploads/2024/03/29/3708-tc2.png)

```text
输入：points = [[1,1],[-2,-2],[-2,2]], s = "abb"

输出：1

解释：

边长为 2 的正方形包含 1 个点 points[0] 。
```

示例 3：

```text
输入：points = [[1,1],[-1,-1],[2,-2]], s = "ccd"

输出：0

解释：

任何正方形都无法只包含 points[0] 和 points[1] 中的一个点，所以合法正方形中都不包含任何点。
```

__提示：__

- `1 <= s.length, points.length <= 10 ^ 5`
- `points[i].length == 2`
- `-10 ^ 9 <= points[i][0], points[i][1] <= 10 ^ 9`
- `s.length == points.length`
- `points` 中的点坐标互不相同。
- `s` 只包含小写英文字母。

__思路:__

```text
1. 排序
按照点的坐标绝对值的最大值进行排序
如果之前已经出现过同样的标签
那么往回找到第一个和当前的点的绝对值的最大值不同的点
返回其边长即可
时间复杂度为 O(NlogN), 空间复杂度为 O(logN), 主要瓶颈在排序
2. 二分查找
左边界为 0
右边界为所有点的坐标绝对值的最大值
二分找到第一个没有重复的标签的位置
如果找到说明左边界可以增加
时间复杂度为 O(NlogM), 空间复杂度为 O(1), 其中 M 为所有点的坐标绝对值的最大值
3. 维护次小值
由于每种标签都能出现 1 次
所以需要找到所有标签的次小值的最小值
一共只有 26 种标签
记录 26 种标签的最小值
如果最小值需要更新
那么就更新次小值为之前的最小值
否则尝试更新次小值
最后找到所有点的坐标绝对值的最大值比这个次小值还要小的点
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxPointsInsideSquare(vector<vector<int>>& points, string s) 
    {
        int mn = INT_MAX, n = points.size(), d = 0, c = 0, result = 0;
        vector<int> distance(26, INT_MAX);
        for (int i = 0; i < n; i++) 
        {
            if ((d = max(abs(points[i].front()), abs(points[i].back()))) < distance[c = s[i] - 'a']) 
            {
                mn = min(mn, distance[c]);
                distance[c] = d;
            } 
            else mn = min(mn, d);
        }
        for (const auto& dis : distance) result += dis < mn;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxPointsInsideSquare(int[][] points, String s) {
        int distance[] = new int[26], mn = Integer.MAX_VALUE, n = points.length, d = 0, c = 0, result = 0;
        Arrays.fill(distance, Integer.MAX_VALUE);
        for (int i = 0; i < n; i++) {
            if ((d = Math.max(Math.abs(points[i][0]), Math.abs(points[i][1]))) < distance[c = s.charAt(i) - 'a']) {
                mn = Math.min(mn, distance[c]);
                distance[c] = d;
            } else mn = Math.min(mn, d);
        }
        for (int dis : distance) if (dis < mn) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxPointsInsideSquare(self, points: List[List[int]], s: str) -> int:
        n, points, visited = len(points), sorted([[x, y, ord(c) - ord('a')] for (x, y), c in zip(points, s)], key=lambda p: max(abs(p[0]), abs(p[1]))), [False] * 26
        for i, [x, y, c] in enumerate(points):
            if visited[c]:
                edge = max(abs(x), abs(y))
                for j in range(i - 1, -1, -1):
                    if max(abs(points[j][0]), abs(points[j][1])) != edge:
                        return j + 1
                return 0
            visited[c] = True
        return n
```
