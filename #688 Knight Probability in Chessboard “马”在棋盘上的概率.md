# 688 Knight Probability in Chessboard “马”在棋盘上的概率

__Description__:
On an n x n chessboard, a knight starts at the cell (row, column) and attempts to make exactly k moves. The rows and columns are 0-indexed, so the top-left cell is (0, 0), and the bottom-right cell is (n - 1, n - 1).

A chess knight has eight possible moves it can make, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![knight](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly k moves or has moved off the chessboard.

Return the probability that the knight remains on the board after it has stopped moving.

__Example:__

Example 1:

Input: n = 3, k = 2, row = 0, column = 0
Output: 0.06250
Explanation: There are two moves (to (1,2), (2,1)) that will keep the knight on the board.
From each of those positions, there are also two moves that will keep the knight on the board.
The total probability the knight stays on the board is 0.0625.

Example 2:

Input: n = 1, k = 0, row = 0, column = 0
Output: 1.00000

__Constraints:__
1 <= n <= 25
0 <= k <= 100
0 <= row, column <= n

__题目描述__:
已知一个 NxN 的国际象棋棋盘，棋盘的行号和列号都是从 0 开始。即最左上角的格子记为 (0, 0)，最右下角的记为 (N-1, N-1)。

现有一个 “马”（也译作 “骑士”）位于 (r, c) ，并打算进行 K 次移动。

如下图所示，国际象棋的 “马” 每一步先沿水平或垂直方向移动 2 个格子，然后向与之相垂直的方向再移动 1 个格子，共有 8 个可选的位置。

![马](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/knight.png)

现在 “马” 每一步都从可选的位置（包括棋盘外部的）中独立随机地选择一个进行移动，直到移动了 K 次或跳到了棋盘外面。

求移动结束后，“马” 仍留在棋盘上的概率。

__示例 :__

输入: 3, 2, 0, 0
输出: 0.0625
解释:
输入的数据依次为 N, K, r, c
第 1 步时，有且只有 2 种走法令 “马” 可以留在棋盘上（跳到（1,2）或（2,1））。对于以上的两种情况，各自在第2步均有且只有2种走法令 “马” 仍然留在棋盘上。
所以 “马” 在结束后仍在棋盘上的概率为 0.0625。

__注意:__

N 的取值范围为 [1, 25]
K 的取值范围为 [0, 100]
开始时，“马” 总是位于棋盘上

__思路__:

动态规划
dp[r][c][k] 记录落在 (r, c) 时, 还剩 k 步的概率和
dp[r][c][k] = sum(dp[r + dr][c + dc][k - 1] / 8.0, 对于落在边界外的全部记为 0, dr 和 dc 为马能跳的 8 个相对位置
考虑到只需要上一次的计算结果, 可以将空间复杂度优化到 O(n ^ 2)
时间复杂度为 O(n ^ 2 * k), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double knightProbability(int n, int k, int row, int column) 
    {
        vector<vector<double>> dp(n, vector<double>(n));
        vector<int> dr{ 2, 2, 1, 1, -1, -1, -2, -2 }, dc{ 1, -1, 2, -2, 2, -2, 1, -1 };
        dp[row][column] = 1;
        while (k--)
        {
            vector<vector<double>> cur(n, vector<double>(n));
            for (int r = 0; r < n; r++)
            {
                for (int c = 0; c < n; c++)
                {
                    for (int i = 0; i < 8; i++)
                    {
                        int x = r + dr[i], y = c + dc[i];
                        if (-1 < x and x < n and -1 < y and y < n) cur[x][y] += dp[r][c] / 8.0;
                    }
                }
            }
            dp = cur;
        }
        return accumulate(dp.cbegin(), dp.cend(), 0.0, [](auto grid, const auto& row) { return accumulate(row.cbegin(), row.cend(), grid); });
    }
};
```

__Java__:

```Java
class Solution {
    public double knightProbability(int n, int k, int row, int column) {
        double[][] dp = new double[n][n];
        int dr[] = new int[]{2, 2, 1, 1, -1, -1, -2, -2}, dc[] = new int[]{1, -1, 2, -2, 2, -2, 1, -1};
        dp[row][column] = 1;
        while (k-- > 0) {
            double[][] cur = new double[n][n];
            for (int r = 0; r < n; r++) {
                for (int c = 0; c < n; c++) {
                    for (int i = 0; i < 8; i++) {
                        int x = r + dr[i], y = c + dc[i];
                        if (-1 < x && x < n && -1 < y && y < n) cur[x][y] += dp[r][c] / 8.0;
                    }
                }
            }
            dp = cur;
        }
        double result = 0.0;
        for (double[] r: dp) for (double x: r) result += x;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def knightProbability(self, n: int, k: int, row: int, column: int) -> float:
        memo, moves = {}, ((-1, -2), (-2, -1), (-2, 1), (-1, 2), (1, -2), (2, -1), (2, 1), (1, 2))
        def dfs(k: int, r: int, c: int) -> float:
            if r < 0 or c < 0 or r >= n or c >= n:
                return 0
            if not k:
                return 1
            if (k, r, c) in memo:
                return memo[(k, r, c)]
            result = 0
            for move in moves:
                result += dfs(k - 1, r + move[0], c + move[1])
            result /= 8.0
            memo[(k, r, c)] = result
            return result
        return dfs(k, row, column)
```
