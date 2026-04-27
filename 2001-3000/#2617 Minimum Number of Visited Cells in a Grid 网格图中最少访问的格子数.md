# 2617 Minimum Number of Visited Cells in a Grid 网格图中最少访问的格子数

__Description:__

You are given a __0-indexed__ `m x n` integer matrix `grid`. Your initial position is at the __top-left__ cell `(0, 0)`.

Starting from the cell `(i, j)`, you can move to one of the following cells:

- Cells `(i, k)` with `j < k <= grid[i][j] + j` (rightward movement), or
- Cells `(k, j)` with `i < k <= grid[i][j] + i` (downward movement).

Return _the minimum number of cells you need to visit to reach the __bottom-right__ cell_ `(m - 1, n - 1)`. If there is no valid path, return `-1`.

__Example:__

Example 1:

![2617-1](https://assets.leetcode.com/uploads/2023/01/25/ex1.png)

```text
Input: grid = [[3,4,2,1],[4,2,3,1],[2,1,0,0],[2,4,0,0]]
Output: 4
Explanation: The image above shows one of the paths that visits exactly 4 cells.
```

Example 2:

![2617-2](https://assets.leetcode.com/uploads/2023/01/25/ex2.png)

```text
Input: grid = [[3,4,2,1],[4,2,1,1],[2,1,1,0],[3,4,1,0]]
Output: 3
Explanation: The image above shows one of the paths that visits exactly 3 cells.
```

Example 3:

![2617-3](https://assets.leetcode.com/uploads/2023/01/26/ex3.png)

```text
Input: grid = [[2,1,0],[1,0,0]]
Output: -1
Explanation: It can be proven that no path exists.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] < m * n`
- `grid[m - 1][n - 1] == 0`

__题目描述:__

给你一个下标从 __0__ 开始的 `m x n` 整数矩阵 `grid` 。你一开始的位置在 __左上角__ 格子 `(0, 0)` 。

当你在格子 `(i, j)` 的时候，你可以移动到以下格子之一:

- 满足 `j < k <= grid[i][j] + j` 的格子 `(i, k)` （向右移动），或者
- 满足 `i < k <= grid[i][j] + i` 的格子 `(k, j)` （向下移动）。

请你返回到达 __右下角__ 格子 `(m - 1, n - 1)` 需要经过的最少移动格子数，如果无法到达右下角格子，请你返回 `-1` 。

__示例:__

示例 1：

![2617-4](https://assets.leetcode.com/uploads/2023/01/25/ex1.png)

```text
输入：grid = [[3,4,2,1],[4,2,3,1],[2,1,0,0],[2,4,0,0]]
输出：4
解释：上图展示了到达右下角格子经过的 4 个格子。
```

示例 2：

![2617-5](https://assets.leetcode.com/uploads/2023/01/25/ex2.png)

```text
输入：grid = [[3,4,2,1],[4,2,1,1],[2,1,1,0],[3,4,1,0]]
输出：3
解释：上图展示了到达右下角格子经过的 3 个格子。
```

示例 3：

![2617-6](https://assets.leetcode.com/uploads/2023/01/26/ex3.png)

```text
输入：grid = [[2,1,0],[1,0,0]]
输出：-1
解释：无法到达右下角格子。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] < m * n`
- `grid[m - 1][n - 1] == 0`

__思路:__

```text
贪心 ➕ 优先队列
从左上角开始, 使用优先队列维护当前行和列的最小访问格子数
记录从 (0, 0) 到 (i, j) 的最小访问格子数 f
设从 (i, j) 往右最远可以到达 (i, grid[i][j] + j), 将 (f, grid[i][j] + j) 加入当前行的优先队列
设从 (i, j) 往下最远可以到达 (grid[i][j] + i, j), 将 (f, grid[i][j] + i) 加入当前列的优先队列
起点 (0, 0) 的 f = 1
如果 (i, j) 不能到达则 f = inf
每次遍历时先将不能到达的格子从优先队列中移除  
如果行不为空, 则 f = min(f, row.top().first + 1)
如果列不为空, 则 f = min(f, cols[j].top().first + 1)
时间复杂度为 O(MN(logM + logN)), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumVisitedCells(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), f = 0, inf = 0x3f3f3f3f;
        vector<priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>>> cols(n);
        for (int i = 0; i < m; i++) 
        {
            priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> row;
            for (int j = 0; j < n; j++) 
            {
                while (!row.empty() and row.top().second < j) row.pop();
                while (!cols[j].empty() and cols[j].top().second < i) cols[j].pop();
                f = i || j ? inf : 1;
                if (!row.empty()) f = row.top().first + 1;
                if (!cols[j].empty()) f = min(f, cols[j].top().first + 1);
                if (grid[i][j] and f < inf) 
                {
                    row.emplace(f, grid[i][j] + j);
                    cols[j].emplace(f, grid[i][j] + i);
                }
            }
        }
        return f < inf ? f : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumVisitedCells(int[][] grid) {
        int m = grid.length, n = grid[0].length, f = 0, inf = 0x3f3f3f3f;
        PriorityQueue<int[]>[] cols = new PriorityQueue[n];
        Arrays.setAll(cols, i -> new PriorityQueue<int[]>((a, b) -> a[0] - b[0]));
        PriorityQueue<int[]> row = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < m; i++) {
            row.clear();
            for (int j = 0; j < n; j++) {
                while (!row.isEmpty() && row.peek()[1] < j) row.poll();
                while (!cols[j].isEmpty() && cols[j].peek()[1] < i) cols[j].poll();
                f = i > 0 || j > 0 ? inf : 1;
                if (!row.isEmpty()) f = row.peek()[0] + 1;
                if (!cols[j].isEmpty()) f = Math.min(f, cols[j].peek()[0] + 1);
                if (grid[i][j] > 0 && f < inf) {
                    row.offer(new int[]{ f, grid[i][j] + j });
                    cols[j].offer(new int[]{ f, grid[i][j] + i });
                }
            }
        }
        return f < inf ? f : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumVisitedCells(self, grid: List[List[int]]) -> int:
        heaps = [[] for _ in grid[0]]
        for i, r in enumerate(grid):
            row = []
            for j, (g, col) in enumerate(zip(r, heaps)):
                while row and row[0][1] < j:
                    heappop(row)
                while col and col[0][1] < i:
                    heappop(col)
                f = inf if i or j else 1 
                if row: 
                    f = row[0][0] + 1
                if col: 
                    f = min(f, col[0][0] + 1)
                if g and f < inf:
                    heappush(row, (f, g + j))
                    heappush(col, (f, g + i))
        return f if f < inf else -1
```
