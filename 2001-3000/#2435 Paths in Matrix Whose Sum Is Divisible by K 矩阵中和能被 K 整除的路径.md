# 2435 Paths in Matrix Whose Sum Is Divisible by K 矩阵中和能被 K 整除的路径

__Description:__

You are given a __0-indexed__ `m x n` integer matrix `grid` and an integer `k`. You are currently at position `(0, 0)` and you want to reach position `(m - 1, n - 1)` moving only __down__ or __right__.

Return _the number of paths where the sum of the elements on the path is divisible by_ `k`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![2435-1](https://assets.leetcode.com/uploads/2022/08/13/image-20220813183124-1.png)

```text
Input: grid = [[5,2,4],[3,0,5],[0,7,2]], k = 3
Output: 2
Explanation: There are two paths where the sum of the elements on the path is divisible by k.
The first path highlighted in red has a sum of 5 + 2 + 4 + 5 + 2 = 18 which is divisible by 3.
The second path highlighted in blue has a sum of 5 + 3 + 0 + 5 + 2 = 15 which is divisible by 3.
```

Example 2:

![2435-2](https://assets.leetcode.com/uploads/2022/08/17/image-20220817112930-3.png)

```text
Input: grid = [[0,0]], k = 5
Output: 1
Explanation: The path highlighted in red has a sum of 0 + 0 = 0 which is divisible by 5.
```

Example 3:

![2435-3](https://assets.leetcode.com/uploads/2022/08/12/image-20220812224605-3.png)

```text
Input: grid = [[7,3,4,9],[2,3,6,2],[2,3,7,0]], k = 1
Output: 10
Explanation: Every integer is divisible by 1 so the sum of the elements on every possible path is divisible by k.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 5 * 10 ^ 4`
- `1 <= m * n <= 5 * 10 ^ 4`
- `0 <= grid[i][j] <= 100`
- `1 <= k <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的 `m x n` 整数矩阵 `grid` 和一个整数 `k` 。你从起点 `(0, 0)` 出发，每一步只能往 __下__ 或者往 __右__ ，你想要到达终点 `(m - 1, n - 1)` 。

请你返回路径和能被 `k` 整除的路径数目，由于答案可能很大，返回答案对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

![2435-4](https://assets.leetcode.com/uploads/2022/08/13/image-20220813183124-1.png)

```text
输入：grid = [[5,2,4],[3,0,5],[0,7,2]], k = 3
输出：2
解释：有两条路径满足路径上元素的和能被 k 整除。
第一条路径为上图中用红色标注的路径，和为 5 + 2 + 4 + 5 + 2 = 18 ，能被 3 整除。
第二条路径为上图中用蓝色标注的路径，和为 5 + 3 + 0 + 5 + 2 = 15 ，能被 3 整除。
```

示例 2：

![2435-5](https://assets.leetcode.com/uploads/2022/08/17/image-20220817112930-3.png)

```text
输入：grid = [[0,0]], k = 5
输出：1
解释：红色标注的路径和为 0 + 0 = 0 ，能被 5 整除。
```

示例 3：

![2435-6](https://assets.leetcode.com/uploads/2022/08/12/image-20220812224605-3.png)

```text
输入：grid = [[7,3,4,9],[2,3,6,2],[2,3,7,0]], k = 1
输出：10
解释：每个数字都能被 1 整除，所以每一条路径的和都能被 k 整除。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 5 * 10 ^ 4`
- `1 <= m * n <= 5 * 10 ^ 4`
- `0 <= grid[i][j] <= 100`
- `1 <= k <= 50`

__思路:__

```text
动态规划
设 dp[i][j][p] 表示从 (0, 0) 到 (i, j) 的路径和除 k 余数为 p 的路径数目
初始化 dp[0][0][grid[0][0] % k] = 1
由上一状态 grid[i][j] 转移而来的状态为 grid[i - 1][j] 或 grid[i][j - 1]
(grid[i][j] + p) % k = p
得 p = (p - grid[i][j]) % k 
状态转移方程为
dp[i][j][p] = dp[i - 1][j][(p - grid[i][j]) % k] + dp[i][j - 1][(p - grid[i][j]) % k]
为了保证 p - grid[i][j] 为正数，需要加上 k 并取模
即 dp[i][j][p] = dp[i - 1][j][((p - grid[i][j]) % k + k) % k] + dp[i][j - 1][((p - grid[i][j]) % k + k) % k]
注意讨论边界条件，当 i = 0 或 j = 0 时，只能从一条路径转移而来
时间复杂度为 O(MNK), 空间复杂度为 O(MNK), 注意到 dp[i][j][p] 只与 dp[i - 1][j][p] 和 dp[i][j - 1][p] 有关，可以用滚动数组优化空间复杂度为 O(NK)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPaths(vector<vector<int>>& grid, int k) 
    {
        int m = grid.size(), n = grid.front().size(), MOD = 1e9 + 7, dp[m][n][k];
        memset(dp, 0, sizeof(dp));
        dp[0][0][grid[0][0] % k] = 1;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                for (int p = 0; p < k; p++) 
                {
                    if (i != 0) dp[i][j][p] = (dp[i][j][p] + dp[i - 1][j][((p - grid[i][j]) % k + k) % k]) % MOD;
                    if (j != 0) dp[i][j][p] = (dp[i][j][p] + dp[i][j - 1][((p - grid[i][j]) % k + k) % k]) % MOD;
                }
            }
        }
        return dp[m - 1][n - 1][0] % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPaths(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length, MOD = 1_000_000_007, dp[][][] = new int[m][n][k];
        dp[0][0][grid[0][0] % k] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int p = 0; p < k; p++) {
                    if (i != 0) dp[i][j][p] = (dp[i][j][p] + dp[i - 1][j][((p - grid[i][j]) % k + k) % k]) % MOD;
                    if (j != 0) dp[i][j][p] = (dp[i][j][p] + dp[i][j - 1][((p - grid[i][j]) % k + k) % k]) % MOD;
                }
            }
        }
        return dp[m - 1][n - 1][0] % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPaths(self, grid: List[List[int]], k: int) -> int:
        m, n, MOD = len(grid), len(grid[0]), 10 ** 9 + 7
        dp = [[[0] * k for _ in range(n)] for _ in range(m)]
        dp[0][0][grid[0][0] % k] = 1
        for i in range(m):
            for j in range(n):
                for p in range(k):
                    if i:
                        dp[i][j][p] += dp[i - 1][j][(p - grid[i][j]) % k]
                    if j:
                        dp[i][j][p] += dp[i][j - 1][(p - grid[i][j]) % k]
        return dp[-1][-1][0] % MOD
```
