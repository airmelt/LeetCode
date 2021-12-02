# 934 Shortest Bridge 最短的桥

__Description__:
You are given an n x n binary matrix grid where 1 represents land and 0 represents water.

An island is a 4-directionally connected group of 1's not connected to any other 1's. There are exactly two islands in grid.

You may change 0's to 1's to connect the two islands to form one island.

Return the smallest number of 0's you must flip to connect the two islands.

__Example:__

Example 1:

Input: grid = [[0,1],[1,0]]
Output: 1

Example 2:

Input: grid = [[0,1,0],[0,0,0],[0,0,1]]
Output: 2

Example 3:

Input: grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
Output: 1

__Constraints:__

n == grid.length == grid[i].length
2 <= n <= 100
grid[i][j] is either 0 or 1.
There are exactly two islands in grid.

__题目描述__:
在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。）

现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。

返回必须翻转的 0 的最小数目。（可以保证答案至少是 1 。）

__示例 :__

示例 1：

输入：A = [[0,1],[1,0]]
输出：1

示例 2：

输入：A = [[0,1,0],[0,0,0],[0,0,1]]
输出：2

示例 3：

输入：A = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
输出：1

__提示:__

2 <= A.length == A[0].length <= 100
A[i][j] == 0 或 A[i][j] == 1

__思路__:

DFS + BFS
先用 DFS 的方式找到第一座岛, 并将第一座岛加入队列
然后用 BFS 的方式搜索第一座岛到第二座岛的最短距离
可以将原岛屿值改成 2 标记访问过的位置
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int shortestBridge(vector<vector<int>>& grid) 
    {
        queue<int> q;
        int flag = 0, n = grid.size(), result = -1; 
        const vector<vector<int>> d{ { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };
        for (int i = 0; i < n; i++) 
        {
            if (flag) break;
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j]) 
                {
                    dfs(grid, n, i, j, q, d);
                    flag = true;
                    break;
                }
            }
        }
        while (!q.empty()) 
        {
            int s = q.size();
            ++result;
            for (int i = 0; i < s; i++) 
            {
                int cur = q.front();
                q.pop();
                for (const auto& next : d) 
                {
                    int x = next[0] + cur / n, y = next[1] + cur % n;
                    if (x < 0 or x >= n or y < 0 or y >= n or grid[x][y] == 2) continue;
                    if (grid[x][y] == 1) return result;
                    q.push(x * n + y);
                    grid[x][y] = 2;
                }
            }
        }
        return result;
    }
private:
    void dfs(vector<vector<int>>& grid, int n, int i, int j, queue<int>& q, const vector<vector<int>>& d) 
    {
        if (i < 0 or i >= n or j < 0 or j >= n or grid[i][j] != 1) return;
        grid[i][j] = 2;
        q.push(i * n + j);
        for (const auto& next : d) dfs(grid, n, i + next[0], j + next[1], q, d);
    }
};
```

__Java__:

```Java
class Solution {
    public int shortestBridge(int[][] grid) {
        Queue<Integer> queue = new LinkedList<>();
        boolean flag = false;
        int n = grid.length, result = -1, d[][] = new int[][]{ { 0, 1 }, { 0, -1 }, { 1, 0 }, { -1, 0 } };
        for (int i = 0; i < n; i++) {
            if (flag) break;
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    dfs(grid, n, i, j, queue, d);
                    flag = true;
                    break;
                }
            }
        }
        while (!queue.isEmpty()) {
            int s = queue.size();
            ++result;
            for (int i = 0; i < s; i++) {
                int cur = queue.poll();
                for (int[] next : d) {
                    int x = next[0] + cur / n, y = next[1] + cur % n;
                    if (x < 0 || x >= n || y < 0 || y >= n || grid[x][y] == 2) continue;
                    if (grid[x][y] == 1) return result;
                    queue.offer(x * n + y);
                    grid[x][y] = 2;
                }
            }
        }
        return result;
    }
    
    private void dfs(int[][] grid, int n, int i, int j, Queue<Integer> queue, int[][] d) {
        if (i < 0 || i >= n || j < 0 || j >= n || grid[i][j] != 1) return;
        grid[i][j] = 2;
        queue.offer(i * n + j);
        for (int[] next : d) dfs(grid, n, i + next[0], j + next[1], queue, d);
    }
}
```

__Python__:

```Python
class Solution:
    def shortestBridge(self, grid: List[List[int]]) -> int:
        n, queue, flag, d, result = len(grid), deque(), False, [(-1, 0), (1, 0), (0, -1), (0, 1)], -1
        
        def dfs(i: int, j: int) -> None:
            if not -1 < i < n or not -1 < j < n or not grid[i][j] or grid[i][j] == 2:
                return
            grid[i][j] = 2
            queue.append((i, j))
            for dx, dy in d:
                dfs(i + dx, j + dy)
                
        for i in range(n):
            if flag:
                break
            for j in range(n):
                if grid[i][j]:
                    dfs(i, j)
                    flag = True
                    break
        while queue:
            s = len(queue)
            result += 1
            for i in range(s):
                cur = queue.popleft()
                for dx, dy in d:
                    x, y = cur[0] + dx, cur[1] + dy
                    if not -1 < x < n or not -1 < y < n or grid[x][y] == 2:
                        continue
                    if grid[x][y] == 1:
                        return result
                    queue.append((x, y))
                    grid[x][y] = 2
        return result
```
