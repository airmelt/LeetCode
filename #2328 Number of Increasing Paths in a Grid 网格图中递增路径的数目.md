# 2328 Number of Increasing Paths in a Grid 网格图中递增路径的数目

__Description:__

You are given an `m x n` integer matrix `grid`, where you can move from a cell to any adjacent cell in all `4` directions.

Return _the number of __strictly__ __increasing__ paths in the grid such that you can start from __any__ cell and end at __any__ cell._ Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Two paths are considered different if they do not have exactly the same sequence of visited cells.

__Example:__

Example 1:

![2328-1](https://assets.leetcode.com/uploads/2022/05/10/griddrawio-4.png)

```text
Input: grid = [[1,1],[3,4]]
Output: 8
Explanation: The strictly increasing paths are:
```

- Paths with length 1: [1], [1], [3], [4].
- Paths with length 2: [1 -> 3], [1 -> 4], [3 -> 4].
- Paths with length 3: [1 -> 3 -> 4].

The total number of paths is 4 + 3 + 1 = 8.

Example 2:

```text
Input: grid = [[1],[2]]
Output: 3
Explanation: The strictly increasing paths are:
```

- Paths with length 1: [1], [2].
- Paths with length 2: [1 -> 2].

The total number of paths is 2 + 1 = 3.

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 5`

__题目描述:__

给你一个 `m x n` 的整数网格图 `grid` ，你可以从一个格子移动到 `4` 个方向相邻的任意一个格子。

请你返回在网格图中从 __任意__ 格子出发，达到 __任意__ 格子，且路径中的数字是 __严格递增__ 的路径数目。由于答案可能会很大，请将结果对 `10 ^ 9 + 7` __取余__ 后返回。

如果两条路径中访问过的格子不是完全相同的，那么它们视为两条不同的路径。

__示例:__

示例 1：

![2328-2](https://assets.leetcode.com/uploads/2022/05/10/griddrawio-4.png)

```text
输入：grid = [[1,1],[3,4]]
输出：8
解释：严格递增路径包括：
```

- 长度为 1 的路径：[1]，[1]，[3]，[4] 。
- 长度为 2 的路径：[1 -> 3]，[1 -> 4]，[3 -> 4] 。
- 长度为 3 的路径：[1 -> 3 -> 4] 。

路径数目为 4 + 3 + 1 = 8 。

示例 2：

```text
输入：grid = [[1],[2]]
输出：3
解释：严格递增路径包括：
```

- 长度为 1 的路径：[1]，[2] 。
- 长度为 2 的路径：[1 -> 2] 。

路径数目为 2 + 1 = 3 。

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 5`

__思路:__

```text
记忆化
设 dp[i][j] 表示从 (i, j) 出发的严格递增路径数目
dp[i][j] = 1 + sum(dp[x][y]), (x, y) 为 (i, j) 的下一个位置, 且 grid[x][y] > grid[i][j]
最后结果为 sum(dp[i][j])
时间复杂度为 O(MN), 空间复杂度为 O(1), 不计返回的结果矩阵    
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPaths(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), MOD = 1e9 + 7, dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, result = 0;
        vector<vector<int>> dp(m, vector<int>(n, -1));
        function<int(int, int)> dfs = [&](int i, int j) -> int 
        {
            if (dp[i][j] != -1) return dp[i][j];
            int r = 1;
            for (const auto& d : dir) if (-1 < i + d[0] and i + d[0] < m and -1 < j + d[1] and j + d[1] < n and grid[i + d[0]][j + d[1]] > grid[i][j]) r = (r + dfs(i + d[0], j + d[1])) % MOD;
            return dp[i][j] = r;
        };
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result = (result + dfs(i, j)) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    int[][] grid, dp;
    int m, n;
    static final int MOD = 1_000_000_007;
    static final int[][] dir = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int countPaths(int[][] grid) {
        m = grid.length;
        n = grid[0].length;
        this.grid = grid;
        dp = new int[m][n];
        for (int i = 0; i < m; i++) Arrays.fill(dp[i], -1);
        int result = 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result = (result + dfs(i, j)) % MOD;
        return result;
    }

    private int dfs(int i, int j) {
        if (dp[i][j] != -1) return dp[i][j];
        int result = 1;
        for (int[] d : dir) if (-1 < i + d[0] && i + d[0] < m && -1 < j + d[1] && j + d[1] < n && grid[i + d[0]][j + d[1]] > grid[i][j]) result = (result + dfs(i + d[0], j + d[1])) % MOD;
        return dp[i][j] = result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPaths(self, grid: List[List[int]]) -> int:
        MOD, m, n = 10 ** 9 + 7, len(grid), len(grid[0])
        @lru_cache(None)
        def dfs(i: int, j: int) -> int:
            result = 1
            for x, y in (i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1):
                if -1 < x < m and -1 < y < n and grid[x][y] > grid[i][j]:
                    result += dfs(x, y)
            return result % MOD
        return sum(dfs(i, j) for i in range(m) for j in range(n)) % MOD
```
