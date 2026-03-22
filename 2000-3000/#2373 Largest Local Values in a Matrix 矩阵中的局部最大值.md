# 2373 Largest Local Values in a Matrix 矩阵中的局部最大值

__Description:__

You are given an `n x n` integer matrix `grid`.

Generate an integer matrix `maxLocal` of size `(n - 2) x (n - 2)` such that:

- `maxLocal[i][j]` is equal to the __largest__ value of the `3 x 3` matrix in `grid` centered around row `i + 1` and column `j + 1`.

In other words, we want to find the largest value in every contiguous `3 x 3` matrix in `grid`.

Return the generated matrix.

__Example:__

Example 1:

![2373-1](https://assets.leetcode.com/uploads/2022/06/21/ex1.png)

```text
Input: grid = [[9,9,8,1],[5,6,2,6],[8,2,6,4],[6,2,2,2]]
Output: [[9,9],[8,6]]
Explanation: The diagram above shows the original matrix and the generated matrix.
Notice that each value in the generated matrix corresponds to the largest value of a contiguous 3 x 3 matrix in grid.
```

Example 2:

![2373-2](https://assets.leetcode.com/uploads/2022/07/02/ex2new2.png)

```text
Input: grid = [[1,1,1,1,1],[1,1,1,1,1],[1,1,2,1,1],[1,1,1,1,1],[1,1,1,1,1]]
Output: [[2,2,2],[2,2,2],[2,2,2]]
Explanation: Notice that the 2 is contained within every contiguous 3 x 3 matrix in grid.
```

__Constraints:__

- `n == grid.length == grid[i].length`
- `3 <= n <= 100`
- `1 <= grid[i][j] <= 100`

__题目描述:__

给你一个大小为 `n x n` 的整数矩阵 `grid` 。

生成一个大小为 `(n - 2) x (n - 2)` 的整数矩阵  `maxLocal` ，并满足:

- `maxLocal[i][j]` 等于 `grid` 中以 `i + 1` 行和 `j + 1` 列为中心的 `3 x 3` 矩阵中的 __最大值__ 。

换句话说，我们希望找出 `grid` 中每个 `3 x 3` 矩阵中的最大值。

返回生成的矩阵。

__示例:__

示例 1：

![2373-3](https://assets.leetcode.com/uploads/2022/06/21/ex1.png)

```text
输入：grid = [[9,9,8,1],[5,6,2,6],[8,2,6,4],[6,2,2,2]]
输出：[[9,9],[8,6]]
解释：原矩阵和生成的矩阵如上图所示。
注意，生成的矩阵中，每个值都对应 grid 中一个相接的 3 x 3 矩阵的最大值。
```

示例 2：

![2373-4](https://assets.leetcode.com/uploads/2022/07/02/ex2new2.png)

```text
输入：grid = [[1,1,1,1,1],[1,1,1,1,1],[1,1,2,1,1],[1,1,1,1,1],[1,1,1,1,1]]
输出：[[2,2,2],[2,2,2],[2,2,2]]
解释：注意，2 包含在 grid 中每个 3 x 3 的矩阵中。
```

__提示：__

- `n == grid.length == grid[i].length`
- `3 <= n <= 100`
- `1 <= grid[i][j] <= 100`

__思路:__

```text
模拟
类似池化操作
遍历每个 3 x 3 的矩阵, 计算其中的最大值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> largestLocal(vector<vector<int>>& grid) 
    {
        int n = grid.size();
        vector<vector<int>> result(n - 2, vector<int>(n - 2));
        for (int i = 0; i < n - 2; i++) for (int j = 0; j < n - 2; j++) for (int x = i; x < i + 3; x++) for (int y = j; y < j + 3; y++) result[i][j] = max(result[i][j], grid[x][y]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] largestLocal(int[][] grid) {
        int n = grid.length, result[][] = new int[n - 2][n - 2];
        for (int i = 0; i < n - 2; i++) for (int j = 0; j < n - 2; j++) for (int x = i; x < i + 3; x++) for (int y = j; y < j + 3; y++) result[i][j] = Math.max(result[i][j], grid[x][y]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestLocal(self, grid: List[List[int]]) -> List[List[int]]:
        return [[max(grid[x][y] for x in range(i, i + 3) for y in range(j, j + 3)) for j in range(len(grid) - 2)] for i in range(len(grid) - 2)]
```
