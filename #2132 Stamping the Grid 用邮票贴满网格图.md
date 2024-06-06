# 2132 Stamping the Grid 用邮票贴满网格图

__Description:__

You are given an `m x n` binary matrix `grid` where each cell is either `0` (empty) or `1` (occupied).

You are then given stamps of size `stampHeight x stampWidth`. We want to fit the stamps such that they follow the given __restrictions__ and __requirements__:

Return `true` _if it is possible to fit the stamps while following the given restrictions and requirements. Otherwise, return_ `false`.

__Example:__

Example 1:

![2132-1](https://assets.leetcode.com/uploads/2021/11/03/ex1.png)

```text
Input: grid = [[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0]], stampHeight = 4, stampWidth = 3
Output: true
Explanation: We have two overlapping stamps (labeled 1 and 2 in the image) that are able to cover all the empty cells.
```

Example 2:

![2132-2](https://assets.leetcode.com/uploads/2021/11/03/ex2.png)

```text
Input: grid = [[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]], stampHeight = 2, stampWidth = 2 
Output: false 
Explanation: There is no way to fit the stamps onto all the empty cells without the stamps going outside the grid.
```

__Constraints:__

- `m == grid.length`
- `n == grid[r].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 2 * 10 ^ 5`
- `grid[r][c]` is either `0` or `1`.
- `1 <= stampHeight, stampWidth <= 10 ^ 5`

__题目描述:__

给你一个 `m x n` 的二进制矩阵 `grid` ，每个格子要么为 `0` （空）要么为 `1` （被占据）。

给你邮票的尺寸为 `stampHeight x stampWidth` 。我们想将邮票贴进二进制矩阵中，且满足以下 __限制__ 和 __要求__ :

如果在满足上述要求的前提下，可以放入邮票，请返回 `true` ，否则返回 __`false` 。

__示例:__

示例 1：

![2132-3](https://assets.leetcode.com/uploads/2021/11/03/ex1.png)

```text
输入：grid = [[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0],[1,0,0,0]], stampHeight = 4, stampWidth = 3
输出：true
解释：我们放入两个有重叠部分的邮票（图中标号为 1 和 2），它们能覆盖所有与空格子。
```

示例 2：

![2132-4](https://assets.leetcode.com/uploads/2021/11/03/ex2.png)

```text
输入：grid = [[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]], stampHeight = 2, stampWidth = 2 
输出：false 
解释：没办法放入邮票覆盖所有的空格子，且邮票不超出网格图以外。
```

__提示：__

- `m == grid.length`
- `n == grid[r].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 2 * 10 ^ 5`
- `grid[r][c]` 要么是 `0` ，要么是 `1` 。
- `1 <= stampHeight, stampWidth <= 10 ^ 5`

__思路:__

```text
前缀和 ➕ 差分数组
用前缀和计算 grid, 则如果一个区域为空, 前缀和为 0
遍历所有可能的邮票位置, 计算差分数组 d
求出差分数组 d 的前缀和, 如果某个位置的前缀和为 0, 则说明这个位置为空
即未被邮票覆盖, 返回 false
否则返回 true
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool possibleToStamp(vector<vector<int>>& grid, int stampHeight, int stampWidth) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> pre(m + 1, vector<int>(n + 1)), d(m + 2, vector<int>(n + 2));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j];
        for (int i = stampHeight; i <= m; i++) 
        {
            for (int j = stampWidth; j <= n; j++) 
            {
                int x = i - stampHeight + 1, y = j - stampWidth + 1;
                if (!(pre[i][j] - pre[i][y - 1] - pre[x - 1][j] + pre[x - 1][y - 1])) 
                {
                    ++d[x][y];
                    --d[x][j + 1];
                    --d[i + 1][y];
                    ++d[i + 1][j + 1];
                }
            }
        }
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                d[i + 1][j + 1] += d[i + 1][j] + d[i][j + 1] - d[i][j];
                if (!grid[i][j] and !d[i + 1][j + 1]) return false;
            }
        }
        return true; 
    }
};
```

__Java__:

```Java
class Solution {
    public boolean possibleToStamp(int[][] grid, int stampHeight, int stampWidth) {
        int m = grid.length, n = grid[0].length, pre[][] = new int[m + 1][n + 1], d[][] = new int[m + 2][n + 2];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j];
        for (int i = stampHeight; i <= m; i++) {
            for (int j = stampWidth; j <= n; j++) {
                int x = i - stampHeight + 1, y = j - stampWidth + 1;
                if (pre[i][j] - pre[i][y - 1] - pre[x - 1][j] + pre[x - 1][y - 1] == 0) {
                    ++d[x][y];
                    --d[x][j + 1];
                    --d[i + 1][y];
                    ++d[i + 1][j + 1];
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                d[i + 1][j + 1] += d[i + 1][j] + d[i][j + 1] - d[i][j];
                if (grid[i][j] == 0 && d[i + 1][j + 1] == 0) return false;
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def possibleToStamp(self, grid: List[List[int]], stampHeight: int, stampWidth: int) -> bool:
        m, n = len(grid), len(grid[0])
        pre, d = [[0] * (n + 1) for _ in range(m + 1)], [[0] * (n + 2) for _ in range(m + 2)]
        for i in range(m):
            for j in range(n):
                pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j]
        for i in range(stampHeight, m + 1):
            for j in range(stampWidth, n + 1):
                x, y = i - stampHeight + 1, j - stampWidth + 1
                if not pre[i][j] - pre[i][y - 1] - pre[x - 1][j] + pre[x - 1][y - 1]:
                    d[x][y] += 1
                    d[x][j + 1] -= 1
                    d[i + 1][y] -= 1
                    d[i + 1][j + 1] += 1
        for i in range(m):
            for j in range(n):
                d[i + 1][j + 1] += d[i + 1][j] + d[i][j + 1] - d[i][j]
                if not grid[i][j] and not d[i + 1][j + 1]:
                    return False
        return True
```
