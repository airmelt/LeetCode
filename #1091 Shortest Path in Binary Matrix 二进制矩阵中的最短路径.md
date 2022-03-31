# 1091 Shortest Path in Binary Matrix 二进制矩阵中的最短路径

__Description__:
Given an n x n binary matrix grid, return the length of the shortest clear path in the matrix. If there is no clear path, return -1.

A clear path in a binary matrix is a path from the top-left cell (i.e., (0, 0)) to the bottom-right cell (i.e., (n - 1, n - 1)) such that:

All the visited cells of the path are 0.
All the adjacent cells of the path are 8-directionally connected (i.e., they are different and they share an edge or a corner).
The length of a clear path is the number of visited cells of this path.

__Example:__

Example 1:

![Binary Matrix 1](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

Input: grid = [[0,1],[1,0]]
Output: 2

Example 2:

![Binary Matrix 2](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

Input: grid = [[0,0,0],[1,1,0],[1,1,0]]
Output: 4

Example 3:

Input: grid = [[1,0,0],[1,1,0],[1,1,0]]
Output: -1

__Constraints:__

n == grid.length
n == grid[i].length
1 <= n <= 100
grid[i][j] is 0 or 1

__题目描述__:
给你一个 n x n 的二进制矩阵 grid 中，返回矩阵中最短 畅通路径 的长度。如果不存在这样的路径，返回 -1 。

二进制矩阵中的 畅通路径 是一条从 左上角 单元格（即，(0, 0)）到 右下角 单元格（即，(n - 1, n - 1)）的路径，该路径同时满足下述要求：

路径途经的所有单元格都的值都是 0 。
路径中所有相邻的单元格应当在 8 个方向之一 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。
畅通路径的长度 是该路径途经的单元格总数。

__示例 :__

示例 1：

![二进制矩阵 1](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

输入：grid = [[0,1],[1,0]]
输出：2

示例 2：

![二进制矩阵 2](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

输入：grid = [[0,0,0],[1,1,0],[1,1,0]]
输出：4

示例 3：

输入：grid = [[1,0,0],[1,1,0],[1,1,0]]
输出：-1

__提示:__

n == grid.length
n == grid[i].length
1 <= n <= 100
grid[i][j] 为 0 或 1

__思路__:

BFS
先判断边界条件, 起点终点不能为 1
将起点加入队列, 按 8 个方向搜索
如果 grid 可以修改, 可以将 grid 对应坐标的值修改以记录访问过的
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) 
    {
        int n = grid.size(), l = 1, s = 0, r = 0, c = 0;
        vector<int> dq(n * n);
        if (grid.back().back() or grid.front().front()) return -1;
        grid.back().back() = 1;
        dq.front() = (n - 1) * 257;
        while (l > s) 
        {
            int row = dq[s] >> 8, col = dq[s++] & 0xff, step = grid[row][col] + 1;
            for (int i = -1; i <= 1; i++) 
            {
                if ((r = row + i) < 0 or r >= n) continue;
                for (int j = -1; j <= 1; j++) 
                {
                    if ((c = col + j) < 0 or c >= n or grid[r][c]) continue;
                    if (!r and !c) return step;
                    grid[r][c] = step;
                    dq[l++] = (r << 8) | c;
                }
            }
        }
        return n == 1 ? 1 : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        int n = grid.length, l = 1, s = 0, r = 0, c = 0, dq[] = new int[n * n];
        if (grid[n - 1][n - 1] != 0 || grid[0][0] != 0) return -1;
        grid[n - 1][n - 1] = 1;
        dq[0] = (n - 1) * 257;
        while (l > s) {
            int row = dq[s] >> 8, col = dq[s++] & 0xff, step = grid[row][col] + 1;
            for (int i = -1; i <= 1; i++) {
                if ((r = row + i) < 0 || r >= n) continue;
                for (int j = -1; j <= 1; j++) {
                    if ((c = col + j) < 0 || c >= n || grid[r][c] != 0) continue;
                    if (r == 0 && c == 0) return step;
                    grid[r][c] = step;
                    dq[l++] = (r << 8) | c;
                }
            }
        }
        return n == 1 ? 1 : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        if grid[0][0]:
            return -1
        n, queue, visited, result, d = len(grid), deque([(0, 0)]), {(0, 0)}, 0, [(-1, -1), (-1, 0), (-1, 1), (0, 1), (1, 1), (1, 0), (1, -1), (0, -1)]
        while queue:
            result += 1
            for _ in range(len(queue)):
                x, y = queue.pop()
                for dx, dy in d:
                    if -1 < (i := x + dx) < n and -1 < (j := y + dy) < n and not grid[i][j] and not (i, j) in visited:
                        if (pos := (i, j)) == (n - 1, n - 1):
                            return result + 1
                        queue.appendleft(pos)
                        visited.add(pos)
        return 1 if len(grid) == 1 else -1
```
