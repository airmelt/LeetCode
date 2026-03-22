# 741 Cherry Pickup 摘樱桃

__Description__:
You are given an n x n grid representing a field of cherries, each cell is one of three possible integers.

0 means the cell is empty, so you can pass through,
1 means the cell contains a cherry that you can pick up and pass through, or
-1 means the cell contains a thorn that blocks your way.
Return the maximum number of cherries you can collect by following the rules below:

Starting at the position (0, 0) and reaching (n - 1, n - 1) by moving right or down through valid path cells (cells with value 0 or 1).
After reaching (n - 1, n - 1), returning to (0, 0) by moving left or up through valid path cells.
When passing through a path cell containing a cherry, you pick it up, and the cell becomes an empty cell 0.
If there is no valid path between (0, 0) and (n - 1, n - 1), then no cherries can be collected.

__Example:__

Example 1:

![cherry](https://assets.leetcode.com/uploads/2020/12/14/grid.jpg)

Input: grid = [[0,1,-1],[1,0,-1],[1,1,1]]
Output: 5
Explanation: The player started at (0, 0) and went down, down, right right to reach (2, 2).
4 cherries were picked up during this single trip, and the matrix becomes [[0,1,-1],[0,0,-1],[0,0,0]].
Then, the player went left, up, up, left to return home, picking up one more cherry.
The total number of cherries picked up is 5, and this is the maximum possible.

Example 2:

Input: grid = [[1,1,-1],[1,-1,1],[-1,1,1]]
Output: 0

__Constraints:__

n == grid.length
n == grid[i].length
1 <= n <= 50
grid[i][j] is -1, 0, or 1.
grid[0][0] != -1
grid[n - 1][n - 1] != -1

__题目描述__:
一个N x N的网格(grid) 代表了一块樱桃地，每个格子由以下三种数字的一种来表示：

0 表示这个格子是空的，所以你可以穿过它。
1 表示这个格子里装着一个樱桃，你可以摘到樱桃然后穿过它。
-1 表示这个格子里有荆棘，挡着你的路。
你的任务是在遵守下列规则的情况下，尽可能的摘到最多樱桃：

从位置 (0, 0) 出发，最后到达 (N-1, N-1) ，只能向下或向右走，并且只能穿越有效的格子（即只可以穿过值为0或者1的格子）；
当到达 (N-1, N-1) 后，你要继续走，直到返回到 (0, 0) ，只能向上或向左走，并且只能穿越有效的格子；
当你经过一个格子且这个格子包含一个樱桃时，你将摘到樱桃并且这个格子会变成空的（值变为0）；
如果在 (0, 0) 和 (N-1, N-1) 之间不存在一条可经过的路径，则没有任何一个樱桃能被摘到。

__示例 :__

示例 1:

输入: grid =
[[0, 1, -1],
 [1, 0, -1],
 [1, 1,  1]]
输出: 5
解释：
玩家从（0,0）点出发，经过了向下走，向下走，向右走，向右走，到达了点(2, 2)。
在这趟单程中，总共摘到了4颗樱桃，矩阵变成了[[0,1,-1],[0,0,-1],[0,0,0]]。
接着，这名玩家向左走，向上走，向上走，向左走，返回了起始点，又摘到了1颗樱桃。
在旅程中，总共摘到了5颗樱桃，这是可以摘到的最大值了。

__说明:__

grid 是一个 N * N 的二维数组，N的取值范围是1 <= N <= 50。
每一个 grid[i][j] 都是集合 {-1, 0, 1}其中的一个数。
可以保证起点 grid[0][0] 和终点 grid[N-1][N-1] 的值都不会是 -1。

__思路__:

动态规划
相当于两个人从左上角走到右下角不重复计算的和
dp[i][j][k][l] 表示两人分别走到 (i, j), (k, l) 得分总和
dp[i][j][k][l] = max{ dp[i - 1][j][k - 1][l], dp[i - 1][j][k][l - 1],dp[i][j - 1][k - 1][l], dp[i][j - 1][k][l - 1] } + grid[i][j] + grid[k][l]
注意到 i + j = k + l, 所以可以降低 1 维
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int cherryPickup(vector<vector<int>>& grid) 
    {
        int n = grid.size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(n, -1)));
        return max(0, dfs(grid, 0, 0, 0, n, dp));
        
    }

    int dfs(vector<vector<int>>& grid, int i, int j, int k, int n, vector<vector<vector<int>>>& dp) 
    {
        int l = i + j - k;
        if (i >= n or j >= n or k >= n or l >= n or l < 0 or grid[i][j] == -1 or grid[k][l] == -1) return INT_MIN + 100;
        if (dp[i][j][k] != -1) return dp[i][j][k];
        if (i == n - 1 and j == n - 1 and k == n - 1) return dp[i][j][k] = grid[i][j];
        int result = max(max(dfs(grid, i + 1, j, k + 1, n, dp), dfs(grid, i + 1, j, k, n, dp)), max(dfs(grid, i, j + 1, k + 1, n, dp), dfs(grid, i, j + 1, k, n, dp)));
        result += grid[i][j] + grid[k][l] + (i == k and j == l and grid[i][j] == 1 ? -1 : 0);
        return dp[i][j][k] = result; 
    }
};
```

__Java__:

```Java
class Solution {
    public int cherryPickup(int[][] grid) {
        int n = grid.length;
        int[][][] dp = new int[n][n][n];
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) for (int k = 0; k < n; k++) dp[i][j][k] = -1;
        return Math.max(0, dfs(grid, 0, 0, 0, n, dp));
    }
    
    private int dfs(int[][] grid, int i, int j, int k, int n, int dp[][][]) {
        int l = i + j - k;
        if (i >= n || j >= n || k >= n || l >= n || l < 0 || grid[i][j] == -1 || grid[k][l] == -1) return Integer.MIN_VALUE + 100;
        if (dp[i][j][k] != -1) return dp[i][j][k];
        if (i == n - 1 && j == n - 1 && k == n - 1) return dp[i][j][k] = grid[i][j];
        int result = Math.max(Math.max(dfs(grid, i + 1, j, k + 1, n, dp), dfs(grid, i + 1, j, k, n, dp)), Math.max(dfs(grid, i, j + 1, k + 1, n, dp), dfs(grid, i, j + 1, k, n, dp)));
        result += grid[i][j] + grid[k][l] + (i == k && j == l && grid[i][j] == 1 ? -1 : 0);
        return dp[i][j][k] = result; 
    }
}
```

__Python__:

```Python
class Solution:
    def cherryPickup(self, grid: List[List[int]]) -> int:
        n = len(grid)
        dp = [[[-1] * n for _ in range(n)] for __ in range(n)]
        
        def dfs(i: int, j: int, k: int) -> int:
            l = i + j - k
            if i >= n or j >= n or k >= n or l >= n or l < 0 or grid[i][j] == -1 or grid[k][l] == -1:
                return -(1 << 31) + 100
            if dp[i][j][k] != -1:
                return dp[i][j][k];
            if i == n - 1 and j == n - 1 and k == n - 1:
                dp[i][j][k] = grid[i][j]
                return dp[i][j][k]
            result = max(dfs(i + 1, j, k + 1), dfs(i + 1, j, k), dfs(i, j + 1, k + 1), dfs(i, j + 1, k));
            dp[i][j][k] = result + grid[i][j] + grid[k][l] + (-1 if i == k and j == l and grid[i][j] == 1 else 0)
            return dp[i][j][k]
        
        return max(0, dfs(0, 0, 0))
```
