# 2397 Maximum Rows Covered by Columns 被列覆盖的最多行数

__Description:__

You are given an `m x n` binary matrix `matrix` and an integer `numSelect`.

Your goal is to select exactly `numSelect` __distinct__ columns from `matrix` such that you cover as many rows as possible.

A row is considered __covered__ if all the `1`'s in that row are also part of a column that you have selected. If a row does not have any `1`s, it is also considered covered.

More formally, let us consider `selected = {c1, c2, ...., cNumSelect}` as the set of columns selected by you. A row `i` is __covered__ by `selected` if:

- For each cell where `matrix[i][j] == 1`, the column `j` is in `selected`.
- Or, no cell in row `i` has a value of `1`.

Return the __maximum__ number of rows that can be __covered__ by a set of `numSelect` columns.

__Example:__

Example 1:

![2397-1](https://assets.leetcode.com/uploads/2022/07/14/rowscovered.png)

```text
Input: matrix = [[0,0,0],[1,0,1],[0,1,1],[0,0,1]], numSelect = 2
```

Output: 3

Explanation:

One possible way to cover 3 rows is shown in the diagram above.
We choose s = {0, 2}.

- Row 0 is covered because it has no occurrences of 1.
- Row 1 is covered because the columns with value 1, i.e. 0 and 2 are present in s.
- Row 2 is not covered because matrix\[2][1] == 1 but 1 is not present in s.
- Row 3 is covered because matrix\[2][2] == 1 and 2 is present in s.
Thus, we can cover three rows.
Note that s = {1, 2} will also cover 3 rows, but it can be shown that no more than three rows can be covered.

Example 2:

![2397-2](https://assets.leetcode.com/uploads/2022/07/14/rowscovered2.png)

```text
Input: matrix = [[1],[0]], numSelect = 1
```

Output: 2

Explanation:

Selecting the only column will result in both rows being covered since the entire matrix is selected.

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 12`
- `matrix[i][j]` is either `0` or `1`.
- `1 <= numSelect <= n`

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `m x n` 的二进制矩阵 `matrix` ；另给你一个整数 `numSelect`，表示你必须从 `matrix` 中选择的 __不同__ 列的数量。

如果一行中所有的 `1` 都被你选中的列所覆盖，则认为这一行被 __覆盖__ 了。

__形式上__，假设 `s = {c1, c2, ...., cNumSelect}` 是你选择的列的集合。对于矩阵中的某一行 `row` ，如果满足下述条件，则认为这一行被集合 `s` __覆盖__:

- 对于满足 `matrix[row][col] == 1` 的每个单元格 `matrix[row][col]`（ `0 <= col <= n - 1`）， `col` 均存在于 `s` 中，或者
- `row` 中 __不存在__ 值为 `1` 的单元格。

你需要从矩阵中选出 `numSelect` 个列，使集合覆盖的行数最大化。

返回一个整数，表示可以由 `numSelect` 列构成的集合 __覆盖__ 的 __最大行数__ 。

__示例:__

示例 1：

![2397-3](https://assets.leetcode.com/uploads/2022/07/14/rowscovered.png)

```text
输入：matrix = [[0,0,0],[1,0,1],[0,1,1],[0,0,1]], numSelect = 2
输出：3
解释：
图示中显示了一种覆盖 3 行的可行办法。
选择 s = {0, 2} 。
```

- 第 0 行被覆盖，因为其中没有出现 1 。
- 第 1 行被覆盖，因为值为 1 的两列（即 0 和 2）均存在于 s 中。
- 第 2 行未被覆盖，因为 matrix\[2][1] == 1 但是 1 未存在于 s 中。
- 第 3 行被覆盖，因为 matrix\[2][2] == 1 且 2 存在于 s 中。

因此，可以覆盖 3 行。
另外 s = {1, 2} 也可以覆盖 3 行，但可以证明无法覆盖更多行。

示例 2：

![2397-4](https://assets.leetcode.com/uploads/2022/07/14/rowscovered2.png)

```text
输入：matrix = [[1],[0]], numSelect = 1
输出：2
解释：
选择唯一的一列，两行都被覆盖了，因为整个矩阵都被覆盖了。
所以我们返回 2 。
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 12`
- `matrix[i][j]` 要么是 `0` 要么是 `1`
- `1 <= numSelect <= n`

__思路:__

```text
状态压缩 ➕ 动态规划
统计 0, 1, 2, ..., 1 << n 中 1 的个数为 numSelect
遍历其所有的子集 sub_set，统计每个子集覆盖的行数
如果 matrix 的 1 个数列是否是 sub_set 的子集，则更新覆盖的行数
时间复杂度为 O(M * 2 ^ N), 空间复杂度为 O(M)
使用位运算优化遍历子集
先求 lowbit = sub_set & -sub_set
前半部分 x = sub_set + lowbit
再求 sub_set = ((sub_set ^ x) / lowbit >> 2) | x
时间复杂度不变但是可以优化掉很多不必要的计算
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumRows(vector<vector<int>>& matrix, int numSelect) 
    {
        int m = matrix.size(), n = matrix.front().size(), result = 0, sub_set = (1 << numSelect) - 1, total = 1 << n;
        vector<int> mask(m);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) mask[i] |= matrix[i][j] << j;
        while (sub_set < total) 
        {
            int cur = 0;
            for (const auto& row : mask) if ((row & sub_set) == row) ++cur;
            result = max(result, cur);
            int lowbit = sub_set & -sub_set, x = sub_set + lowbit;
            sub_set = ((sub_set ^ x) / lowbit >> 2) | x;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumRows(int[][] matrix, int numSelect) {
        int m = matrix.length, n = matrix[0].length, result = 0, mask[] = new int[m], total = 1 << n;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) mask[i] |= matrix[i][j] << j;
        for (int subset = 0, cur = 0; subset < total; subset++, cur = 0) {
            if (Integer.bitCount(subset) == numSelect) {
                for (int row : mask) if ((row & subset) == row) ++cur;
                result = Math.max(result, cur);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumRows(self, matrix: List[List[int]], numSelect: int) -> int:
        return max(sum(row & subset == row for row in mask) for subset in range(1 << len(matrix[0])) if subset.bit_count() == numSelect) if (mask := [sum(x << j for j, x in enumerate(row)) for i, row in enumerate(matrix)]) else 0
```
