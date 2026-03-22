# 2088 Count Fertile Pyramids in a Land 统计农场中肥沃金字塔的数目

__Description:__

A farmer has a __rectangular grid__ of land with `m` rows and `n` columns that can be divided into unit cells. Each cell is either __fertile__ (represented by a `1`) or __barren__ (represented by a `0`). All cells outside the grid are considered barren.

A pyramidal plot of land can be defined as a set of cells with the following criteria:

An inverse pyramidal plot of land can be defined as a set of cells with similar criteria:

Some examples of valid and invalid pyramidal (and inverse pyramidal) plots are shown below. Black cells indicate fertile cells.

![2088-1](https://assets.leetcode.com/uploads/2021/11/08/image.png)

Given a __0-indexed__ `m x n` binary matrix `grid` representing the farmland, return _the __total number__ of pyramidal and inverse pyramidal plots that can be found in_ `grid`.

__Example:__

Example 1:

![2088-2](https://assets.leetcode.com/uploads/2021/12/22/1.JPG)

```text
Input: grid = [[0,1,1,0],[1,1,1,1]]
Output: 2
Explanation: The 2 possible pyramidal plots are shown in blue and red respectively.
There are no inverse pyramidal plots in this grid. 
Hence total number of pyramidal and inverse pyramidal plots is 2 + 0 = 2.
```

Example 2:

![2088-3](https://assets.leetcode.com/uploads/2021/12/22/2.JPG)

```text
Input: grid = [[1,1,1],[1,1,1]]
Output: 2
Explanation: The pyramidal plot is shown in blue, and the inverse pyramidal plot is shown in red. 
Hence the total number of plots is 1 + 1 = 2.
```

Example 3:

![2088-4](https://assets.leetcode.com/uploads/2021/12/22/3.JPG)

```text
Input: grid = [[1,1,1,1,0],[1,1,1,1,1],[1,1,1,1,1],[0,1,0,0,1]]
Output: 13
Explanation: There are 7 pyramidal plots, 3 of which are shown in the 2nd and 3rd figures.
There are 6 inverse pyramidal plots, 2 of which are shown in the last figure.
The total number of plots is 7 + 6 = 13.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `grid[i][j]` is either `0` or `1`.

__题目描述:__

有一个 __矩形网格__ 状的农场，划分为 `m` 行 `n` 列的单元格。每个格子要么是 __肥沃的__ （用 `1` 表示），要么是 __贫瘠__ 的（用 `0` 表示）。网格图以外的所有与格子都视为贫瘠的。

农场中的 金字塔 区域定义如下：

一个 倒金字塔 类似定义如下：

下图展示了部分符合定义和不符合定义的金字塔区域。黑色区域表示肥沃的格子。

![2088-5](https://assets.leetcode.com/uploads/2021/11/08/image.png)

给你一个下标从 __0__ 开始且大小为 `m x n` 的二进制矩阵 `grid` ，它表示农场，请你返回 `grid` 中金字塔和倒金字塔的 __总数目__ 。

__示例:__

示例 1：

![2088-6](https://assets.leetcode.com/uploads/2021/10/23/eg11.png)

![2088-7](https://assets.leetcode.com/uploads/2021/10/23/exa12.png)

![2088-8](https://assets.leetcode.com/uploads/2021/10/23/exa13.png)

```text
输入：grid = [[0,1,1,0],[1,1,1,1]]
输出：2
解释：
2 个可能的金字塔区域分别如上图蓝色和红色区域所示。
这个网格图中没有倒金字塔区域。
所以金字塔区域总数为 2 + 0 = 2 。
```

示例 2：

![2088-9](https://assets.leetcode.com/uploads/2021/10/23/eg21.png)

![2088-10](https://assets.leetcode.com/uploads/2021/10/23/exa22.png)

![2088-11](https://assets.leetcode.com/uploads/2021/10/23/exa23.png)

```text
输入：grid = [[1,1,1],[1,1,1]]
输出：2
解释：
金字塔区域如上图蓝色区域所示，倒金字塔如上图红色区域所示。
所以金字塔区域总数目为 1 + 1 = 2 。
```

示例 3：

![2088-12](https://assets.leetcode.com/uploads/2021/10/23/eg3.png)

```text
输入：grid = [[1,0,1],[0,0,0],[1,0,1]]
输出：0
解释：
网格图中没有任何金字塔或倒金字塔区域。
```

示例 4：

![2088-13](https://assets.leetcode.com/uploads/2021/10/23/eg41.png)

![2088-14](https://assets.leetcode.com/uploads/2021/10/23/eg42.png)

![2088-15](https://assets.leetcode.com/uploads/2021/10/23/eg43.png)

![2088-16](https://assets.leetcode.com/uploads/2021/10/23/eg44.png)

```text
输入：grid = [[1,1,1,1,0],[1,1,1,1,1],[1,1,1,1,1],[0,1,0,0,1]]
输出：13
解释：
有 7 个金字塔区域。上图第二和第三张图中展示了它们中的 3 个。
有 6 个倒金字塔区域。上图中最后一张图展示了它们中的 2 个。
所以金字塔区域总数目为 7 + 6 = 13.
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `grid[i][j]` 要么是 `0` ，要么是 `1` 。

__思路:__

```text
动态规划
用 dp[i][j] 表示以 grid[i][j] 为顶点的金字塔的最大高度
考虑正向金字塔
如果 grid[i][j] 为 1, 则 dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j + 1], dp[i - 1][j]) + 1
也就是说它等于左上，右上，上方的最小值加 1
在计算的过程中，如果 i > 1, 则 result += dp[i][j] - 1, 因为金字塔的高度为 1 时不计入
倒立的可以用类似方法计算
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPyramids(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), dp[m][n], result = 0;
        auto f = [&]()
        {
            for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) dp[i][j] = grid[i][j];
            for (int i = 1; i < m; i++)
            {
                for (int j = 1; j < n - 1; j++)
                {
                    if (grid[i][j])
                    {
                        dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j + 1], dp[i - 1][j]}) + 1;
                        result += dp[i][j] - 1;
                    }
                }
            }
        };
        f();
        for (int i = 0; i < m >> 1; i++) swap(grid[i], grid[m - 1 - i]);
        f();
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[][] dp;
    private int m, n, result = 0;

    public int countPyramids(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        dp = new int[m][n];
        f(grid);
        for (int i = 0; i < m >> 1; i++) {
            int[] temp = grid[i];
            grid[i] = grid[m - i - 1];
            grid[m - i - 1] = temp;
        }
        f(grid);
        return result;
    }

    private void f(int[][] grid) {
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) dp[i][j] = grid[i][j];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n - 1; j++) {
                if (grid[i][j] == 1) {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j - 1], dp[i - 1][j + 1]), dp[i - 1][j]) + 1;
                    result += dp[i][j] - 1;
                }
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def countPyramids(self, grid: List[List[int]]) -> int:
        m, n, result = len(grid), len(grid[0]), 0
        def f():
            dp = copy.deepcopy(grid)
            nonlocal result
            for i in range(1, m):
                for j in range(1, n - 1):
                    if grid[i][j]:
                        dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j + 1], dp[i - 1][j]) + 1
                        result += dp[i][j] - 1
        f()
        for i in range(m >> 1):
            grid[i], grid[m - i - 1] = grid[m - i - 1], grid[i]
        f()
        return result
```
