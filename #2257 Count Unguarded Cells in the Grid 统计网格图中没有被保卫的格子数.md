# 2257 Count Unguarded Cells in the Grid 统计网格图中没有被保卫的格子数

__Description:__

You are given two integers `m` and `n` representing a __0-indexed__ `m x n` grid. You are also given two 2D integer arrays `guards` and `walls` where `guards[i] = [row_i, col_i]` and `walls[j] = [row_j, col_j]` represent the positions of the `i ^ th` guard and `j ^ th` wall respectively.

A guard can see every cell in the four cardinal directions (north, east, south, or west) starting from their position unless obstructed by a wall or another guard. A cell is guarded if there is at least one guard that can see it.

Return the number of unoccupied cells that are not guarded.

__Example:__

Example 1:

![2257-1](https://assets.leetcode.com/uploads/2022/03/10/example1drawio2.png)

```text
Input: m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]
Output: 7
Explanation: The guarded and unguarded cells are shown in red and green respectively in the above diagram.
There are a total of 7 unguarded cells, so we return 7.
```

Example 2:

![2257-2](https://assets.leetcode.com/uploads/2022/03/10/example2drawio.png)

```text
Input: m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]
Output: 4
Explanation: The unguarded cells are shown in green in the above diagram.
There are a total of 4 unguarded cells, so we return 4.
```

__Constraints:__

- `1 <= m, n <= 10 ^ 5`
- `2 <= m * n <= 10 ^ 5`
- `1 <= guards.length, walls.length <= 5 * 10 ^ 4`
- `2 <= guards.length + walls.length <= m * n`
- `guards[i].length == walls[j].length == 2`
- `0 <= row_i, row_j < m`
- `0 <= col_i, col_j < n`
- All the positions in `guards` and `walls` are __unique__.

__题目描述:__

给你两个整数 `m` 和 `n` 表示一个下标从 __0__ 开始的 `m x n` 网格图。同时给你两个二维整数数组 `guards` 和 `walls` ，其中 `guards[i] = [row_i, col_i]` 且 `walls[j] = [row_j, col_j]` ，分别表示第 `i` 个警卫和第 `j` 座墙所在的位置。

一个警卫能看到 4 个坐标轴方向（即东、南、西、北）的 所有 格子，除非他们被一座墙或者另外一个警卫 挡住 了视线。如果一个格子能被 至少 一个警卫看到，那么我们说这个格子被 保卫 了。

请你返回空格子中，有多少个格子是 没被保卫 的。

__示例:__

示例 1：

![2257-3](https://assets.leetcode.com/uploads/2022/03/10/example1drawio2.png)

```text
输入：m = 4, n = 6, guards = [[0,0],[1,1],[2,3]], walls = [[0,1],[2,2],[1,4]]
输出：7
解释：上图中，被保卫和没有被保卫的格子分别用红色和绿色表示。
总共有 7 个没有被保卫的格子，所以我们返回 7 。
```

示例 2：

![2257-4](https://assets.leetcode.com/uploads/2022/03/10/example2drawio.png)

```text
输入：m = 3, n = 3, guards = [[1,1]], walls = [[0,1],[1,0],[2,1],[1,2]]
输出：4
解释：上图中，没有被保卫的格子用绿色表示。
总共有 4 个没有被保卫的格子，所以我们返回 4 。
```

__提示：__

- `1 <= m, n <= 10 ^ 5`
- `2 <= m * n <= 10 ^ 5`
- `1 <= guards.length, walls.length <= 5 * 10 ^ 4`
- `2 <= guards.length + walls.length <= m * n`
- `guards[i].length == walls[j].length == 2`
- `0 <= row_i, row_j < m`
- `0 <= col_i, col_j < n`
- `guards` 和 `walls` 中所有位置 __互不相同__ 。

__思路:__

```text
DFS
将所有的位置标记为 0, 守卫标记为 -1, 墙标记为 -2
从每一个守卫开始, 沿着四个方向进行 DFS, 将所有可以到达的位置标记为 1
直到碰到墙或者其他守卫
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countUnguarded(int m, int n, vector<vector<int>>& guards, vector<vector<int>>& walls) 
    {
        vector<vector<int>> d{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}, grid(m, vector<int>(n));
        int result = 0;
        for (const auto& guard : guards) grid[guard.front()][guard.back()] = -1;
        for (const auto& wall : walls) grid[wall.front()][wall.back()] = -2;
        for (const auto& guard : guards) 
        {
            int x = guard.front(), y = guard.back();
            for (const auto& direction : d) 
            {
                int dx = direction.front(), dy = direction.back(), nx = x + dx, ny = y + dy;
                while (-1 < nx and nx < m and -1 < ny and ny < n and grid[nx][ny] > -1) 
                {
                    grid[nx][ny] = 1;
                    nx += dx;
                    ny += dy;
                }
            }
        }
        for (const auto& row : grid) for (const auto& v : row) result += !v;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countUnguarded(int m, int n, int[][] guards, int[][] walls) {
        int d[][] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}}, grid[][] = new int[m][n], result = 0;
        for (int[] guard : guards) grid[guard[0]][guard[1]] = -1;
        for (int[] wall : walls) grid[wall[0]][wall[1]] = -2;
        for (int[] guard : guards) {
            int x = guard[0], y = guard[1];
            for (int[] direction : d) {
                int dx = direction[0], dy = direction[1], nx = x + dx, ny = y + dy;
                while (-1 < nx && nx < m && -1 < ny && ny < n && grid[nx][ny] > -1) {
                    grid[nx][ny] = 1;
                    nx += dx;
                    ny += dy;
                }
            }
        }
        for (int[] row : grid) for (int v : row) if (v == 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countUnguarded(self, m: int, n: int, guards: List[List[int]], walls: List[List[int]]) -> int:
        d, grid = [(1, 0), (-1, 0), (0, 1), (0, -1)], [[0] * n for _ in range(m)]
        for x, y in guards:
            grid[x][y] = -1
        for x, y in walls:
            grid[x][y] = -2
        for x, y in guards:
            for dx, dy in d:
                nx, ny = x + dx, y + dy
                while -1 < nx < m and -1 < ny < n and grid[nx][ny] > -1:
                    grid[nx][ny] = 1
                    nx += dx
                    ny += dy
        return sum(not v for row in grid for v in row)
```
