# 1210 Minimum Moves to Reach Target with Rotations 穿过迷宫的最少移动次数

__Description:__

In an n*n grid, there is a snake that spans 2 cells and starts moving from the top left corner at (0, 0) and (0, 1). The grid has empty cells represented by zeros and blocked cells represented by ones. The snake wants to reach the lower right corner at (n-1, n-2) and (n-1, n-1).

In one move the snake can:

Move one cell to the right if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
Move down one cell if there are no blocked cells there. This move keeps the horizontal/vertical position of the snake as it is.
Rotate clockwise if it's in a horizontal position and the two cells under it are both empty. In that case the snake moves from (r, c) and (r, c+1) to (r, c) and (r+1, c).

![Rotate clockwise](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image-2.png)

Rotate counterclockwise if it's in a vertical position and the two cells to its right are both empty. In that case the snake moves from (r, c) and (r+1, c) to (r, c) and (r, c+1).

![Rotate counterclockwise](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image-1.png)

Return the minimum number of moves to reach the target.

If there is no way to reach the target, return -1.

__Example:__

Example 1:

![maze](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image.png)

```text
Input: grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
               [0,0,0,0,1,1],
               [0,0,1,0,1,0],
               [0,1,1,0,0,0],
               [0,1,1,0,0,0]]
```

Output: 11
Explanation:
One possible solution is [right, right, rotate clockwise, right, down, down, down, down, rotate counterclockwise, right, down].

Example 2:

```text
Input: grid = [[0,0,1,1,1,1],
               [0,0,0,0,1,1],
               [1,1,0,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,0]]
```

Output: 9

__Constraints:__

2 <= n <= 100
0 <= grid[i][j] <= 1
It is guaranteed that the snake starts at empty cells.

__题目描述：__

你还记得那条风靡全球的贪吃蛇吗？

我们在一个 n*n 的网格上构建了新的迷宫地图，蛇的长度为 2，也就是说它会占去两个单元格。蛇会从左上角（(0, 0) 和 (0, 1)）开始移动。我们用 0 表示空单元格，用 1 表示障碍物。蛇需要移动到迷宫的右下角（(n-1, n-2) 和 (n-1, n-1)）。

每次移动，蛇可以这样走：

如果没有障碍，则向右移动一个单元格。并仍然保持身体的水平／竖直状态。
如果没有障碍，则向下移动一个单元格。并仍然保持身体的水平／竖直状态。
如果它处于水平状态并且其下面的两个单元都是空的，就顺时针旋转 90 度。蛇从（(r, c)、(r, c+1)）移动到 （(r, c)、(r+1, c)）。

![顺时针旋转](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image-2.png)

如果它处于竖直状态并且其右面的两个单元都是空的，就逆时针旋转 90 度。蛇从（(r, c)、(r+1, c)）移动到（(r, c)、(r, c+1)）。

![逆时针旋转](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image-1.png)

返回蛇抵达目的地所需的最少移动次数。

如果无法到达目的地，请返回 -1。

__示例：__

示例 1：

![迷宫](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/09/28/image.png)

```text
输入：grid = [[0,0,0,0,0,1],
               [1,1,0,0,1,0],
               [0,0,0,0,1,1],
               [0,0,1,0,1,0],
               [0,1,1,0,0,0],
               [0,1,1,0,0,0]]
```

输出：11
解释：
一种可能的解决方案是 [右, 右, 顺时针旋转, 右, 下, 下, 下, 下, 逆时针旋转, 右, 下]。

示例 2：

```text
输入：grid = [[0,0,1,1,1,1],
               [0,0,0,0,1,1],
               [1,1,0,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,1],
               [1,1,1,0,0,0]]
```

输出：9

__提示：__

2 <= n <= 100
0 <= grid[i][j] <= 1
蛇保证从空单元格开始出发。

__思路：__

动态规划
注意贪吃蛇只能从水平方向变为竖直方向和竖直方向变为水平方向, 只有两个状态, 不能无限旋转
分别记录蛇尾在 (i, j), 蛇为水平和竖直方向的移动次数
水平方向先不考虑旋转
如果水平方向可以移动, 要么从水平方向的左边一格移动过来, 要么从水平方向的上边一格
即 hdp[i + 1][j + 1] = min(hdp[i + 1][j + 1], min(hdp[i + 1][j], hdp[i][j + 1]) + 1)
考虑旋转的情况, 若移动之后为水平方向, 也就是说是从竖直方向旋转过来, 蛇尾应该从当前位置顺时针旋转, 即 hdp[i + 1][j + 1] = vdp[i + 1][j + 1] + 1, 且当前位置的左边一格和上边一格都必须没有障碍物
竖直方向处理类似
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    static constexpr int INF = 0x3f3f3f3f;
    bool check(const vector<vector<int>>& grid, const int i, const int j, const int n)
    {
        return i > -1 and i < n and j > -1 and j < n and !grid[i][j];
    }
public:
    int minimumMoves(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> hdp(n + 1, vector<int>(n + 1, INF)), vdp(n + 1, vector<int>(n + 1, INF));
        hdp[1][1] = 0;
        for (int i = 0; i < n; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j]) continue;
                if (check(grid, i, j + 1, n)) hdp[i + 1][j + 1] = min(hdp[i + 1][j + 1], min(hdp[i + 1][j], hdp[i][j + 1]) + 1);
                if (check(grid, i + 1, j, n)) vdp[i + 1][j + 1] = min(vdp[i + 1][j + 1], min(vdp[i + 1][j], vdp[i][j + 1]) + 1);
                if (check(grid, i, j + 1, n) && check(grid, i + 1, j + 1, n)) hdp[i + 1][j + 1] = min(hdp[i + 1][j + 1], vdp[i + 1][j + 1] + 1);
                if (check(grid, i + 1, j, n) && check(grid, i + 1, j + 1, n)) vdp[i + 1][j + 1] = min(hdp[i + 1][j + 1] + 1, vdp[i + 1][j + 1]);
            }
        }
        return hdp[n][n - 1] == INF ? -1 : hdp[n][n - 1];
    }
};
```

__Java__:

```Java
class Solution {
    private static final int INF = 0x3f3f3f3f;
    
    private static final boolean check(int[][] grid, int i, int j, int n) {
        return i > -1 && i < n && j > -1 && j < n && grid[i][j] == 0;
    }
    
    public int minimumMoves(int[][] grid) {
        int n = grid.length, hdp[][] = new int[n + 1][n + 1], vdp[][] = new int[n + 1][n + 1];
        for (int[] row : hdp) Arrays.fill(row, INF);
        for (int[] row : vdp) Arrays.fill(row, INF);
        hdp[1][1] = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != 0) continue;
                if (check(grid, i, j + 1, n)) hdp[i + 1][j + 1] = Math.min(hdp[i + 1][j + 1], Math.min(hdp[i + 1][j], hdp[i][j + 1]) + 1);
                if (check(grid, i + 1, j, n)) vdp[i + 1][j + 1] = Math.min(vdp[i + 1][j + 1], Math.min(vdp[i + 1][j], vdp[i][j + 1]) + 1);
                if (check(grid, i, j + 1, n) && check(grid, i + 1, j + 1, n)) hdp[i + 1][j + 1] = Math.min(hdp[i + 1][j + 1], vdp[i + 1][j + 1] + 1);
                if (check(grid, i + 1, j, n) && check(grid, i + 1, j + 1, n)) vdp[i + 1][j + 1] = Math.min(hdp[i + 1][j + 1] + 1, vdp[i + 1][j + 1]);
            }
        }
        return hdp[n][n - 1] == INF ? -1 : hdp[n][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        n = len(grid)
        @lru_cache(None)
        def dfs(i: int, j: int, state: bool, flag: bool) -> int:
            if i == n - 1 and j == n - 1 and not state:
                return 0
            result = float("inf")
            if not state:
                if j + 1 < n and not grid[i][j + 1]:
                    result = min(result, 1 + dfs(i, j + 1, state, 0))
                if i + 1 < n and not grid[i + 1][j - 1] and not grid[i + 1][j]:
                    result = min(result, 1 + dfs(i + 1, j, state, 0))
                if i + 1 < n and not grid[i + 1][j] and not grid[i + 1][j - 1] and not flag:
                    result = min(result, 1 + dfs(i + 1, j - 1, 1, 1))
            else:
                if i + 1 < n and not grid[i + 1][j]:
                    result = min(result, 1 + dfs(i + 1, j, state, 0))
                if j + 1 < n and not grid[i][j + 1] and not grid[i - 1][j + 1]:
                    result = min(result, 1 + dfs(i, j + 1, state, 0))
                if j + 1 < n and not grid[i][j + 1] and not grid[i - 1][j + 1] and not flag:
                    result = min(result, 1 + dfs(i - 1, j + 1, 0, 1))
            return result
        return result if (result := dfs(0, 1, 0, 0)) < float("inf") else -1
```
