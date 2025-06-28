# 2577 Minimum Time to Visit a Cell In a Grid 在网格图中访问一个格子的最少时间

__Description:__

You are given a `m x n` matrix `grid` consisting of _non-negative_ integers where `grid[row][col]` represents the __minimum__ time required to be able to visit the cell `(row, col)`, which means you can visit the cell `(row, col)` only when the time you visit it is greater than or equal to `grid[row][col]`.

You are standing in the __top-left__ cell of the matrix in the `0 ^ th` second, and you must move to __any__ adjacent cell in the four directions: up, down, left, and right. Each move you make takes 1 second.

Return _the __minimum__ time required in which you can visit the bottom-right cell of the matrix_. If you cannot visit the bottom-right cell, then return `-1`.

__Example:__

Example 1:

![2577-1](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)

```text
Input: grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
Output: 7
Explanation: One of the paths that we can take is the following:
```

- at t = 0, we are on the cell (0,0).
- at t = 1, we move to the cell (0,1). It is possible because grid\[0]\[1] <= 1.
- at t = 2, we move to the cell (1,1). It is possible because grid\[1]\[1] <= 2.
- at t = 3, we move to the cell (1,2). It is possible because grid\[1]\[2] <= 3.
- at t = 4, we move to the cell (1,1). It is possible because grid\[1]\[1] <= 4.
- at t = 5, we move to the cell (1,2). It is possible because grid\[1]\[2] <= 5.
- at t = 6, we move to the cell (1,3). It is possible because grid\[1]\[3] <= 6.
- at t = 7, we move to the cell (2,3). It is possible because grid\[2]\[3] <= 7.

The final time is 7. It can be shown that it is the minimum time possible.

Example 2:

![2577-2](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)

```text
Input: grid = [[0,2,4],[3,2,1],[1,0,4]]
Output: -1
Explanation: There is no path from the top left to the bottom-right cell.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] <= 10 ^ 5`
- `grid[0][0] == 0`

__题目描述:__

给你一个 `m x n` 的矩阵 `grid` ，每个元素都为 __非负__ 整数，其中 `grid[row][col]` 表示可以访问格子 `(row, col)` 的 __最早__ 时间。也就是说当你访问格子 `(row, col)` 时，最少已经经过的时间为 `grid[row][col]` 。

你从 __最左上角__ 出发，出发时刻为 `0` ，你必须一直移动到上下左右相邻四个格子中的 __任意__ 一个格子（即不能停留在格子上）。每次移动都需要花费 1 单位时间。

请你返回 __最早__ 到达右下角格子的时间，如果你无法到达右下角的格子，请你返回 `-1` 。

__示例:__

示例 1：

![2577-3](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-8.png)

```text
输入：grid = [[0,1,3,2],[5,1,2,5],[4,3,8,6]]
输出：7
解释：一条可行的路径为：
```

- 时刻 t = 0 ，我们在格子 (0,0) 。
- 时刻 t = 1 ，我们移动到格子 (0,1) ，可以移动的原因是 grid\[0]\[1] <= 1 。
- 时刻 t = 2 ，我们移动到格子 (1,1) ，可以移动的原因是 grid\[1]\[1] <= 2 。
- 时刻 t = 3 ，我们移动到格子 (1,2) ，可以移动的原因是 grid\[1]\[2] <= 3 。
- 时刻 t = 4 ，我们移动到格子 (1,1) ，可以移动的原因是 grid\[1]\[1] <= 4 。
- 时刻 t = 5 ，我们移动到格子 (1,2) ，可以移动的原因是 grid\[1]\[2] <= 5 。
- 时刻 t = 6 ，我们移动到格子 (1,3) ，可以移动的原因是 grid\[1]\[3] <= 6 。
- 时刻 t = 7 ，我们移动到格子 (2,3) ，可以移动的原因是 grid\[2]\[3] <= 7 。

最终到达时刻为 7 。这是最早可以到达的时间。

示例 2：

![2577-4](https://assets.leetcode.com/uploads/2023/02/14/yetgriddrawio-9.png)

```text
输入：grid = [[0,2,4],[3,2,1],[1,0,4]]
输出：-1
解释：没法从左上角按题目规定走到右下角。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] <= 10 ^ 5`
- `grid[0][0] == 0`

__思路:__

```text
如果可以走到 grid[0][1] 或 grid[1][0], 可以通过反复在这两个格子之间来回走动的方式等待
则一定可以走到 grid[-1][-1]
1. 二分
从最后一个格子出发反向检查是否能走到起点
至少要走 m + n - 2 步, 或者 grid[-1][-1] 步, 取较大的
最多走 max(grid) + m + n - 1 步
每次检查 4 个方向是否可以走动以及不能走回终点
由于每次需要满足时间要求, 每一轮还需要检查 grid[i][j] < t
最后如果能走到, 需要将结果的奇偶性调整为与 m + n 的奇偶性一致
时间复杂度为 O(MNlogC), 空间复杂度为 O(MN), 其中 C 为 grid 中的最大值
2. Dijkstra
设 dis[i][j] 为到达格子 (i, j) 的最早时间
dis[0][0] = 0
从 (0, 0) 开始, 每次将当前格子四个方向的格子加入优先队列
枚举 4 方向将最小的格子加入优先队列
如果走到 (m - 1, n - 1) 则返回当前时间
否则更新 dis[i][j] 的值
每次走到格子需要调整时间的奇偶性
时间复杂度为 O(MNlogMN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTime(vector<vector<int>> &grid) 
    {
        int m = grid.size(), n = grid.front().size(), dis[m][n], x = 0, y = 0, nd = 0, inf = 0x3f3f3f3f;
        if (grid[0][1] > 1 and grid[1][0] > 1) return -1;
        memset(dis, inf, sizeof(dis));
        dis[0][0] = 0;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        pq.emplace(0, 0, 0);
        while (true) 
        {
            auto [d, i, j] = pq.top();
            pq.pop();
            if (d > dis[i][j]) continue;
            if (i == m - 1 and j == n - 1) return d;
            for (const auto& q : DIRS) if (-1 < (x = i + q[0]) and x < m and -1 < (y = j + q[1]) and y < n and (nd = max(d + 1, grid[x][y]) + ((max(d + 1, grid[x][y]) - x - y) & 1)) < dis[x][y]) pq.emplace(dis[x][y] = nd, x, y);
        }
    }
private:
    static constexpr int DIRS[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
};
```

__Java__:

```Java
class Solution {
    private final static int[][] DIRS = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int minimumTime(int[][] grid) {
        int m = grid.length, n = grid[0].length, dis[][] = new int[m][n], d = 0, i = 0, j = 0, x = 0, y = 0, nd = 0;
        if (grid[0][1] > 1 && grid[1][0] > 1) return -1;
        for (i = 0; i < m; i++) Arrays.fill(dis[i], Integer.MAX_VALUE);
        dis[0][0] = 0;
        var pq = new PriorityQueue<int[]>((a, b) -> a[0] - b[0]);
        pq.add(new int[]{0, 0, 0});
        while (true) {
            var p = pq.poll();
            if ((d = p[0]) > dis[i = p[1]][j = p[2]]) continue;
            if (i == m - 1 && j == n - 1) return d;
            for (var q : DIRS) if (-1 < (x = i + q[0]) && x < m && -1 < (y = j + q[1]) && y < n && (nd = Math.max(d + 1, grid[x][y]) + ((Math.max(d + 1, grid[x][y]) - x - y) & 1)) < dis[x][y]) pq.add(new int[]{dis[x][y] = nd, x, y});
        }
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTime(self, grid: List[List[int]]) -> int:
        m, n, d = len(grid), len(grid[0]), [(0, 1), (0, -1), (1, 0), (-1, 0)]
        if grid[0][1] > 1 and grid[1][0] > 1:
            return -1
        visited, left, right = [[-inf] * n for _ in range(m)], max(grid[-1][-1], m + n - 2), max(map(max, grid)) + m + n - 1

        def check(end_time: int) -> bool:
            visited[-1][-1], q, t = end_time, [(m - 1, n - 1)], end_time
            while q:
                cur, q = q[:], []
                for x, y in cur:
                    for dx, dy in d:
                        if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and visited[nx][ny] != end_time and grid[nx][ny] < t:
                            if not nx and not ny:
                                return True
                            visited[nx][ny] = end_time
                            q.append((nx, ny))
                t -= 1
            return False
        while left <= right:
            if check(mid := left + ((right - left) >> 1)):
                right = mid - 1
            else:
                left = mid + 1
        return left + ((left - m - n) & 1)
```
