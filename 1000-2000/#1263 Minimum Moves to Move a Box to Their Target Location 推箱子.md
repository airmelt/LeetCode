# 1263 Minimum Moves to Move a Box to Their Target Location 推箱子

__Description:__

A storekeeper is a game in which the player pushes boxes around in a warehouse trying to get them to target locations.

The game is represented by an m x n grid of characters grid where each element is a wall, floor, or box.

Your task is to move the box 'B' to the target position 'T' under the following rules:

The character 'S' represents the player. The player can move up, down, left, right in grid if it is a floor (empty cell).
The character '.' represents the floor which means a free cell to walk.
The character '#' represents the wall which means an obstacle (impossible to walk there).
There is only one box 'B' and one target cell 'T' in the grid.
The box can be moved to an adjacent free cell by standing next to the box and then moving in the direction of the box. This is a push.
The player cannot walk through the box.
Return the minimum number of pushes to move the box to the target. If there is no way to reach the target, return -1.

__Example:__

Example 1:

![Box](https://assets.leetcode.com/uploads/2019/11/06/sample_1_1620.png)

Input: grid = [["#","#","#","#","#","#"],
               ["#","T","#","#","#","#"],
               ["#",".",".","B",".","#"],
               ["#",".","#","#",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: 3
Explanation: We return only the number of times the box is pushed.

Example 2:

Input: grid = [["#","#","#","#","#","#"],
               ["#","T","#","#","#","#"],
               ["#",".",".","B",".","#"],
               ["#","#","#","#",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: -1

Example 3:

Input: grid = [["#","#","#","#","#","#"],
               ["#","T",".",".","#","#"],
               ["#",".","#","B",".","#"],
               ["#",".",".",".",".","#"],
               ["#",".",".",".","S","#"],
               ["#","#","#","#","#","#"]]
Output: 5
Explanation: push the box down, left, left, up and up.

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 20
grid contains only characters '.', '#', 'S', 'T', or 'B'.
There is only one character 'S', 'B', and 'T' in the grid.

__题目描述：__

「推箱子」是一款风靡全球的益智小游戏，玩家需要将箱子推到仓库中的目标位置。

游戏地图用大小为 m x n 的网格 grid 表示，其中每个元素可以是墙、地板或者是箱子。

现在你将作为玩家参与游戏，按规则将箱子 'B' 移动到目标位置 'T' ：

玩家用字符 'S' 表示，只要他在地板上，就可以在网格中向上、下、左、右四个方向移动。
地板用字符 '.' 表示，意味着可以自由行走。
墙用字符 '#' 表示，意味着障碍物，不能通行。
箱子仅有一个，用字符 'B' 表示。相应地，网格上有一个目标位置 'T'。
玩家需要站在箱子旁边，然后沿着箱子的方向进行移动，此时箱子会被移动到相邻的地板单元格。记作一次「推动」。
玩家无法越过箱子。
返回将箱子推到目标位置的最小 推动 次数，如果无法做到，请返回 -1。

__示例：__

示例 1：

![推箱子](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/sample_1_1620.png)

输入：grid = [["#","#","#","#","#","#"],
             ["#","T","#","#","#","#"],
             ["#",".",".","B",".","#"],
             ["#",".","#","#",".","#"],
             ["#",".",".",".","S","#"],
             ["#","#","#","#","#","#"]]
输出：3
解释：我们只需要返回推箱子的次数。

示例 2：

输入：grid = [["#","#","#","#","#","#"],
             ["#","T","#","#","#","#"],
             ["#",".",".","B",".","#"],
             ["#","#","#","#",".","#"],
             ["#",".",".",".","S","#"],
             ["#","#","#","#","#","#"]]
输出：-1

示例 3：

输入：grid = [["#","#","#","#","#","#"],
             ["#","T",".",".","#","#"],
             ["#",".","#","B",".","#"],
             ["#",".",".",".",".","#"],
             ["#",".",".",".","S","#"],
             ["#","#","#","#","#","#"]]
输出：5
解释：向下、向左、向左、向上再向上。

__提示：__

m == grid.length
n == grid[i].length
1 <= m, n <= 20
grid 仅包含字符 '.', '#',  'S' , 'T', 以及 'B'。
grid 中 'S', 'B' 和 'T' 各只能出现一个。

__思路：__

BFS
两次 BFS
先找出箱子的下一个位置
然后判断人是否能合理的到达推箱子的位置
时间复杂度为 O(m ^ 2n ^ 2), 空间复杂度为 O(m ^ 2n ^ 2)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minPushBox(vector<vector<char>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<vector<vector<bool>>>> visited(m, vector<vector<vector<bool>>>(n, vector<vector<bool>>(m, vector<bool>(n))));
        Node start{};
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] == 'S') 
                {
                    start.px = i;
                    start.py = j;
                } 
                else if (grid[i][j] == 'B') 
                {
                    start.bx = i;
                    start.by = j;
                }
            }
        }
        visited[start.bx][start.by][start.px][start.py] = true;
        queue<Node> q({start});
        while (!q.empty()) 
        {
            Node cur = q.front();
            q.pop();
            if (grid[cur.bx][cur.by] == 'T') return cur.result;
            Node next{};
            for (const auto& dir : dirs) 
            {
                next.bx = cur.bx + dir.front();
                next.by = cur.by + dir.back();
                next.px = cur.bx - dir.front();
                next.py = cur.by - dir.back();
                if (invalid(next.bx, next.by, grid, m, n) or invalid(next.px, next.py, grid, m, n) or visited[next.bx][next.by][next.px][next.py]) continue;
                if (check(grid, cur, next, m, n)) 
                {
                    next.result = cur.result + 1;
                    visited[next.bx][next.by][next.px][next.py] = true;
                    q.push(next);
                }
            }
        }
        return -1;
    }

private:
    struct Node 
    {
        int result, bx, by, px, py;
    };
    
    vector<vector<int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    bool invalid(int x, int y, vector<vector<char>>& grid, int m, int n) const 
    {
        return x < 0 or x >= m or y < 0 or y >= n or grid[x][y] == '#';
    }

    bool check(vector<vector<char>>& grid, Node& box, Node& target, int m, int n) 
    {
        vector<vector<bool>> visited(m, vector<bool>(n));
        visited[box.px][box.py] = true;
        queue<Node> q({box});
        while (!q.empty()) 
        {
            Node cur = q.front();
            q.pop();
            if (cur.px == target.px and cur.py == target.py) return true;
            Node next{};
            for (const auto& dir : dirs) 
            {
                next.px = cur.px + dir.front();
                next.py = cur.py + dir.back();
                if (invalid(next.px, next.py, grid, m, n) or (next.px == box.bx and next.py == box.by) or visited[next.px][next.py]) continue;
                visited[next.px][next.py] = true;
                q.push(next);
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    class Node {
        int result = 0, bx = 0, by = 0, px = 0, py = 0;
    }
    
    int[][] dirs = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    
    public int minPushBox(char[][] grid) {
        int m = grid.length, n = grid[0].length;
        boolean[][][][] visited = new boolean[m][n][m][n];
        Node start = new Node();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 'S') {
                    start.px = i;
                    start.py = j;
                } else if (grid[i][j] == 'B') {
                    start.bx = i;
                    start.by = j;
                }
            }
        }
        visited[start.bx][start.by][start.px][start.py] = true;
        Queue<Node> queue = new LinkedList<>();
        queue.offer(start);
        while (!queue.isEmpty()) {
            Node cur = queue.poll();
            if (grid[cur.bx][cur.by] == 'T') return cur.result;
            for (int[] dir : dirs) 
            {
                Node next = new Node();
                next.bx = cur.bx + dir[0];
                next.by = cur.by + dir[1];
                next.px = cur.bx - dir[0];
                next.py = cur.by - dir[1];
                if (invalid(next.bx, next.by, grid, m, n) || invalid(next.px, next.py, grid, m, n) || visited[next.bx][next.by][next.px][next.py]) continue;
                if (check(grid, cur, next, m, n)) {
                    next.result = cur.result + 1;
                    visited[next.bx][next.by][next.px][next.py] = true;
                    queue.offer(next);
                }
            }
        }
        return -1; 
    }
    
    private boolean invalid(int x, int y, char[][] grid, int m, int n) {
        return x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == '#';
    }

    private boolean check(char[][] grid, Node box, Node target, int m, int n) {
        boolean[][] visited = new boolean[m][n];
        visited[box.px][box.py] = true;
        Queue<Node> queue = new LinkedList<>();
        queue.offer(box);
        while (!queue.isEmpty()) {
            Node cur = queue.poll();
            if (cur.px == target.px && cur.py == target.py) return true;
            for (int[] dir : dirs) 
            {
                Node next = new Node();
                next.px = cur.px + dir[0];
                next.py = cur.py + dir[1];
                if (invalid(next.px, next.py, grid, m, n) || (next.px == box.bx && next.py == box.by) || visited[next.px][next.py]) continue;
                visited[next.px][next.py] = true;
                queue.offer(next);
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def minPushBox(self, grid: List[List[str]]) -> int:
        m, n, tx, ty, px, py, bx, by = len(grid), len(grid[0]), 0, 0, 0, 0, 0, 0
        def valid(px: int, py: int, x: int, y: int, bx: int, by: int) -> bool:
            if px == x and py == y:
                return True
            q, v = deque([(px, py)]), {(px, py), (bx, by)}
            while q:
                a, b = q.popleft()
                for i, j in [(a + 1, b), (a - 1, b), (a, b + 1), (a, b - 1)]:
                    if -1 < i < m and -1 < j < n and grid[i][j] != '#' and (i, j) not in v:
                        if i == x and j == y:
                            return True
                        v.add((i, j))
                        q.append((i, j))
            return False
        
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 'T':
                    tx, ty = i, j
                elif grid[i][j] == 'S':
                    px, py = i, j
                elif grid[i][j] == 'B':
                    bx, by = i, j
        queue, visited = deque([(bx, by, px, py, 0)]), {(bx, by, px, py)}
        while queue:
            a, b, x, y, result = queue.popleft()
            dirs = {0: (a - 1, b), 1: (a + 1, b), 2: (a, b - 1), 3: (a, b + 1)}
            for d, (i, j) in enumerate([(a + 1, b), (a - 1, b), (a, b + 1 ), (a, b - 1)]):
                if -1 < i < m and -1 < j < n and grid[i][j] != '#' and -1 < dirs[d][0] < m and -1 < dirs[d][1] < n and grid[dirs[d][0]][dirs[d][1]] != '#' and (dirs[d][0], dirs[d][1], a, b) not in visited and valid(x, y, i, j, a, b):
                    if dirs[d] == (tx, ty):
                        return result + 1
                    visited.add((dirs[d][0], dirs[d][1], a, b))
                    queue.append((dirs[d][0], dirs[d][1], a, b, result + 1))
        return -1
```
