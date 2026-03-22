# 2245 Maximum Trailing Zeros in a Cornered Path 转角路径的乘积中最多能有几个尾随零

__Description:__

You are given a 2D integer array `grid` of size `m x n`, where each cell contains a positive integer.

A cornered path is defined as a set of adjacent cells with at most one turn. More specifically, the path should exclusively move either horizontally or vertically up to the turn (if there is one), without returning to a previously visited cell. After the turn, the path will then move exclusively in the alternate direction: move vertically if it moved horizontally, and vice versa, also without returning to a previously visited cell.

The product of a path is defined as the product of all the values in the path.

Return _the __maximum__ number of __trailing zeros__ in the product of a cornered path found in_ `grid`.

Note:

- __Horizontal__ movement means moving in either the left or right direction.
- __Vertical__ movement means moving in either the up or down direction.

__Example:__

Example 1:

![2245-1](https://assets.leetcode.com/uploads/2022/03/23/ex1new2.jpg)

```text
Input: grid = [[23,17,15,3,20],[8,1,20,27,11],[9,4,6,2,21],[40,9,1,10,6],[22,7,4,5,3]]
Output: 3
Explanation: The grid on the left shows a valid cornered path.
It has a product of 15 * 20 * 6 * 1 * 10 = 18000 which has 3 trailing zeros.
It can be shown that this is the maximum trailing zeros in the product of a cornered path.

The grid in the middle is not a cornered path as it has more than one turn.
The grid on the right is not a cornered path as it requires a return to a previously visited cell.
```

Example 2:

![2245-2](https://assets.leetcode.com/uploads/2022/03/25/ex2.jpg)

```text
Input: grid = [[4,3,2],[7,6,1],[8,8,8]]
Output: 0
Explanation: The grid is shown in the figure above.
There are no cornered paths in the grid that result in a product with a trailing zero.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 1000`

__题目描述:__

给你一个二维整数数组 `grid` ，大小为 `m x n`，其中每个单元格都含一个正整数。

转角路径 定义为：包含至多一个弯的一组相邻单元。具体而言，路径应该完全 向水平方向 或者 向竖直方向 移动过弯（如果存在弯），而不能访问之前访问过的单元格。在过弯之后，路径应当完全朝 另一个 方向行进：如果之前是向水平方向，那么就应该变为向竖直方向；反之亦然。当然，同样不能访问之前已经访问过的单元格。

一条路径的 乘积 定义为：路径上所有值的乘积。

请你从 `grid` 中找出一条乘积中尾随零数目最多的转角路径，并返回该路径中尾随零的数目。

注意：

- __水平__ 移动是指向左或右移动。
- __竖直__ 移动是指向上或下移动。

__示例:__

示例 1：

![2245-3](https://assets.leetcode.com/uploads/2022/03/23/ex1new2.jpg)

```text
输入：grid = [[23,17,15,3,20],[8,1,20,27,11],[9,4,6,2,21],[40,9,1,10,6],[22,7,4,5,3]]
输出：3
解释：左侧的图展示了一条有效的转角路径。
其乘积为 15 * 20 * 6 * 1 * 10 = 18000 ，共计 3 个尾随零。
可以证明在这条转角路径的乘积中尾随零数目最多。

中间的图不是一条有效的转角路径，因为它有不止一个弯。
右侧的图也不是一条有效的转角路径，因为它需要重复访问已经访问过的单元格。
```

示例 2：

![2245-4](https://assets.leetcode.com/uploads/2022/03/25/ex2.jpg)

```text
输入：grid = [[4,3,2],[7,6,1],[8,8,8]]
输出：0
解释：网格如上图所示。
不存在乘积含尾随零的转角路径。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 1000`

__思路:__

```text
前缀和
尾随 0 的个数只和因子 2 或 5 有关
用数组 c2, c5 预处理因子 2 和 5 的个数
由于数组中的数不大于 1000, 处理到 1000 即可
记录 grid 中的 2 和 5 因子数的前缀和
用 dp[i + 1][j + 1] 表示加到 grid[i][j] 的因子的前缀和
用 dp[i][j][0] 和 dp[i][j][1] 分别记录 2 和 5 的因子数
dp[i][j + 1][0] = dp[i][j][0] + c2[grid[i][j]]
dp[i][j + 1][1] = dp[i][j][1] + c5[grid[i][j]]
也可以分别记录行和列的前缀和
为了得到最大路径, 必须从数组的边界出发并从边界结束
一共有 4 种可能的方向
分别是左上, 左下, 右上, 右下
每次统计时取 2 和 5 的较大值即可
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxTrailingZeros(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<int> c2(1001), c5(1001);
        for (int i = 2; i < 1001; i++)
        {
            if (!(i & 1)) c2[i] = c2[i >> 1] + 1;
            if (!(i % 5)) c5[i] = c5[i / 5] + 1;
        }
        vector<vector<int>> f2(m + 1, vector<int>(n + 1)), g2(m + 1, vector<int>(n + 1)), f5(m + 1, vector<int>(n + 1)), g5(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                f2[i + 1][j + 1] = f2[i + 1][j] + c2[grid[i][j]];
                g2[i + 1][j + 1] = g2[i][j + 1] + c2[grid[i][j]];
                f5[i + 1][j + 1] = f5[i + 1][j] + c5[grid[i][j]];
                g5[i + 1][j + 1] = g5[i][j + 1] + c5[grid[i][j]];
            }
        }
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                result = max(result, min(f2[i + 1][j + 1] + g2[i][j + 1], f5[i + 1][j + 1] + g5[i][j + 1]));
                result = max(result, min(f2[i + 1][j + 1] + g2[m][j + 1] - g2[i + 1][j + 1], f5[i + 1][j + 1] + g5[m][j + 1] - g5[i + 1][j + 1]));
                result = max(result, min(f2[i + 1][n] - f2[i + 1][j + 1] + g2[i + 1][j + 1], f5[i + 1][n] - f5[i + 1][j + 1] + g5[i + 1][j + 1]));
                result = max(result, min(f2[i + 1][n] - f2[i + 1][j + 1] + g2[m][j + 1] - g2[i][j + 1], f5[i + 1][n] - f5[i + 1][j + 1] + g5[m][j + 1] - g5[i][j + 1]));
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    static int[] c2 = new int[1001];
    static int[] c5 = new int[1001];

    static {
        for (int i = 2; i < 1001; i++) {
            if ((i & 1) == 0) c2[i] = c2[i >> 1] + 1;
            if ((i % 5) == 0) c5[i] = c5[i / 5] + 1;
        }
    }

    public int maxTrailingZeros(int[][] grid) {
        int m = grid.length, n = grid[0].length, result = 0, dp[][][] = new int[m + 1][n + 1][2];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                dp[i][j + 1][0] = dp[i][j][0] + c2[grid[i][j]];
                dp[i][j + 1][1] = dp[i][j][1] + c5[grid[i][j]];
            }
        }
        for (int j = 0; j < n; j++) {
            for (int i = 0, s2 = 0, s5 = 0; i < m; i++) {
                s2 += c2[grid[i][j]];
                s5 += c5[grid[i][j]];
                result = Math.max(result, Math.max(Math.min(s2 + dp[i][j][0], s5 + dp[i][j][1]), Math.min(s2 + dp[i][n][0] - dp[i][j + 1][0], s5 + dp[i][n][1] - dp[i][j + 1][1])));
            }
            for (int i = m - 1, s2 = 0, s5 = 0; i > -1; i--) {
                s2 += c2[grid[i][j]];
                s5 += c5[grid[i][j]];
                result = Math.max(result, Math.max(Math.min(s2 + dp[i][j][0], s5 + dp[i][j][1]), Math.min(s2 + dp[i][n][0] - dp[i][j + 1][0], s5 + dp[i][n][1] - dp[i][j + 1][1])));
            }
        }
        return result;
    }
}
```

__Python__:

```Python
c2, c5 = [0] * 1001, [0] * 1001
for i in range(2, 1001):
    if not (i & 1):
        c2[i] = c2[i >> 1] + 1
    if not i % 5:
        c5[i] = c5[i // 5] + 1

class Solution:
    def maxTrailingZeros(self, grid: List[List[int]]) -> int:
        m, n, result = len(grid), len(grid[0]), 0
        dp = [[(0, 0)] * (n + 1) for _ in range(m + 1)]
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                dp[i][j + 1] = (dp[i][j][0] + c2[v], dp[i][j][1] + c5[v])
        for j, col in enumerate(zip(*grid)):
            s2 = s5 = 0
            for i, v in enumerate(col):
                s2 += c2[v]
                s5 += c5[v]
                result = max(result, min(s2 + dp[i][j][0], s5 + dp[i][j][1]), min(s2 + dp[i][n][0] - dp[i][j + 1][0], s5 + dp[i][n][1] - dp[i][j + 1][1]))
            s2 = s5 = 0
            for i in range(m - 1, -1, -1):
                s2 += c2[col[i]]
                s5 += c5[col[i]]
                result = max(result, min(s2 + dp[i][j][0], s5 + dp[i][j][1]), min(s2 + dp[i][n][0] - dp[i][j + 1][0], s5 + dp[i][n][1] - dp[i][j + 1][1]))
        return result
```
