# 2556 Disconnect Path in a Binary Matrix by at Most One Flip 二进制矩阵中翻转最多一次使路径不连通

__Description:__

You are given a __0-indexed__ `m x n` __binary__ matrix `grid`. You can move from a cell `(row, col)` to any of the cells `(row + 1, col)` or `(row, col + 1)` that has the value `1`. The matrix is __disconnected__ if there is no path from `(0, 0)` to `(m - 1, n - 1)`.

You can flip the value of __at most one__ (possibly none) cell. You __cannot flip__ the cells `(0, 0)` and `(m - 1, n - 1)`.

Return `true` _if it is possible to make the matrix disconnect or_ `false` _otherwise_.

__Note__ that flipping a cell changes its value from `0` to `1` or from `1` to `0`.

__Example:__

Example 1:

![2556-1](https://assets.leetcode.com/uploads/2022/12/07/yetgrid2drawio.png)

```text
Input: grid = [[1,1,1],[1,0,0],[1,1,1]]
Output: true
Explanation: We can change the cell shown in the diagram above. There is no path from (0, 0) to (2, 2) in the resulting grid.
```

Example 2:

![2556-2](https://assets.leetcode.com/uploads/2022/12/07/yetgrid3drawio.png)

```text
Input: grid = [[1,1,1],[1,0,1],[1,1,1]]
Output: false
Explanation: It is not possible to change at most one cell such that there is not path from (0, 0) to (2, 2).
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `grid[i][j]` is either `0` or `1`.
- `grid[0][0] == grid[m - 1][n - 1] == 1`

__题目描述:__

给你一个下标从 __0__ 开始的 `m x n` __二进制__ 矩阵 `grid` 。你可以从一个格子 `(row, col)` 移动到格子 `(row + 1, col)` 或者 `(row, col + 1)` ，前提是前往的格子值为 `1` 。如果从 `(0, 0)` 到 `(m - 1, n - 1)` 没有任何路径，我们称该矩阵是 __不连通__ 的。

你可以翻转 __最多一个__ 格子的值（也可以不翻转）。你 __不能翻转__ 格子 `(0, 0)` 和 `(m - 1, n - 1)` 。

如果可以使矩阵不连通，请你返回 `true` ，否则返回 `false` 。

__注意__ ，翻转一个格子的值，可以使它的值从 `0` 变 `1` ，或从 `1` 变 `0` 。

__示例:__

示例 1：

![2556-3](https://assets.leetcode.com/uploads/2022/12/07/yetgrid2drawio.png)

```text
输入：grid = [[1,1,1],[1,0,0],[1,1,1]]
输出：true
解释：按照上图所示我们翻转蓝色格子里的值，翻转后从 (0, 0) 到 (2, 2) 没有路径。
```

示例 2：

![2556-4](https://assets.leetcode.com/uploads/2022/12/07/yetgrid3drawio.png)

```text
输入：grid = [[1,1,1],[1,0,1],[1,1,1]]
输出：false
解释：无法翻转至多一个格子，使 (0, 0) 到 (2, 2) 没有路径。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10 ^ 5`
- `grid[0][0] == grid[m - 1][n - 1] == 1`

__思路:__

```text
DFS
尝试从左上走到右下
如果可以走到右下, 则说明矩阵是连通的
将当前格子值设为 0, 继续尝试从左上走到右下
如果可以走到右下, 则说明矩阵仍然是连通的, 返回 false
如果无法走到右下, 则说明矩阵不连通, 返回 true
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isPossibleToCutPath(vector<vector<int>>& grid) 
    {
        auto dfs = [&](this auto&& dfs, int x, int y) -> bool
        {
            if (x == grid.size() - 1 and y == grid.front().size() - 1) return true;
        grid[x][y] = 0;
        return x < grid.size() - 1 and grid[x + 1][y] and dfs(x + 1, y) or y < grid.front().size() - 1 and grid[x][y + 1] and dfs(x, y + 1);
        };
        return !dfs(0, 0) or !dfs(0, 0);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isPossibleToCutPath(int[][] grid) {
        return !dfs(0, 0, grid) || !dfs(0, 0, grid);
    }

    private boolean dfs(int x, int y, int[][] grid) {
        if (x == grid.length - 1 && y == grid[0].length - 1) return true;
        grid[x][y] = 0;
        return x < grid.length - 1 && grid[x + 1][y] == 1 && dfs(x + 1, y, grid) || y < grid[0].length - 1 && grid[x][y + 1] == 1 && dfs(x, y + 1, grid);
    }
}
```

__Python__:

```Python
class Solution:
    def isPossibleToCutPath(self, grid: List[List[int]]) -> bool:
        def dfs(x: int, y: int) -> bool:
            if x == len(grid) - 1 and y == len(grid[0]) - 1:
                return True
            grid[x][y] = 0
            return x < len(grid) - 1 and grid[x + 1][y] and dfs(x + 1, y) or y < len(grid[0]) - 1 and grid[x][y + 1] and dfs(x, y + 1)
        return not dfs(0, 0) or not dfs(0, 0)
            
```
