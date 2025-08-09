# 2639 Find the Width of Columns of a Grid 查询网格图中每一列的宽度

__Description:__

You are given a __0-indexed__ `m x n` integer matrix `grid`. The width of a column is the maximum __length__ of its integers.

- For example, if `grid = [[-10], [3], [12]]`, the width of the only column is `3` since `-10` is of length `3`.

Return _an integer array_ `ans` _of size_ `n` _where_ `ans[i]` _is the width of the_ `i ^ th` _column_.

The __length__ of an integer `x` with `len` digits is equal to `len` if `x` is non-negative, and `len + 1` otherwise.

__Example:__

Example 1:

```text
Input: grid = [[1],[22],[333]]
Output: [3]
Explanation: In the 0th column, 333 is of length 3.
```

Example 2:

```text
Input: grid = [[-15,1,3],[15,7,12],[5,6,-2]]
Output: [3,1,2]
Explanation: 
In the 0th column, only -15 is of length 3.
In the 1st column, all integers are of length 1. 
In the 2nd column, both 12 and -2 are of length 2.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `-10 ^ 9 <= grid[r][c] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的 `m x n` 整数矩阵 `grid` 。矩阵中某一列的宽度是这一列数字的最大 __字符串长度__ 。

- 比方说，如果 `grid = [[-10], [3], [12]]` ，那么唯一一列的宽度是 `3` ，因为 `-10` 的字符串长度为 `3` 。

请你返回一个大小为 `n` 的整数数组 `ans` ，其中 `ans[i]` 是第 `i` 列的宽度。

一个有 `len` 个数位的整数 `x` ，如果是非负数，那么 __字符串__ __长度__ 为 `len` ，否则为 `len + 1` 。

__示例:__

示例 1：

```text
输入：grid = [[1],[22],[333]]
输出：[3]
解释：第 0 列中，333 字符串长度为 3 。
```

示例 2：

```text
输入：grid = [[-15,1,3],[15,7,12],[5,6,-2]]
输出：[3,1,2]
解释：
第 0 列中，只有 -15 字符串长度为 3 。
第 1 列中，所有整数的字符串长度都是 1 。
第 2 列中，12 和 -2 的字符串长度都为 2 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `-10 ^ 9 <= grid[r][c] <= 10 ^ 9`

__思路:__

```text
模拟
将数字转换为字符串再计算长度
也可以只记录每一列的最大值和最小值
最小值需要小于 0 才计算长度
时间复杂度为 O(MNlogU), 空间复杂度为 O(1), 其中 U 是每一列的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findColumnWidth(vector<vector<int>>& grid) 
    {
        int n = grid.front().size();
        vector<int> result(n);
        for (int j = 0; j < n; j++) for (const auto& row : grid) result[j] = max(result[j], (int)to_string(row[j]).size());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findColumnWidth(int[][] grid) {
        int n = grid[0].length, result[] = new int[n];
        for (int j = 0; j < n; j++) for (int[] row : grid) result[j] = Math.max(result[j], Integer.toString(row[j]).length());
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findColumnWidth(self, grid: List[List[int]]) -> List[int]:
        return [len(str(max(max(col), -10 * min(col)))) for col in zip(*grid)]
```
