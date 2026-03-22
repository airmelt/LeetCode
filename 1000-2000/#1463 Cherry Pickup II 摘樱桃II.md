# 1463 Cherry Pickup II 摘樱桃II

__Description:__

You are given a  `rows x cols` matrix  `grid` representing a field of cherries where  `grid[i][j]` represents the number of cherries that you can collect from the  `(i, j)` cell.

You have two robots that can collect cherries for you:

- __Robot #1__ is located at the __top-left corner__  `(0, 0)`, and
- __Robot #2__ is located at the __top-right corner__  `(0, cols - 1)`.

Return the maximum number of cherries collection using both robots by following the rules below:

- From a cell  `(i, j)`, robots can move to cell  `(i + 1, j - 1)`,  `(i + 1, j)`, or  `(i + 1, j + 1)`.
- When any robot passes through a cell, It picks up all cherries, and the cell becomes an empty cell.
- When both robots stay in the same cell, only one takes the cherries.
- Both robots cannot move outside of the grid at any moment.
- Both robots should reach the bottom row in  `grid`.

__Example:__

Example 1:

![1463-1](https://assets.leetcode.com/uploads/2020/04/29/sample_1_1802.png)

```text
Input: grid = [[3,1,1],[2,5,1],[1,5,5],[2,1,1]]
Output: 24
Explanation: Path of robot #1 and #2 are described in color green and blue respectively.
Cherries taken by Robot #1, (3 + 2 + 5 + 2) = 12.
Cherries taken by Robot #2, (1 + 5 + 5 + 1) = 12.
Total of cherries: 12 + 12 = 24.
```

Example 2:

![1463-2](https://assets.leetcode.com/uploads/2020/04/23/sample_2_1802.png)

```text
Input: grid = [[1,0,0,0,0,0,1],[2,0,0,0,0,3,0],[2,0,9,0,0,0,0],[0,3,0,5,4,0,0],[1,0,2,3,0,0,6]]
Output: 28
Explanation: Path of robot #1 and #2 are described in color green and blue respectively.
Cherries taken by Robot #1, (1 + 9 + 5 + 2) = 17.
Cherries taken by Robot #2, (1 + 3 + 4 + 3) = 11.
Total of cherries: 17 + 11 = 28.
```

__Constraints:__

- `rows == grid.length`
- `cols == grid[i].length`
- `2 <= rows, cols <= 70`
- `0 <= grid[i][j] <= 100`

__题目描述:__

给你一个  `rows x cols` 的矩阵  `grid` 来表示一块樱桃地。  `grid` 中每个格子的数字表示你能获得的樱桃数目。

你有两个机器人帮你收集樱桃，机器人 1 从左上角格子  `(0,0)` 出发，机器人 2 从右上角格子  `(0, cols-1)` 出发。

请你按照如下规则，返回两个机器人能收集的最多樱桃数目：

- 从格子  `(i,j)` 出发，机器人可以移动到格子  `(i+1, j-1)`， `(i+1, j)` 或者  `(i+1, j+1)` 。
- 当一个机器人经过某个格子时，它会把该格子内所有的樱桃都摘走，然后这个位置会变成空格子，即没有樱桃的格子。
- 当两个机器人同时到达同一个格子时，它们中只有一个可以摘到樱桃。
- 两个机器人在任意时刻都不能移动到  `grid` 外面。
- 两个机器人最后都要到达  `grid` 最底下一行。

__示例:__

示例 1：

![1463-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/sample_1_1802.png)

```text
输入：grid = [[3,1,1],[2,5,1],[1,5,5],[2,1,1]]
输出：24
解释：机器人 1 和机器人 2 的路径在上图中分别用绿色和蓝色表示。
机器人 1 摘的樱桃数目为 (3 + 2 + 5 + 2) = 12 。
机器人 2 摘的樱桃数目为 (1 + 5 + 5 + 1) = 12 。
樱桃总数为： 12 + 12 = 24 。
```

示例 2：

![1463-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/sample_2_1802.png)

```text
输入：grid = [[1,0,0,0,0,0,1],[2,0,0,0,0,3,0],[2,0,9,0,0,0,0],[0,3,0,5,4,0,0],[1,0,2,3,0,0,6]]
输出：28
解释：机器人 1 和机器人 2 的路径在上图中分别用绿色和蓝色表示。
机器人 1 摘的樱桃数目为 (1 + 9 + 5 + 2) = 17 。
机器人 2 摘的樱桃数目为 (1 + 3 + 4 + 3) = 11 。
樱桃总数为： 17 + 11 = 28 。
```

示例 3：

```text
输入：grid = [[1,0,0,3],[0,0,0,3],[0,0,3,3],[9,0,3,3]]
输出：22
```

示例 4：

```text
输入：grid = [[1,1],[1,1]]
输出：4
```

__提示：__

- `rows == grid.length`
- `cols == grid[i].length`
- `2 <= rows, cols <= 70`
- `0 <= grid[i][j] <= 100`

__思路:__

```text
动态规划
设 dp[i][left][right] 为第 i 行左边的机器人走到 left 位置以及右边的机器人走到 right 位置的最大获取樱桃数
最后返回值为 dp[m - 1][left][right] 的最大值, 即两个机器人都走到最后一行的最大值
初始 dp[0][0][n - 1] = grid[0][0] + grid[0][n - 1], dp[0] 的其他值都为 -1 或者 -0x3f3f3f3f, 表示不可能的情况
由于每次机器人只能向左或者向右移动一格, 所以方向只能从 (-1, 0, 1) 中选择一种, 并且不能超过边界
dp[i][left][right] = max(dp[i - 1][left + l][right + r] + grid[i][left] + grid[i][right]), 其中 l, r 为 -1, 0, 1 中的一个, left 和 right 不能相等, 因为如果 left == right, 总可以使得 left != right, dp[i][left][right] >= dp[i][left][right](grid[i][j][k] >= 0)
由于 dp[i] 只取决于 dp[i - 1] 可以用滚动数组的方式优化空间复杂度
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int cherryPickup(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = -0x3f3f3f3f;
        vector<vector<int>> dp(n, vector<int>(n, result)), cur(n, vector<int>(n, result)), d{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 0}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
        dp.front().back() = grid.front().front() + grid.front().back();
        for (int i = 1, left, right; i < m; i++) 
        {
            for (int l = 0; l < min(n, i + 1); l++) for (int r = n - 1; r > max(-1, n - i - 2); r--) for (const auto& dir: d) if ((left = l + dir.front()) != (right = r + dir.back()) and left > -1 and left < n and right > -1 and right < n) cur[left][right] = max(cur[left][right], dp[l][r] + grid[i][left] + grid[i][right]);
            swap(dp, cur);
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) result = max(result, dp[i][j]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int cherryPickup(int[][] grid) {
        int m = grid.length, n = grid[0].length, dp[][] = new int[n][n], cur[][] = new int[n][n], temp[][] = new int[n][n], d[][] = new int[][]{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 0}, {0, 1}, {1, -1}, {1, 0}, {1, 1}}, result = -0x3f3f3f3f;
        for (int r = 0; r < n; r++) {
            Arrays.fill(dp[r], result);
            Arrays.fill(cur[r], result);
        }
        dp[0][n - 1] = grid[0][0] + grid[0][n - 1];
        for (int i = 1, left, right; i < m; i++) {
            for (int l = 0; l < Math.min(n, i + 1); l++) for (int r = n - 1; r > Math.max(-1, n - i - 2); r--) for (int[] dir: d) if ((left = l + dir[0]) != (right = r + dir[1]) && left > -1 && left < n && right > -1 && right < n) cur[left][right] = Math.max(cur[left][right], dp[l][r] + grid[i][left] + grid[i][right]);
            temp = dp;
            dp = cur;
            cur = temp;
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) result = Math.max(result, dp[i][j]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        m, n, d = len(grid), len(grid[0]), [-1, 0, 1]
        dp = [[[float("-inf")] * n for _ in range(n)] for _ in range(m)]
        dp[0][0][-1] = grid[0][0] + grid[0][-1]
        for i in range(1, m):
            for l in range(n):
                for r in range(n):
                    for d1, d2 in product(d, d):
                        if (left := l + d1) != (right := r + d2) and -1 < left < n and -1 < right < n:
                            dp[i][left][right] = max(dp[i][left][right], dp[i - 1][l][r] + grid[i][left] + grid[i][right])
        return max([max(i) for i in dp[-1]])
```
