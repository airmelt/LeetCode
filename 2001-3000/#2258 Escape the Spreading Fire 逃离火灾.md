# 2258 Escape the Spreading Fire 逃离火灾

__Description:__

You are given a __0-indexed__ 2D integer array `grid` of size `m x n` which represents a field. Each cell has one of three values:

- `0` represents grass,
- `1` represents fire,
- `2` represents a wall that you and fire cannot pass through.

You are situated in the top-left cell, `(0, 0)`, and you want to travel to the safe house at the bottom-right cell, `(m - 1, n - 1)`. Every minute, you may move to an __adjacent__ grass cell. __After__ your move, every fire cell will spread to all __adjacent__ cells that are not walls.

Return _the __maximum__ number of minutes that you can stay in your initial position before moving while still safely reaching the safe house_. If this is impossible, return `-1`. If you can __always__ reach the safe house regardless of the minutes stayed, return `10 ^ 9`.

Note that even if the fire spreads to the safe house immediately after you have reached it, it will be counted as safely reaching the safe house.

A cell is adjacent to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

__Example:__

Example 1:

![2258-1](https://assets.leetcode.com/uploads/2022/03/10/ex1new.jpg)

```text
Input: grid = [[0,2,0,0,0,0,0],[0,0,0,2,2,1,0],[0,2,0,0,1,2,0],[0,0,2,2,2,0,2],[0,0,0,0,0,0,0]]
Output: 3
Explanation: The figure above shows the scenario where you stay in the initial position for 3 minutes.
You will still be able to safely reach the safe house.
Staying for more than 3 minutes will not allow you to safely reach the safe house.
```

Example 2:

![2258-2](https://assets.leetcode.com/uploads/2022/03/10/ex2new2.jpg)

```text
Input: grid = [[0,0,0,0],[0,1,2,0],[0,2,0,0]]
Output: -1
Explanation: The figure above shows the scenario where you immediately move towards the safe house.
Fire will spread to any cell you move towards and it is impossible to safely reach the safe house.
Thus, -1 is returned.
```

Example 3:

![2258-3](https://assets.leetcode.com/uploads/2022/03/10/ex3new.jpg)

```text
Input: grid = [[0,0,0],[2,2,0],[1,2,0]]
Output: 1000000000
Explanation: The figure above shows the initial grid.
Notice that the fire is contained by walls and you will always be able to safely reach the safe house.
Thus, 109 is returned.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 300`
- `4 <= m * n <= 2 * 10 ^ 4`
- `grid[i][j]` is either `0`, `1`, or `2`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`

__题目描述:__

给你一个下标从 __0__ 开始大小为 `m x n` 的二维整数数组 `grid` ，它表示一个网格图。每个格子为下面 3 个值之一:

- `0` 表示草地。
- `1` 表示着火的格子。
- `2` 表示一座墙，你跟火都不能通过这个格子。

一开始你在最左上角的格子 `(0, 0)` ，你想要到达最右下角的安全屋格子 `(m - 1, n - 1)` 。每一分钟，你可以移动到 __相邻__ 的草地格子。每次你移动 __之后__ ，着火的格子会扩散到所有不是墙的 __相邻__ 格子。

请你返回你在初始位置可以停留的 __最多__ 分钟数，且停留完这段时间后你还能安全到达安全屋。如果无法实现，请你返回 `-1` 。如果不管你在初始位置停留多久，你 __总是__ 能到达安全屋，请你返回 `10 ^ 9` 。

注意，如果你到达安全屋后，火马上到了安全屋，这视为你能够安全到达安全屋。

如果两个格子有共同边，那么它们为 相邻 格子。

__示例:__

示例 1：

![2258-4](https://assets.leetcode.com/uploads/2022/03/10/ex1new.jpg)

```text
输入：grid = [[0,2,0,0,0,0,0],[0,0,0,2,2,1,0],[0,2,0,0,1,2,0],[0,0,2,2,2,0,2],[0,0,0,0,0,0,0]]
输出：3
解释：上图展示了你在初始位置停留 3 分钟后的情形。
你仍然可以安全到达安全屋。
停留超过 3 分钟会让你无法安全到达安全屋。
```

示例 2：

![2258-5](https://assets.leetcode.com/uploads/2022/03/10/ex2new2.jpg)

```text
输入：grid = [[0,0,0,0],[0,1,2,0],[0,2,0,0]]
输出：-1
解释：上图展示了你马上开始朝安全屋移动的情形。
火会蔓延到你可以移动的所有格子，所以无法安全到达安全屋。
所以返回 -1 。
```

示例 3：

![2258-6](https://assets.leetcode.com/uploads/2022/03/10/ex3new.jpg)

```text
输入：grid = [[0,0,0],[2,2,0],[1,2,0]]
输出：1000000000
解释：上图展示了初始网格图。
注意，由于火被墙围了起来，所以无论如何你都能安全到达安全屋。
所以返回 109 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 300`
- `4 <= m * n <= 2 * 10 ^ 4`
- `grid[i][j]` 是 `0` ， `1` 或者 `2` 。
- `grid[0][0] == grid[m - 1][n - 1] == 0`

__思路:__

```text
BFS
先用 BFS 计算人和火到达每个位置的时间 human, fire
如果人到达的时间 human[-1][-1] 为 INF, 则返回 -1, 表示无法到达安全屋
如果火到达的时间 fire[-1][-1] 为 INF, 则返回 INF, 表示无论如何都可以到达安全屋
如果人比火后到达, 即 human[-1][-1] > fire[-1][-1], 则返回 -1, 表示人无法比火更快到达安全屋, 此时 last = fire[-1][-1] - human[-1][-1] < 0
否则, 由于安全屋在右下角, 可以比较人和火到达安全屋上方和左方的时间 human[-1][-2], human[-2][-1], fire[-1][-2], fire[-2][-1]
如果人到达的时间 human[-1][-2] 或 human[-2][-1] 小于火到达的时间 fire[-1][-2] 或 fire[-2][-1], 则返回 last, 表示人可以和火一起到达安全屋
否则, 返回 last - 1, 表示人必须比火更快到达安全屋
时间复杂度为 O(MN), 空间复杂度为 O(MN)
也可以使用二分搜索检查人是否能在火到达之前到达安全屋
时间复杂度为 O(MNlogMN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumMinutes(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), d[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, INF = 1e9, no_solution = -1, last = 0;
        auto bfs = [&](queue<pair<int, int>>& q) -> vector<int> 
        {
            int t = 0, s = q.size(), nx = 0, ny = 0;
            vector<vector<int>> grid_time(m, vector<int>(n, INF));
            for (t = 1; !q.empty(); t++, s = q.size()) 
            {
                while (s--) 
                {
                    int x = q.front().first, y = q.front().second;
                    q.pop();
                    grid_time[x][y] = t;
                    for (const auto& dir : d) if (-1 < (nx = x + dir[0]) and nx < m and -1 < (ny = y + dir[1]) and ny < n and grid_time[nx][ny] == INF and !grid[nx][ny]) q.push(make_pair(nx, ny));
                }
            }
            return vector<int>{grid_time.back().back(), grid_time.back()[n - 2], grid_time[m - 2].back()};
        };
        queue<pair<int, int>> human_queue, fire_queue;
        human_queue.push(make_pair(0, 0));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) fire_queue.push(make_pair(i, j));
        vector<int> human = bfs(human_queue), fire = bfs(fire_queue);
        return human.front() == INF ? no_solution : (fire.front() == INF ? INF : ((last = fire.front() - human.front()) < 0 ? no_solution : last - ((human[1] == INF or human[1] + last >= fire[1]) and (human[2] == INF or human[2] + last >= fire[2]))));
    }
};
```

__Java__:

```Java
class Solution {
    private static final int[][] D = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    private static final int INF = 1_000_000_000;

    public int maximumMinutes(int[][] grid) {
        int m = grid.length, n = grid[0].length, human[] = bfs(grid, new LinkedList(Arrays.asList(new int[]{0, 0}))), fire[] = new int[3], last = 0, noSolution = -1;
        if (human[0] == INF) return noSolution;
        Queue<int[]> fires = new LinkedList<>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 1) fires.offer(new int[]{i, j});
        fire = bfs(grid, fires);
        if (fire[0] == INF) return INF;
        return (last = fire[0] - human[0]) < 0 ? noSolution : ((human[1] != INF && human[1] + last < fire[1]) || (human[2] != INF && human[2] + last < fire[2]) ? last : last - 1);
    }

    private int[] bfs(int[][] grid, Queue<int[]> q) {
        int m = grid.length, n = grid[0].length, gridTime[][] = new int[m][n], t = 0, s = q.size(), nx = 0, ny = 0;
        for (int[] times : gridTime) Arrays.fill(times, INF);
        for (t = 1; !q.isEmpty(); t++, s = q.size()) {
            while (s-- > 0) {
                int pos[] = q.poll(), x = pos[0], y = pos[1];
                gridTime[x][y] = t;
                for (int[] dir : D) if (-1 < (nx = x + dir[0]) && nx < m && -1 < (ny = y + dir[1]) && ny < n && gridTime[nx][ny] == INF && grid[nx][ny] == 0) q.offer(new int[]{nx, ny});
            }
        }
        return new int[]{gridTime[m - 1][n - 1], gridTime[m - 1][n - 2], gridTime[m - 2][n - 1]};
    }
}
```

__Python__:

```Python
class Solution:
    def maximumMinutes(self, grid: List[List[int]]) -> int:
        m, n, inf, d = len(grid), len(grid[0]), 10 ** 9, [(0, 1), (0, -1), (1, 0), (-1, 0)]

        def bfs(q: deque(Tuple[int, int])) -> Tuple[int, int, int]:
            t, grid_time = 1, [[inf] * n for _ in range(m)]
            while q:
                s = len(q)
                for _ in range(s):
                    x, y = q.popleft()
                    grid_time[x][y] = t
                    for dx, dy in d:
                        if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and grid_time[nx][ny] == inf and not grid[nx][ny]:
                            q.append((nx, ny))
                t += 1
            return grid_time[-1][-1], grid_time[-1][-2], grid_time[-2][-1]
        (human_safe, human_left, human_up), (fire_safe, fire_left, fire_up) = bfs(deque([(0, 0)])), bfs(deque([(i, j) for i in range(m) for j in range(n) if grid[i][j] == 1]))
        return -1 if human_safe == inf or (last := fire_safe - human_safe) < 0 else inf if fire_safe == inf else last if (human_left != inf and human_left + last < fire_left) or (human_up != inf and human_up + last < fire_up) else last - 1
```
