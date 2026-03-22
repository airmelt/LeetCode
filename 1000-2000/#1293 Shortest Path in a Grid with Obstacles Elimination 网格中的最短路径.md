# 1293 Shortest Path in a Grid with Obstacles Elimination 网格中的最短路径

__Description:__

You are given an m x n integer matrix grid where each cell is either 0 (empty) or 1 (obstacle). You can move up, down, left, or right from and to an empty cell in one step.

Return the minimum number of steps to walk from the upper left corner (0, 0) to the lower right corner (m - 1, n - 1) given that you can eliminate at most k obstacles. If it is not possible to find such walk return -1.

__Example:__

Example 1:

![grid 1](https://assets.leetcode.com/uploads/2021/09/30/short1-grid.jpg)

Input: grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
Output: 6
Explanation:
The shortest path without eliminating any obstacle is 10.
The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).

Example 2:

![grid 2](https://assets.leetcode.com/uploads/2021/09/30/short2-grid.jpg)

Input: grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
Output: -1
Explanation: We need to eliminate at least two obstacles to find such a walk.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 40
1 <= k <= m * n
grid\[i][j] is either 0 or 1.
grid\[0][0] == grid\[m - 1][n - 1] == 0

__题目描述：__

给你一个 m * n 的网格，其中每个单元格不是 0（空）就是 1（障碍物）。每一步，您都可以在空白单元格中上、下、左、右移动。

如果您 最多 可以消除 k 个障碍物，请找出从左上角 (0, 0) 到右下角 (m-1, n-1) 的最短路径，并返回通过该路径所需的步数。如果找不到这样的路径，则返回 -1 。

__示例：__

示例 1：

![网格 1](https://assets.leetcode.com/uploads/2021/09/30/short1-grid.jpg)

输入： grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
输出：6
解释：
不消除任何障碍的最短路径是 10。
消除位置 (3,2) 处的障碍后，最短路径是 6 。该路径是 (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).

示例 2：

![网格 2](https://assets.leetcode.com/uploads/2021/09/30/short2-grid.jpg)

输入：grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
输出：-1
解释：我们至少需要消除两个障碍才能找到这样的路径。

__提示：__

grid.length == m
grid[0].length == n
1 <= m, n <= 40
1 <= k <= m*n
grid\[i][j] 是 0 或 1
grid\[0][0] == grid\[m-1][n-1] == 0

__思路：__

BFS
记录到达过的节点及消除障碍的次数, 同一地点及消除次数只需要访问一次
时间复杂度为 O(mnk), 空间复杂度为 O(mnk)

__代码__:

__C++__:

```C++
struct State 
{
    int x, y, r;
    State(int xx, int yy, int rr) 
    {
        x = xx;
        y = yy;
        r = rr;
    }
};

class Solution 
{
private:
    int dx[4]{1, 0, -1, 0}, dy[4]{0, 1, 0, -1};
public:
    int shortestPath(vector<vector<int>>& grid, int k) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        if (k > m + n - 4) return m + n - 2;
        vector<vector<vector<bool>>> visited(m, vector<vector<bool>>(n, vector<bool>(k + 1)));
        queue<State> q;
        q.emplace(0, 0, k);
        while(!q.empty()) 
        {
            int s = q.size();
            while (s--) 
            {
                auto state = q.front();
                q.pop();
                int x = state.x, y = state.y, r = state.r;
                if (x == m - 1 and y == n - 1 and r > -1) return result;
                if (visited[x][y][r] == 1) continue;
                visited[x][y][r] = 1;
                for (int d = 0; d < 4; d++) 
                {
                    if (x + dx[d] > -1 and x + dx[d] < m and y + dy[d] > -1 and y + dy[d] < n) 
                    {
                        if (grid[x + dx[d]][y + dy[d]] and r) q.emplace(x + dx[d], y + dy[d], r - 1);
                        else if (!grid[x + dx[d]][y + dy[d]]) q.emplace(x + dx[d], y + dy[d], r);
                    }
                }
            }
            ++result;
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    class State {
        int x, y, r;
        
        State(int x, int y, int r) {
            this.x = x;
            this.y = y;
            this.r = r;
        }
    }
    
    private int dx[] = new int[]{1, 0, -1, 0}, dy[] = new int[]{0, 1, 0, -1};
    
    public int shortestPath(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length, result = 0, visited[][][] = new int[m][n][k + 1];
        if (k > m + n - 4) return m + n - 2;
        Queue<State> queue = new LinkedList<>();
        queue.offer(new State(0, 0, k));
        while(!queue.isEmpty()) 
        {
            int s = queue.size();
            while (s-- > 0) 
            {
                State state = queue.poll();
                int x = state.x, y = state.y, r = state.r;
                if (x == m - 1 && y == n - 1 && r > -1) return result;
                if (visited[x][y][r] == 1) continue;
                visited[x][y][r] = 1;
                for (int d = 0; d < 4; d++) 
                {
                    if (x + dx[d] > -1 && x + dx[d] < m && y + dy[d] > -1 && y + dy[d] < n) 
                    {
                        if (grid[x + dx[d]][y + dy[d]] == 1 && r > 0) queue.offer(new State(x + dx[d], y + dy[d], r - 1));
                        else if (grid[x + dx[d]][y + dy[d]] == 0) queue.offer(new State(x + dx[d], y + dy[d], r));
                    }
                }
            }
            ++result;
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        if k > (m := len(grid)) + (n := len(grid[0])) - 4:
            return m + n - 2
        visited, d, queue, result = defaultdict(bool), [(1, 0), (-1, 0), (0, 1), (0, -1)], deque([(0, 0, k)]), 0
        while queue:
            s = len(queue)
            while s:
                s -= 1
                x, y, r = queue.popleft()
                if x == m - 1 and y == n - 1 and r > -1:
                    return result
                if visited[(x, y, r)]:
                    continue
                visited[(x, y, r)] = True
                for dx, dy in d:
                    -1 < x + dx < m and - 1 < y + dy < n and ((grid[x + dx][y + dy] and r and queue.append((x + dx, y + dy, r - 1))) or (not grid[x + dx][y + dy] and queue.append((x + dx, y + dy, r))))
            result += 1
        return -1
```
