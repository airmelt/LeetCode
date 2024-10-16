# 2290 Minimum Obstacle Removal to Reach Corner 到达角落需要移除障碍物的最小数目

__Description:__

You are given a __0-indexed__ 2D integer array `grid` of size `m x n`. Each cell has one of two values:

- `0` represents an __empty__ cell,
- `1` represents an __obstacle__ that may be removed.

You can move up, down, left, or right from and to an empty cell.

Return _the __minimum__ number of __obstacles__ to __remove__ so you can move from the upper left corner_ `(0, 0)` _to the lower right corner_ `(m - 1, n - 1)`.

__Example:__

Example 1:

![2290-1](https://assets.leetcode.com/uploads/2022/04/06/example1drawio-1.png)

```text
Input: grid = [[0,1,1],[1,1,0],[1,1,0]]
Output: 2
Explanation: We can remove the obstacles at (0, 1) and (0, 2) to create a path from (0, 0) to (2, 2).
It can be shown that we need to remove at least 2 obstacles, so we return 2.
Note that there may be other ways to remove 2 obstacles to create a path.
```

Example 2:

![2290-2](https://assets.leetcode.com/uploads/2022/04/06/example1drawio.png)

```text
Input: grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]]
Output: 0
Explanation: We can move from (0, 0) to (2, 4) without removing any obstacles, so we return 0.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `2 <= m * n <= 10 ^ 5`
- `grid[i][j]` is either `0` __or__ `1`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `grid` ，数组大小为 `m x n` 。每个单元格都是两个值之一:

- `0` 表示一个 __空__ 单元格，
- `1` 表示一个可以移除的 __障碍物__ 。

你可以向上、下、左、右移动，从一个空单元格移动到另一个空单元格。

现在你需要从左上角 `(0, 0)` 移动到右下角 `(m - 1, n - 1)` ，返回需要移除的障碍物的 __最小__ 数目。

__示例:__

示例 1：

![2290-3](https://assets.leetcode.com/uploads/2022/04/06/example1drawio-1.png)

```text
输入：grid = [[0,1,1],[1,1,0],[1,1,0]]
输出：2
解释：可以移除位于 (0, 1) 和 (0, 2) 的障碍物来创建从 (0, 0) 到 (2, 2) 的路径。
可以证明我们至少需要移除两个障碍物，所以返回 2 。
注意，可能存在其他方式来移除 2 个障碍物，创建出可行的路径。
```

示例 2：

![2290-4](https://assets.leetcode.com/uploads/2022/04/06/example1drawio.png)

```text
输入：grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]]
输出：0
解释：不移除任何障碍物就能从 (0, 0) 到 (2, 4) ，所以返回 0 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `2 <= m * n <= 10 ^ 5`
- `grid[i][j]` 为 `0` __或__ `1`
- `grid[0][0] == grid[m - 1][n - 1] == 0`

__思路:__

```text
BFS
从左上角开始, 用一个二维数组 result 记录到达每个位置的最小障碍物数目
每次从队列中选择一个最小障碍物数目的位置, 并更新其周围的位置
可以用一个双端队列 deque 来存储
如果当前位置的值为 0, 则将其加入队列头部, 否则加入队列尾部
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumObstacles(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), INF = 0x3f3f3f3f, nx = 0, ny = 0, cur = 0;
        vector<vector<int>> d{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}, result(m, vector<int>(n, INF));
        result.front().front() = 0;
        deque<pair<int, int>> q;
        q.emplace_front(0, 0);
        while (!q.empty()) 
        {
            auto [x, y] = q.front();
            q.pop_front();
            for (const auto& dir : d) 
            {
                if ((-1 < (nx = x + dir.front()) and nx < m and -1 < (ny = y + dir.back()) and ny < n) and (result[x][y] + (cur = grid[nx][ny]) < result[nx][ny])) 
                {
                    result[nx][ny] = result[x][y] + cur;
                    !cur ? q.emplace_front(nx, ny) : q.emplace_back(nx, ny);
                }
            }
        }
        return result.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumObstacles(int[][] grid) {
        int m = grid.length, n = grid[0].length, d[][] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}}, result[][] = new int[m][n], INF = 0x3f3f3f3f, nx = 0, ny = 0, cur = 0;
        for (int i = 0; i < m; i++) Arrays.fill(result[i], INF);
        result[0][0] = 0;
        Deque<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{0, 0});
        while (!queue.isEmpty()) {
            int point[] = queue.poll(), x = point[0], y = point[1];
            for (int[] dir : d) {
                if ((-1 < (nx = x + dir[0]) && nx < m && -1 < (ny = y + dir[1]) && ny < n) && (result[x][y] + (cur = grid[nx][ny]) < result[nx][ny])) {
                    result[nx][ny] = result[x][y] + cur;
                    if (cur == 0) queue.addFirst(new int[]{nx, ny});
                    else queue.addLast(new int[]{nx, ny});
                }
            }
        }
        return result[m - 1][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumObstacles(self, grid: List[List[int]]) -> int:
        m, n, q, d = len(grid), len(grid[0]), deque([(0, 0)]), [(0, 1), (1, 0), (0, -1), (-1, 0)]
        result = [[inf] * n for _ in range(m)]
        result[0][0] = 0
        while q:
            x, y = q.popleft()
            for dx, dy in d:
                if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and result[x][y] + (cur := grid[nx][ny]) < result[nx][ny]:
                    result[nx][ny] = result[x][y] + cur
                    if not cur:
                        q.appendleft((nx, ny))
                    else:
                        q.append((nx, ny))
        return result[-1][-1]
```
