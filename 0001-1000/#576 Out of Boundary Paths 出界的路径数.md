# 576 Out of Boundary Paths 出界的路径数

__Description__:
There is an m by n grid with a ball. Given the start coordinate (i,j) of the ball, you can move the ball to adjacent cell or cross the grid boundary in four directions (up, down, left, right). However, you can at most move N times. Find out the number of paths to move the ball out of grid boundary. The answer may be very large, return it after mod 10^9 + 7.

__Example:__

Example 1:

Input: m = 2, n = 2, N = 2, i = 0, j = 0
Output: 6
Explanation:

![out_of_boundary_paths_1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/out_of_boundary_paths_1.png)

Example 2:

Input: m = 1, n = 3, N = 3, i = 0, j = 1
Output: 12
Explanation:

![out_of_boundary_paths_2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/out_of_boundary_paths_2.png)

__Note:__

Once you move the ball out of boundary, you cannot move it back.
The length and height of the grid is in range [1,50].
N is in range [0,50].

__题目描述__:
给定一个 m × n 的网格和一个球。球的起始坐标为 (i,j) ，你可以将球移到相邻的单元格内，或者往上、下、左、右四个方向上移动使球穿过网格边界。但是，你最多可以移动 N 次。找出可以将球移出边界的路径数量。答案可能非常大，返回 结果 mod 10^9 + 7 的值。

__示例 :__

示例 1：

输入: m = 2, n = 2, N = 2, i = 0, j = 0
输出: 6
解释:

![出界的路径数 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/out_of_boundary_paths_1.png)

示例 2：

输入: m = 1, n = 3, N = 3, i = 0, j = 1
输出: 12
解释:

![出界的路径数 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/out_of_boundary_paths_2.png)

__说明:__

球一旦出界，就不能再被移动回网格内。
网格的长度和高度在 [1,50] 的范围内。
N 在 [0,50] 的范围内。

__思路__:

动态规划
dp[i][j][N] 表示从 grid[i][j] 出发恰好移动 N 步到界外的步数
如果下一步没有移出边界, 则dp[i][j][N] = dp[i - 1][j][N - 1] + dp[i + 1][j][N - 1] + dp[i][j - 1][N - 1] + dp[i][j + 1][N - 1], 其中 0 <= i + 1, i - 1 < m, 0 <= j + 1, j - 1 < n
否则 ++dp[i][j][N]
由于每次只与前一次移动有关, 可以将空间复杂度优化到 mn
注意处理 mod
时间复杂度 O(mnN), 空间复杂度 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findPaths(int m, int n, int N, int i, int j) 
    {
        vector<vector<int>> pre(m, vector<int>(n)), direction = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        int mod = 1000000007;
        while (N-- > 0) 
        {
            vector<vector<int>> cur(m, vector<int>(n));
            for (int x = 0; x < m; x++) 
            {
                for (int y = 0; y < n; y++) 
                {
                    for (int k = 0; k < 4; k++) 
                    {
                        int cur_x = x + direction[k][0], cur_y = y + direction[k][1];
                        if (cur_x == -1 or cur_y == -1 or cur_x == m or cur_y == n) ++cur[x][y];
                        else cur[x][y] = (cur[x][y] + pre[cur_x][cur_y]) % mod;
                    }
                }
            }
            pre = cur;
        }
        return pre[i][j] % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int findPaths(int m, int n, int N, int i, int j) {
        int pre[][] = new int[m][n], direction[][] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}}, mod = 1000000007;
        while (N-- > 0) {
            int cur[][] = new int[m][n];
            for (int x = 0; x < m; x++) {
                for (int y = 0; y < n; y++) {
                    for (int k = 0; k < 4; k++) {
                        int curX = x + direction[k][0], curY = y + direction[k][1];
                        if (curX == -1 || curY == -1 || curX == m || curY == n) ++cur[x][y];
                        else cur[x][y] = (cur[x][y] + pre[curX][curY]) % mod;
                    }
                }
            }
            pre = cur;
        }
        return pre[i][j] % mod;
    }
}
```

__Python__:

```Python
class Solution:
    def findPaths(self, m: int, n: int, N: int, i: int, j: int) -> int:
        pre, direction = [[0] * n for _ in range(m)], [(0, 1), (0, -1), (1, 0), (-1, 0)]
        for _ in range(N):
            cur = [[0] * n for _ in range(m)]
            for x in range(m):
                for y in range(n):
                    for dx, dy in direction:
                        cur_x, cur_y = x + dx, y + dy
                        if cur_x == -1 or cur_y == -1 or cur_x == m or cur_y == n:
                            cur[x][y] += 1
                        else:
                            cur[x][y] += pre[cur_x][cur_y]
            pre = cur
        return pre[i][j] % (10 ** 9 + 7)
```
