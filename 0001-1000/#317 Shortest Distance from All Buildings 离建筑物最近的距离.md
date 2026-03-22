# 317 Shortest Distance from All Buildings 离建筑物最近的距离

__Description:__

You are given an `m x n` grid `grid` of values `0`, `1` or `2`, where:

- each `0` marks __an empty land__ that you can pass by freely,
- each `1` marks __a building__ that you cannot pass through, and
- each `2` marks __an obstacle__ that you cannot pass through.

You want to build a house on an empty land that reaches all buildings in the __shortest total travel__ distance. You can only move up, down, left, and right.

Return _the __shortest travel distance__ for such a house._ If it is not possible to build such a house according to the above rules, return `-1`.

The __total travel distance__ is the sum of the distances between the houses of the friends and the meeting point.

The distance is calculated using [Manhattan Distance](http://en.wikipedia.org/wiki/Taxicab_geometry), where `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

__Example:__

Example 1:

![317-1](https://assets.leetcode.com/uploads/2021/03/14/buildings-grid.jpg)

```text
Input: grid = [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
Output: 7
Explanation: Given three buildings at (0,0), (0,4), (2,2), and an obstacle at (0,2).
The point (1,2) is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal.
So return 7.
```

Example 2:

```text
Input: grid = [[1,0]]
Output: 1
```

Example 3:

```text
Input: grid = [[1]]
Output: -1
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0`, `1` or `2`.
- There will be __at least one__ building in the `grid`.

__题目描述:__

给你一个 `m × n` 的网格，值为 `0`、`1` 或 `2` ，其中:

- 每一个 `0` 代表一块你可以自由通过的 __空地__
- 每一个 `1` 代表一个你不能通过的 __建筑__
- 每个 `2` 标记一个你不能通过的 __障碍__

你想要在一块空地上建造一所房子，在 __最短的总旅行距离__ 内到达所有的建筑。你只能上下左右移动。

返回到该房子的 __最短旅行距离__ 。如果根据上述规则无法建造这样的房子，则返回 `-1` 。

__总行走距离__ 是朋友们家到碰头地点的距离之和。

使用 __曼哈顿距离__ 计算距离，其中距离 `(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|` 。

__示例:__

![317-2](https://assets.leetcode.com/uploads/2021/03/14/buildings-grid.jpg)

示例 1：

```text
输入：grid = [[1,0,2,0,1],[0,0,0,0,0],[0,0,1,0,0]]
输出：7 
解析：给定三个建筑物 (0,0)、(0,4) 和 (2,2) 以及一个位于 (0,2) 的障碍物。
由于总距离之和 3+3+1=7 最优，所以位置 (1,2) 是符合要求的最优地点。
故返回7。
```

示例 2：

```text
输入: grid = [[1,0]]
输出: 1
```

示例 3：

```text
输入: grid = [[1]]
输出: -1
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` 是 `0`, `1` 或 `2`.
- `grid` 中 __至少__ 有 __一幢__ 建筑

__思路:__

```text
BFS
从每一个建筑出发
将周围的空地都标记为 empty - 1
empty 每次遇到建筑物都减一
计算每一个空地到所有建筑物的距离和
时间复杂度为 O(M ^ 2N ^ 2), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int shortestDistance(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size(), directions[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}}, empty = 0, result = INT_MAX, nx = 0, ny = 0, total[51][51]{0};
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0, step = 0; j < n; j++, step = 0)
             {
                if (grid[i][j] == 1) 
                {
                    result = INT_MAX;
                    queue<pair<int, int>> q;
                    q.push({i, j});
                    while (!q.empty()) 
                    {
                        ++step;
                        for (int s = q.size(); s > 0; --s) 
                        {
                            auto cur = q.front();
                            q.pop();
                            for (const auto& direction : directions) 
                            {
                                if (-1 < (nx = cur.first + direction[0]) and nx < m and -1 < (ny = cur.second + direction[1]) and ny < n and grid[nx][ny] == empty) 
                                {
                                    --grid[nx][ny];
                                    total[nx][ny] += step;
                                    q.push({nx, ny});
                                    result = min(result, total[nx][ny]);
                                }
                            }
                        }
                    }
                    --empty;
                }
            }
        }
        return result == INT_MAX ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length, directions[][] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}}, total[][] = new int[m][n], empty = 0, result = Integer.MAX_VALUE, nx = 0, ny = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0, step = 0; j < n; j++, step = 0) {
                if (grid[i][j] == 1) {
                    result = Integer.MAX_VALUE;
                    Queue<int[]> queue = new LinkedList<>();
                    queue.offer(new int[]{i, j});
                    while (!queue.isEmpty()) {
                        ++step;
                        for (int s = queue.size(); s > 0; --s) {
                            int cur[] = queue.poll(), x = cur[0], y = cur[1];
                            for (int[] direction : directions) {
                                if (-1 < (nx = x + direction[0]) && nx < m && -1 < (ny = y + direction[1]) && ny < n && grid[nx][ny] == empty) {
                                    --grid[nx][ny];
                                    total[nx][ny] += step;
                                    queue.offer(new int[]{nx, ny});
                                    result = Math.min(result, total[nx][ny]);
                                }
                            }
                        }
                    }
                    --empty;
                }
            }
        }
        return result == Integer.MAX_VALUE ? -1 : result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestDistance(self, grid: List[List[int]]) -> int:
        d, m, n = [[1, 0], [-1, 0], [0, 1], [0, -1]], len(grid), len(grid[0])
        total, empty, result = [[0] * n for _ in range(m)], 0, inf
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    result, queue, step = inf, deque([(i, j)]), 0
                    while queue:
                        step += 1
                        for _ in range(len(queue)):
                            x, y = queue.popleft()
                            for dx, dy in d:
                                if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and grid[nx][ny] == empty:
                                    grid[nx][ny] -= 1
                                    total[nx][ny] += step
                                    queue.append((nx, ny))
                                    result = min(result, total[nx][ny])
                    empty -= 1
        return result if result < inf else -1
```
