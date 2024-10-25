# 2304 Minimum Path Cost in a Grid 网格中的最小路径代价

__Description:__

You are given a __0-indexed__ `m x n` integer matrix `grid` consisting of __distinct__ integers from `0` to `m * n - 1`. You can move in this matrix from a cell to any other cell in the __next__ row. That is, if you are in cell `(x, y)` such that `x < m - 1`, you can move to any of the cells `(x + 1, 0)`, `(x + 1, 1)`, ..., `(x + 1, n - 1)`. __Note__ that it is not possible to move from cells in the last row.

Each possible move has a cost given by a __0-indexed__ 2D array `moveCost` of size `(m * n) x n`, where `moveCost[i][j]` is the cost of moving from a cell with value `i` to a cell in column `j` of the next row. The cost of moving from cells in the last row of `grid` can be ignored.

The cost of a path in `grid` is the __sum__ of all values of cells visited plus the __sum__ of costs of all the moves made. Return _the __minimum__ cost of a path that starts from any cell in the __first__ row and ends at any cell in the __last__ row._

__Example:__

Example 1:

![2304-1](https://assets.leetcode.com/uploads/2022/04/28/griddrawio-2.png)

```text
Input: grid = [[5,3],[4,0],[2,1]], moveCost = [[9,8],[1,5],[10,12],[18,6],[2,4],[14,3]]
Output: 17
Explanation: The path with the minimum possible cost is the path 5 -> 0 -> 1.
```

- The sum of the values of cells visited is 5 + 0 + 1 = 6.
- The cost of moving from 5 to 0 is 3.
- The cost of moving from 0 to 1 is 8.

So the total cost of the path is 6 + 3 + 8 = 17.

Example 2:

```text
Input: grid = [[5,1,2],[4,0,3]], moveCost = [[12,10,15],[20,23,8],[21,7,1],[8,1,13],[9,10,25],[5,3,2]]
Output: 6
Explanation: The path with the minimum possible cost is the path 2 -> 3.
```

- The sum of the values of cells visited is 2 + 3 = 5.
- The cost of moving from 2 to 3 is 1.

So the total cost of this path is 5 + 1 = 6.

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 50`
- `grid` consists of distinct integers from `0` to `m * n - 1`.
- `moveCost.length == m * n`
- `moveCost[i].length == n`
- `1 <= moveCost[i][j] <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数矩阵 `grid` ，矩阵大小为 `m x n` ，由从 `0` 到 `m * n - 1` 的不同整数组成。你可以在此矩阵中，从一个单元格移动到 __下一行__ 的任何其他单元格。如果你位于单元格 `(x, y)` ，且满足 `x < m - 1` ，你可以移动到 `(x + 1, 0)`, `(x + 1, 1)`, ..., `(x + 1, n - 1)` 中的任何一个单元格。__注意:__ 在最后一行中的单元格不能触发移动。

每次可能的移动都需要付出对应的代价，代价用一个下标从 __0__ 开始的二维数组 `moveCost` 表示，该数组大小为 `(m * n) x n` ，其中 `moveCost[i][j]` 是从值为 `i` 的单元格移动到下一行第 `j` 列单元格的代价。从 `grid` 最后一行的单元格移动的代价可以忽略。

`grid` 一条路径的代价是:所有路径经过的单元格的 __值之和__ 加上 所有移动的 __代价之和__ 。从 __第一行__ 任意单元格出发，返回到达 __最后一行__ 任意单元格的最小路径代价_。_

__示例:__

示例 1：

![2304-2](https://assets.leetcode.com/uploads/2022/04/28/griddrawio-2.png)

```text
输入：grid = [[5,3],[4,0],[2,1]], moveCost = [[9,8],[1,5],[10,12],[18,6],[2,4],[14,3]]
输出：17
解释：最小代价的路径是 5 -> 0 -> 1 。
```

- 路径途经单元格值之和 5 + 0 + 1 = 6 。
- 从 5 移动到 0 的代价为 3 。
- 从 0 移动到 1 的代价为 8 。

路径总代价为 6 + 3 + 8 = 17 。

示例 2：

```text
输入：grid = [[5,1,2],[4,0,3]], moveCost = [[12,10,15],[20,23,8],[21,7,1],[8,1,13],[9,10,25],[5,3,2]]
输出：6
解释：
最小代价的路径是 2 -> 3 。
```

- 路径途经单元格值之和 2 + 3 = 5 。
- 从 2 移动到 3 的代价为 1 。

路径总代价为 5 + 1 = 6 。

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 50`
- `grid` 由从 `0` 到 `m * n - 1` 的不同整数组成
- `moveCost.length == m * n`
- `moveCost[i].length == n`
- `1 <= moveCost[i][j] <= 100`

__思路:__

```text
动态规划
设 dp[i][j] 表示走到第 i 行第 j 列的最小代价
dp[i][j] = grid[i][j] + min(dp[i + 1][k] + moveCost[grid[i][j]][k]) for k in range(n)
需要逆序计算, 从倒数第二行开始
考虑到可以直接在 moveCost 和 grid 上修改, 使得空间复杂度为 O(1)
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minPathCost(vector<vector<int>>& grid, vector<vector<int>>& moveCost) 
    {
        int m = grid.size(), n = grid.front().size();
        for (int i = m - 2; i > -1; i--) 
        {
            for (int j = 0; j < n; j++) 
            {
                for (int k = 0; k < n; k++) moveCost[grid[i][j]][k] += grid[i + 1][k];
                grid[i][j] += *min_element(moveCost[grid[i][j]].begin(), moveCost[grid[i][j]].end());
            }
        }
        return *min_element(grid.front().begin(), grid.front().end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minPathCost(int[][] grid, int[][] moveCost) {
        int m = grid.length, n = grid[0].length;
        for (int i = m - 2; i > -1; i--) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) moveCost[grid[i][j]][k] += grid[i + 1][k];
                grid[i][j] += Arrays.stream(moveCost[grid[i][j]]).min().getAsInt();
            }
        }
        return Arrays.stream(grid[0]).min().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def minPathCost(self, grid: List[List[int]], moveCost: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        for i in range(m - 2, -1, -1):
            for j in range(n):
                grid[i][j] += min(g + c for g, c in zip(grid[i + 1], moveCost[grid[i][j]]))
        return min(grid[0])
```
