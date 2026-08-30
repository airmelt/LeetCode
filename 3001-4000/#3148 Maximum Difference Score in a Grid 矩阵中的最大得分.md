# 3148 Maximum Difference Score in a Grid 矩阵中的最大得分

__Description:__

You are given an `m x n` matrix `grid` consisting of __positive__ integers. You can move from a cell in the matrix to __any__ other cell that is either to the bottom or to the right (not necessarily adjacent). The score of a move from a cell with the value `c1` to a cell with the value `c2` is `c2 - c1`.

You can start at __any__ cell, and you have to make __at least__ one move.

Return the __maximum__ total score you can achieve.

__Example:__

Example 1:

![3148-1](https://assets.leetcode.com/uploads/2024/03/14/grid1.png)

```text
Input: grid = [[9,5,7,3],[8,9,6,1],[6,7,14,3],[2,5,3,1]]

Output: 9

Explanation: We start at the cell (0, 1), and we perform the following moves:
```

- Move from the cell (0, 1) to (2, 1) with a score of 7 - 5 = 2.
- Move from the cell (2, 1) to (2, 2) with a score of 14 - 7 = 7.

The total score is 2 + 7 = 9.

Example 2:

![3148-2](https://assets.leetcode.com/uploads/2024/04/08/moregridsdrawio-1.png)

```text
Input: grid = [[4,3,2],[3,2,1]]

Output: -1

Explanation: We start at the cell (0, 0), and we perform one move: (0, 0) to (0, 1). The score is 3 - 4 = -1.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 5`

__题目描述:__

给你一个由 __正整数__ 组成、大小为 `m x n` 的矩阵 `grid`。你可以从矩阵中的任一单元格移动到另一个位于正下方或正右侧的任意单元格（不必相邻）。从值为 `c1` 的单元格移动到值为 `c2` 的单元格的得分为 `c2 - c1` 。

你可以从 __任一__ 单元格开始，并且必须至少移动一次。

返回你能得到的 __最大__ 总得分。

__示例:__

示例 1：

![3148-3](https://assets.leetcode.com/uploads/2024/03/14/grid1.png)

```text
输入：grid = [[9,5,7,3],[8,9,6,1],[6,7,14,3],[2,5,3,1]]

输出：9

解释：从单元格 (0, 1) 开始，并执行以下移动：
```

- 从单元格 (0, 1) 移动到 (2, 1)，得分为 7 - 5 = 2 。
- 从单元格 (2, 1) 移动到 (2, 2)，得分为 14 - 7 = 7 。

总得分为 2 + 7 = 9 。

示例 2：

![3148-4](https://assets.leetcode.com/uploads/2024/04/08/moregridsdrawio-1.png)

```text
输入：grid = [[4,3,2],[3,2,1]]

输出：-1

解释：从单元格 (0, 0) 开始，执行一次移动：从 (0, 0) 到 (0, 1) 。得分为 3 - 4 = -1 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 5`

__思路:__

```text
前缀和
类似前缀和的思想
记录下 grid[i][j] 之前的行列的最小值
由于至少需要移动一次, 所以初始化结果为 -inf
然后取 max(grid[i][j] - min(pre, col[j]))
其中 col[j] 记录列的最小值, pre 记录行的最小值
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxScore(vector<vector<int>>& grid) 
    {
        int result = INT_MIN, m = grid.size(), n = grid.front().size();
        vector<int> col(n, INT_MAX);
        for (int i = 0, pre = INT_MAX; i < m; i++, pre = INT_MAX) 
        {
            for (int j = 0; j < n; j++) 
            {
                result = max(result, grid[i][j] - min(pre, col[j]));
                col[j] = min(col[j], grid[i][j]);
                pre = min(col[j], pre);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxScore(List<List<Integer>> grid) {
        int result = Integer.MIN_VALUE, m = grid.size(), n = grid.get(0).size(), col[] = new int[n];
        Arrays.fill(col, Integer.MAX_VALUE);
        for (int i = 0, pre = Integer.MAX_VALUE; i < m; i++, pre = Integer.MAX_VALUE) {
            for (int j = 0; j < n; j++) {
                result = Math.max(result, grid.get(i).get(j) - Math.min(pre, col[j]));
                col[j] = Math.min(col[j], grid.get(i).get(j));
                pre = Math.min(col[j], pre);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxScore(self, grid: List[List[int]]) -> int:
        result, col = -inf, [inf] * (n := len(grid[0]))
        for i, row in enumerate(grid):
            pre = inf
            for j, num in enumerate(row):
                result, col[j], pre = max(result, num - min(pre, col[j])), min(num, col[j]), min(pre, min(col[j], num))
        return result
```
