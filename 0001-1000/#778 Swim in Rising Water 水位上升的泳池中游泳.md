# 778 Swim in Rising Water 水位上升的泳池中游泳

__Description__:
You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point (i, j).

The rain starts to fall. At time t, the depth of the water everywhere is t. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the least time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).

__Example:__

Example 1:

![swim grid 1](https://assets.leetcode.com/uploads/2021/06/29/swim1-grid.jpg)

Input: grid = [[0,2],[1,3]]
Output: 3
Explanation:
At time 0, you are in grid location (0, 0).
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.
You cannot reach point (1, 1) until time 3.
When the depth of water is 3, we can swim anywhere inside the grid.

Example 2:

![swim grid 2](https://assets.leetcode.com/uploads/2021/06/29/swim2-grid-1.jpg)

Input: grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
Output: 16
Explanation: The final route is shown.
We need to wait until time 16 so that (0, 0) and (4, 4) are connected.

__Constraints:__

n == grid.length
n == grid[i].length
1 <= n <= 50
0 <= grid[i][j] < n^2
Each value grid[i][j] is unique.

__题目描述__:
在一个 N x N 的坐标方格 grid 中，每一个方格的值 grid[i][j] 表示在位置 (i,j) 的平台高度。

现在开始下雨了。当时间为 t 时，此时雨水导致水池中任意位置的水位为 t 。你可以从一个平台游向四周相邻的任意一个平台，但是前提是此时水位必须同时淹没这两个平台。假定你可以瞬间移动无限距离，也就是默认在方格内部游动是不耗时的。当然，在你游泳的时候你必须待在坐标方格里面。

你从坐标方格的左上平台 (0，0) 出发。最少耗时多久你才能到达坐标方格的右下平台 (N-1, N-1)？

__示例 :__

示例 1:

输入: [[0,2],[1,3]]
输出: 3
解释:
时间为0时，你位于坐标方格的位置为 (0, 0)。
此时你不能游向任意方向，因为四个相邻方向平台的高度都大于当前时间为 0 时的水位。

等时间到达 3 时，你才可以游向平台 (1, 1). 因为此时的水位是 3，坐标方格中的平台没有比水位 3 更高的，所以你可以游向坐标方格中的任意位置

示例2:

输入: [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
输出: 16
解释:

```text
 0  1  2  3  4
24 23 22 21  5
12 13 14 15 16
11 17 18 19 20
10  9  8  7  6
```

最终的路线用加粗进行了标记。
我们必须等到时间为 16，此时才能保证平台 (0, 0) 和 (4, 4) 是连通的

__提示:__

2 <= N <= 50.
grid[i][j] 是 [0, ..., N*N - 1] 的排列。

__思路__:

1. 优先队列
在搜索的同时将周围的水位加入优先队列
每次选择最低的水位移动
2. 并查集
将两个相邻的方块看作两个结点, 权为两者水位较高值
时间复杂度为 O(n ^ 2lgn), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class UnionSet
{
private:
    int* parent;
    int n;
public:
    UnionSet(int n) : n(n)
    {
        parent = new int[n];
        for (int i = 0; i < n; ++i)
        {
            parent[i] = i;
        }
    }

    int Find(int x)
    {
        if (parent[x] != x)
        {
            parent[x] = Find(parent[x]);
        }
        return parent[x];
    }

    bool Merge(int x, int y)
    {
        x = Find(x);
        y = Find(y);
        if (x == y)
        {
            return false;
        }
        parent[x] = y;
        return true;
    }

    bool Connected(int x, int y)
    {
        return Find(x) == Find(y);
    }
};

class Solution 
{
public:
    int swimInWater(vector<vector<int>>& grid) 
    {
        int N = grid.size();
        int n = N * N;
        int* index = new int[n];
        
        for (int i = 0; i < N; i++) for (int j = 0; j < N; j++) index[grid[i][j]] = i * N + j;
        UnionSet us(n);
        int dires[5] = {1, 0, -1, 0, 1};
        for (int i = 0; i < n; ++i)
        {
            int x = index[i] / N, y = index[i] % N;
            for (int j = 0; j < 4; ++j)
            {
                int nx = x + dires[j], ny = y + dires[j + 1];
                if (nx >= 0 and nx < N and ny >= 0 and ny < N and grid[nx][ny] <= i) 
if (us.Merge(nx * N + ny, index[i])) if (us.Connected(n - 1, 0)) return i;
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int swimInWater(int[][] grid) {
        int m = grid.length, n = grid[0].length, result = 0, d[][] = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
        boolean[][] visited = new boolean[m][n];
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (grid[a[0]][a[1]] - grid[b[0]][b[1]]));
        pq.offer(new int[]{0, 0});
        while (!pq.isEmpty()) {
            int[] pos = pq.poll();
            result = Math.max(result, grid[pos[0]][pos[1]]);
            if (pos[0] == m - 1 && pos[1] == n - 1) break;
            for (int[] dir : d) {
                int x = pos[0] + dir[0], y = pos[1] + dir[1];
                if (x < 0 || y < 0 || x > m - 1 || y > n - 1 || visited[x][y]) continue;
                pq.offer(new int[]{x, y});
                visited[x][y] = true;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        d, m, n, result, q = [(0, 1), (0, -1), (1, 0), (-1, 0)], len(grid), len(grid[0]), 0, [[grid[0][0], 0, 0]]
        visited = [[False] * n for _ in range(m)]
        while q:
            h, x, y = heapq.heappop(q)
            result = max(result, h)
            if x == m - 1 and y == n - 1:
                break
            for dx, dy in d:
                next_x, next_y = x + dx, y + dy
                if -1 < next_x < m and -1 < next_y < n and not visited[next_x][next_y]:
                    visited[next_x][next_y] = True
                    heapq.heappush(q, [grid[next_x][next_y], next_x, next_y])
        return result
```
