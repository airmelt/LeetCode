# 3071 Minimum Operations to Write the Letter Y on a Grid 在矩阵上写出字母 Y 所需的最少操作次数

__Description:__

You are given a __0-indexed__ `n x n` grid where `n` is odd, and `grid[r][c]` is `0`, `1`, or `2`.

We say that a cell belongs to the Letter Y if it belongs to one of the following:

- The diagonal starting at the top-left cell and ending at the center cell of the grid.
- The diagonal starting at the top-right cell and ending at the center cell of the grid.
- The vertical line starting at the center cell and ending at the bottom border of the grid.

The Letter Y is written on the grid if and only if:

- All values at cells belonging to the Y are equal.
- All values at cells not belonging to the Y are equal.
- The values at cells belonging to the Y are different from the values at cells not belonging to the Y.

Return _the __minimum__ number of operations needed to write the letter Y on the grid given that in one operation you can change the value at any cell to_ `0`_,_ `1`_,_ _or_ `2`_._

__Example:__

Example 1:

![3071-1](https://assets.leetcode.com/uploads/2024/01/22/y2.png)

```text
Input: grid = [[1,2,2],[1,1,0],[0,1,0]]
Output: 3
Explanation: We can write Y on the grid by applying the changes highlighted in blue in the image above. After the operations, all cells that belong to Y, denoted in bold, have the same value of 1 while those that do not belong to Y are equal to 0.
It can be shown that 3 is the minimum number of operations needed to write Y on the grid.
```

Example 2:

![3071-2](https://assets.leetcode.com/uploads/2024/01/22/y3.png)

```text
Input: grid = [[0,1,0,1,0],[2,1,0,1,2],[2,2,2,0,1],[2,2,2,2,2],[2,1,2,2,2]]
Output: 12
Explanation: We can write Y on the grid by applying the changes highlighted in blue in the image above. After the operations, all cells that belong to Y, denoted in bold, have the same value of 0 while those that do not belong to Y are equal to 2. 
It can be shown that 12 is the minimum number of operations needed to write Y on the grid.
```

__Constraints:__

- `3 <= n <= 49`
- `n == grid.length == grid[i].length`
- `0 <= grid[i][j] <= 2`
- `n` is odd.

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `n x n` 的矩阵 `grid` ，其中 `n` 为奇数，且 `grid[r][c]` 的值为 `0` 、 `1` 或 `2` 。

如果一个单元格属于以下三条线中的任一一条，我们就认为它是字母 Y 的一部分：

- 从左上角单元格开始到矩阵中心单元格结束的对角线。
- 从右上角单元格开始到矩阵中心单元格结束的对角线。
- 从中心单元格开始到矩阵底部边界结束的垂直线。

当且仅当满足以下全部条件时，可以判定矩阵上写有字母 Y ：

- 属于 Y 的所有单元格的值相等。
- 不属于 Y 的所有单元格的值相等。
- 属于 Y 的单元格的值与不属于Y的单元格的值不同。

每次操作你可以将任意单元格的值改变为 `0` 、 `1` 或 `2` 。返回在矩阵上写出字母 Y 所需的 __最少__ 操作次数。

__示例:__

示例 1：

![3071-3](https://assets.leetcode.com/uploads/2024/01/22/y2.png)

```text
输入：grid = [[1,2,2],[1,1,0],[0,1,0]]
输出：3
解释：将在矩阵上写出字母 Y 需要执行的操作用蓝色高亮显示。操作后，所有属于 Y 的单元格（加粗显示）的值都为 1 ，而不属于 Y 的单元格的值都为 0 。
可以证明，写出 Y 至少需要进行 3 次操作。
```

示例 2：

![3071-4](https://assets.leetcode.com/uploads/2024/01/22/y3.png)

```text
输入：grid = [[0,1,0,1,0],[2,1,0,1,2],[2,2,2,0,1],[2,2,2,2,2],[2,1,2,2,2]]
输出：12
解释：将在矩阵上写出字母 Y 需要执行的操作用蓝色高亮显示。操作后，所有属于 Y 的单元格（加粗显示）的值都为 0 ，而不属于 Y 的单元格的值都为 2 。
可以证明，写出 Y 至少需要进行 12 次操作。
```

__提示：__

- `3 <= n <= 49`
- `n == grid.length == grid[i].length`
- `0 <= grid[i][j] <= 2`
- `n` 为奇数。

__思路:__

```text
1. 暴力法
由于组合仅有 6 种
可以遍历所有组合
分别对于在 Y 上和不在 Y 上的统计出现个数
最后取最小值即可
时间复杂度为 O(K ^ 2 * N ^ 2), 空间复杂度为 O(1), K 为数组出现元素的范围, 这里为 3
2. 一次遍历
一次遍历统计 0-2 每个元素出现的个数
分别按照在 Y 上存到数组 c1
和不在 Y 上存到数组 c2
统计 c1[i] + c2[j] 的最大值 c, 其中 i 和 j 都属于范围 [0, 2], 且 i != j
最后用 n * n - c
时间复杂度为 O(N ^ 2 + K ^ 2), 空间复杂度为 O(K), K 为数组出现元素的范围, 这里为 3
如果 K 非常大, 可以记录出现元素的最大值和次大值, 修改元素的最大值一定会出现在这 4 个值的组合中
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperationsToWriteY(vector<vector<int>>& grid) 
    {
        int c1[3]{}, c2[3]{}, n = grid.size(), half = n >> 1, c = 0;
        for (int i = 0; i < half; i++) 
        {
            ++c1[grid[i][i]];
            ++c1[grid[i][n - i - 1]];
            for (int j = 0; j < n; j++) if (j != i and j != n - i - 1) ++c2[grid[i][j]];
        }
        for (int i = half; i < n; i++) 
        {
            ++c1[grid[i][half]];
            for (int j = 0; j < n; j++) if (j != half) ++c2[grid[i][j]];
        }
        for (int i = 0; i < 3; i++) for (int j = 0; j < 3; j++) if (i != j) c = max(c, c1[i] + c2[j]);
        return n * n - c;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperationsToWriteY(int[][] grid) {
        int c1[] = new int[3], c2[] = new int[3], n = grid.length, half = n >> 1, c = 0;
        for (int i = 0; i < half; i++) {
            ++c1[grid[i][i]];
            ++c1[grid[i][n - i - 1]];
            for (int j = 0; j < n; j++) if (j != i && j != n - i - 1) ++c2[grid[i][j]];
        }
        for (int i = half; i < n; i++) {
            ++c1[grid[i][half]];
            for (int j = 0; j < n; j++) if (j != half) ++c2[grid[i][j]];
        }
        for (int i = 0; i < 3; i++) for (int j = 0; j < 3; j++) if (i != j) c = Math.max(c, c1[i] + c2[j]);
        return n * n - c;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperationsToWriteY(self, grid: List[List[int]]) -> int:
        n = len(grid)
        half = n >> 1
        start = (half, half)

        def helper(y: int, x: int) -> int:
            i, j = start
            result = grid[i][j] != y
            for k in range(1, half + 1):
                result += grid[i - k][j - k] != y
                result += grid[i - k][j + k] != y
                result += grid[i + k][j] != y
            for i in range(n):
                for j in range(n):
                    if i == j and i <= half and j <= half:
                        continue
                    if i + j == n - 1 and i <= half and j >= half:
                        continue
                    if i >= half and j == half:
                        continue
                    result += grid[i][j] != x
            return result
        return min(helper(i, j) for i in range(3) for j in range(3) if i != j)
```
