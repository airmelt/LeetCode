# 1162 As Far from Land as Possible 地图分析

__Description__:
Given an n x n grid containing only values 0 and 1, where 0 represents water and 1 represents land, find a water cell such that its distance to the nearest land cell is maximized, and return the distance. If no land or water exists in the grid, return -1.

The distance used in this problem is the Manhattan distance: the distance between two cells (x0, y0) and (x1, y1) is |x0 - x1| + |y0 - y1|.

__Example:__

Example 1:

![Graph 1](https://assets.leetcode.com/uploads/2019/05/03/1336_ex1.JPG)

Input: grid = [[1,0,1],[0,0,0],[1,0,1]]
Output: 2
Explanation: The cell (1, 1) is as far as possible from all the land with distance 2.
Example 2:

![Graph 2](https://assets.leetcode.com/uploads/2019/05/03/1336_ex2.JPG)

Input: grid = [[1,0,0],[0,0,0],[0,0,0]]
Output: 4
Explanation: The cell (2, 2) is as far as possible from all the land with distance 4.

__Constraints:__

n == grid.length
n == grid[i].length
1 <= n <= 100
grid[i][j] is 0 or 1

__题目描述__:
你现在手里有一份大小为 n x n 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地。

请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的，并返回该距离。如果网格上只有陆地或者海洋，请返回 -1。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

__示例 :__

示例 1：

![地图 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/1336_ex1.jpeg)

输入：grid = [[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释：
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
示例 2：

![地图 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/1336_ex2.jpeg)

输入：grid = [[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释：
海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到最大，最大距离为 4。

__提示:__

n == grid.length
n == grid[i].length
1 <= n <= 100
grid[i][j] 不是 0 就是 1

__思路__:

1. BFS
将所有陆地入队, 找到离陆地最远的海洋即为所求
每次 BFS 将距离加 1, 可以直接在原数组中操作, 因为海洋始终为 0
需要记录是否存在海洋以及最远海洋坐标用来返回最大距离
记录每一层的和及对应层数输出最大的和的层的编号
时间复杂度为 O(mn), 空间复杂度为 O(mn)
2. 动态规划
设 dp[i][j] 表示坐标为 (i - 1, j - 1) 的海洋距离陆地的距离
dp[i][j] = 0, if grid[i - 1][j - 1] = 0
dp[i][j] = min(dp[i + 1][j], dp[i][j + 1], dp[i - 1][j], dp[i][j - 1) + 1
由于 1 <= n <= 100, 可以将 dp 初始化为一个大于 100 的数, 方便判断是否有海洋
由于开始时 dp 的值不完全, 需要将动态规划拆分左上和右下两个方向进行
时间复杂度为 O(mn), 空间复杂度为 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxDistance(vector<vector<int>>& grid) 
    {
        queue<pair<int, int>> q;
        int n = grid.size(), ocean = 0, dx[]{0, 0, 1, -1}, dy[]{1, -1, 0, 0}, point[]{-1, -1};
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (grid[i][j]) q.push(make_pair(i, j));
        while (!q.empty())
        {
            int x = point[0] = q.front().first, y = point[1] = q.front().second;
            q.pop();
            for (int i = 0; i < 4; i++) 
            {
                int next_x = x + dx[i], next_y = y + dy[i];
                if (next_x < 0 or next_x >= n or next_y < 0 or next_y >= n or grid[next_x][next_y]) continue;
                grid[next_x][next_y] = grid[x][y] + 1;
                ocean = 1;
                q.push(make_pair(next_x, next_y));
            }
        }
        return point[0] == -1 or !ocean ? -1 : grid[point[0]][point[1]] - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDistance(int[][] grid) {
        int result = -1, n = grid.length, dp[][] = new int[n + 2][n + 2], MAX = 0x3f3f3f3f;
        for (int i = 0; i < n + 2; i++) Arrays.fill(dp[i], MAX);
        for (int i = 1; i <= n; i++) for (int j = 1; j <= n; j++) dp[i][j] = grid[i - 1][j - 1] == 0 ? Math.min(dp[i - 1][j], dp[i][j - 1]) + 1 : 0;
        for (int i = n; i > 0; i--) for (int j = n; j > 0; j--) {
            if (grid[i - 1][j - 1] == 0) {
                dp[i][j] = Math.min(Math.min(dp[i + 1][j], dp[i][j + 1]) + 1, dp[i][j]);
                result = Math.max(dp[i][j], result);
            } else dp[i][j] = 0;
        }
        return result != -1 && result < MAX ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDistance(self, grid: List[List[int]]) -> int:
        n, result = len(grid), -1
        dp = [[float('inf')] * (n + 2) for _ in range(n + 2)]
        for i in range(1, n + 1):
            for j in range(1, n + 1):
                dp[i][j] = 0 if grid[i - 1][j - 1] else min(dp[i - 1][j], dp[i][j - 1]) + 1
        for i in range(n, 0, -1):
            for j in range(n, 0, -1):
                dp[i][j], result = (0, result) if grid[i - 1][j - 1] else (a := min(dp[i + 1][j] + 1, dp[i][j + 1] + 1, dp[i][j]), max(a, result))
        return result if result != -1 and result != float("inf") else -1
```
