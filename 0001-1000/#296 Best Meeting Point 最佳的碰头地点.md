# 296 Best Meeting Point 最佳的碰头地点

__Description:__

Given an `m x n` binary grid `grid` where each `1` marks the home of one friend, return the _minimal_ __total travel distance__.

The __total travel distance__ is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using [Manhattan Distance](http://en.wikipedia.org/wiki/Taxicab_geometry), where `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

__Example:__

Example 1:

![296-1](https://assets.leetcode.com/uploads/2021/03/14/meetingpoint-grid.jpg)

```text
Input: grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 6
Explanation: Given three friends living at (0,0), (0,4), and (2,2).
The point (0,2) is an ideal meeting point, as the total travel distance of 2 + 2 + 2 = 6 is minimal.
So return 6.
```

Example 2:

```text
Input: grid = [[1,1]]
Output: 1
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `grid[i][j]` is either `0` or `1`.
- There will be __at least two__ friends in the `grid`.

__题目描述:__

给你一个 `m x n` 的二进制网格 `grid` ，其中 `1` 表示某个朋友的家所处的位置。返回 _最小的_ __总行走距离__ 。

__总行走距离__ 是朋友们家到碰头地点的距离之和。

我们将使用 [曼哈顿距离](https://baike.baidu.com/item/%E6%9B%BC%E5%93%88%E9%A1%BF%E8%B7%9D%E7%A6%BB) 来计算，其中 `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|` 。

__示例:__

示例 1：

![296-2](https://assets.leetcode.com/uploads/2021/03/14/meetingpoint-grid.jpg)

```text
输入: grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
输出: 6 
解释: 给定的三个人分别住在(0,0)，(0,4) 和 (2,2):
     (0,2) 是一个最佳的碰面点，其总行走距离为 2 + 2 + 2 = 6，最小，因此返回 6。
```

示例 2：

```text
输入: grid = [[1,1]]
输出: 1
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `grid[i][j]` 是 `0` 或 `1`.
- `grid` 中 __至少__ 有两个朋友

__思路:__

```text
中位数
分别计算行和列的中位数
累加行和列到中位数的距离, 使用绝对值计算
时间复杂度为 O(MN), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minTotalDistance(vector<vector<int>>& grid) 
    {
        vector<int> rows, cols;
        for (int i = 0, m = grid.size(), n = grid.front().size(); i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j]) rows.emplace_back(i);
        for (int j = 0, m = grid.size(), n = grid.front().size(); j < n; j++) for (int i = 0; i < m; i++) if (grid[i][j]) cols.emplace_back(j);
        int row = rows[rows.size() >> 1], col = cols[cols.size() >> 1], result = 0;
        for (const auto& r : rows) result += abs(r - row);
        for (const auto& c : cols) result += abs(c - col);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minTotalDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        List<Integer> rows = new ArrayList<>(), cols = new ArrayList<>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) rows.add(i);
        for (int j = 0; j < n; j++) for (int i = 0; i < m; i++) if (grid[i][j] == 1) cols.add(j);
        int row = rows.get(rows.size() >> 1), col = cols.get(cols.size() >> 1), result = 0;
        for (int r : rows) result += Math.abs(r - row);
        for (int c : cols) result += Math.abs(c - col);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minTotalDistance(self, grid: List[List[int]]) -> int:
        return sum(abs(rows[len(rows) >> 1] - r) for r in rows) + sum(abs(cols[len(cols) >> 1] - c) for c in cols) if (rows := [i for i in range(len(grid)) for j in range(len(grid[0])) if grid[i][j]]) and (cols := [j for j in range(len(grid[0])) for i in range(len(grid)) if grid[i][j]]) else 0
```
