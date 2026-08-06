# 3128 Right Triangles 直角三角形

__Description:__

You are given a 2D boolean matrix `grid`.

A collection of 3 elements of `grid` is a __right triangle__ if one of its elements is in the __same row__ with another element and in the __same column__ with the third element. The 3 elements may __not__ be next to each other.

Return an integer that is the number of __right triangles__ that can be made with 3 elements of `grid` such that __all__ of them have a value of 1.

__Example:__

Example 1:

```text
Input: grid = [[0,1,0],[0,1,1],[0,1,0]]

Output: 2

Explanation:

There are two right triangles with elements of the value 1. Notice that the blue ones do not form a right triangle because the 3 elements are in the same column.
```

Example 2:

```text
Input: grid = [[1,0,0,0],[0,1,0,1],[1,0,0,0]]

Output: 0

Explanation:

There are no right triangles with elements of the value 1.  Notice that the blue ones do not form a right triangle.
```

Example 3:

```text
Input: grid = [[1,0,1],[1,0,0],[1,0,0]]

Output: 2

Explanation:

There are two right triangles with elements of the value 1.
```

__Constraints:__

- `1 <= grid.length <= 1000`
- `1 <= grid[i].length <= 1000`
- `0 <= grid[i][j] <= 1`

__题目描述:__

给你一个二维 boolean 矩阵 `grid` 。

如果 `grid` 的 3 个元素的集合中，一个元素与另一个元素在 __同一行__，并且与第三个元素在 __同一列__，则该集合是一个 __直角三角形__。3 个元素 __不必__ 彼此相邻。

请你返回使用 `grid` 中的 3 个元素可以构建的 __直角三角形__ 数目，且满足 3 个元素值 __都__ 为 1 。

__示例:__

示例 1：

```text
输入：grid = [[0,1,0],[0,1,1],[0,1,0]]

输出：2

解释：

有 2 个值为 1 的直角三角形。注意蓝色的那个 没有 组成直角三角形，因为 3 个元素在同一列。
```

示例 2：

```text
输入：grid = [[1,0,0,0],[0,1,0,1],[1,0,0,0]]

输出：0

解释：

没有值为 1 的直角三角形。注意蓝色的那个 没有 组成直角三角形。
```

示例 3：

```text
输入：grid = [[1,0,1],[1,0,0],[1,0,0]]

输出：2

解释：

有两个值为 1 的直角三角形。
```

__提示：__

- `1 <= grid.length <= 1000`
- `1 <= grid[i].length <= 1000`
- `0 <= grid[i][j] <= 1`

__思路:__

```text
模拟
枚举直角三角形的角
那么它可以组成该角行上的 (row - 1) * (col - 1) 个直角三角形
其中 row 和 col 分别为同一行和同一列的数字和
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long numberOfRightTriangles(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<int> cols(n, -1);
        for (const auto& row : grid) for (int j = 0; j < n; j++) cols[j] += row[j];
        long long result = 0LL;
        for (const auto& row : grid) for (int j = 0, x = accumulate(row.begin(), row.end(), -1); j < n; j++) if (row[j]) result += x * cols[j];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long numberOfRightTriangles(int[][] grid) {
        int m = grid.length, n = grid[0].length, cols[] = new int[n];
        Arrays.fill(cols, -1);
        for (var row : grid) for (int j = 0; j < n; j++) cols[j] += row[j];
        long result = 0L;
        for (var row : grid) for (int j = 0, x = Arrays.stream(row).sum() - 1; j < n; j++) if (row[j] == 1) result += x * cols[j];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfRightTriangles(self, grid: List[List[int]]) -> int:
        return sum((sum(row) - 1) * sum(y for x, y in zip(row, cols) if x) for row in grid) if (cols := [sum(col) - 1 for col in zip(*grid)]) else 0
```
