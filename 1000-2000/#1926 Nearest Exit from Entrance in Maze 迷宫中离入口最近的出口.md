# 1926 Nearest Exit from Entrance in Maze 迷宫中离入口最近的出口

__Description:__

You are given an `m x n` matrix `maze` (__0-indexed__) with empty cells (represented as `'.'`) and walls (represented as `'+'`). You are also given the `entrance` of the maze, where `entrance = [entrancerow, entrancecol]` denotes the row and column of the cell you are initially standing at.

In one step, you can move one cell __up__, __down__, __left__, or __right__. You cannot step into a cell with a wall, and you cannot step outside the maze. Your goal is to find the __nearest exit__ from the `entrance`. An __exit__ is defined as an __empty cell__ that is at the __border__ of the `maze`. The `entrance` __does not count__ as an exit.

Return _the __number of steps__ in the shortest path from the_ `entrance` _to the nearest exit, or_ `-1` _if no such path exists_.

__Example:__

Example 1:

![1926-1](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

```text
Input: maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
Output: 1
Explanation: There are 3 exits in this maze at [1,0], [0,2], and [2,3].
Initially, you are at the entrance cell [1,2].
- You can reach [1,0] by moving 2 steps left.
- You can reach [0,2] by moving 1 step up.
It is impossible to reach [2,3] from the entrance.
Thus, the nearest exit is [0,2], which is 1 step away.
```

Example 2:

![1926-2](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

```text
Input: maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
Output: 2
Explanation: There is 1 exit in this maze at [1,2].
[1,0] does not count as an exit since it is the entrance cell.
Initially, you are at the entrance cell [1,0].
- You can reach [1,2] by moving 2 steps right.
Thus, the nearest exit is [1,2], which is 2 steps away.
```

Example 3:

![1926-3](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

```text
Input: maze = [[".","+"]], entrance = [0,0]
Output: -1
Explanation: There are no exits in this maze.
```

__Constraints:__

- `maze.length == m`
- `maze[i].length == n`
- `1 <= m, n <= 100`
- `maze[i][j]` is either `'.'` or `'+'`.
- `entrance.length == 2`
- `0 <= entrancerow < m`
- `0 <= entrancecol < n`
- `entrance` will always be an empty cell.

__题目描述:__

给你一个 `m x n` 的迷宫矩阵 `maze` （__下标从 0 开始__），矩阵中有空格子（用 `'.'` 表示）和墙（用 `'+'` 表示）。同时给你迷宫的入口 `entrance` ，用 `entrance = [entrancerow, entrancecol]` 表示你一开始所在格子的行和列。

每一步操作，你可以往 __上__，__下__，__左__ 或者 __右__ 移动一个格子。你不能进入墙所在的格子，你也不能离开迷宫。你的目标是找到离 `entrance` __最近__ 的出口。__出口__ 的含义是 `maze` __边界__ 上的 __空格子__。 `entrance` 格子 __不算__ 出口。

请你返回从 `entrance` 到最近出口的最短路径的 __步数__ ，如果不存在这样的路径，请你返回 `-1` 。

__示例:__

示例 1：

![1926-4](https://assets.leetcode.com/uploads/2021/06/04/nearest1-grid.jpg)

```text
输入：maze = [["+","+",".","+"],[".",".",".","+"],["+","+","+","."]], entrance = [1,2]
输出：1
解释：总共有 3 个出口，分别位于 (1,0)，(0,2) 和 (2,3) 。
一开始，你在入口格子 (1,2) 处。
- 你可以往左移动 2 步到达 (1,0) 。
- 你可以往上移动 1 步到达 (0,2) 。
从入口处没法到达 (2,3) 。
所以，最近的出口是 (0,2) ，距离为 1 步。
```

示例 2：

![1926-5](https://assets.leetcode.com/uploads/2021/06/04/nearesr2-grid.jpg)

```text
输入：maze = [["+","+","+"],[".",".","."],["+","+","+"]], entrance = [1,0]
输出：2
解释：迷宫中只有 1 个出口，在 (1,2) 处。
(1,0) 不算出口，因为它是入口格子。
初始时，你在入口与格子 (1,0) 处。
- 你可以往右移动 2 步到达 (1,2) 处。
所以，最近的出口为 (1,2) ，距离为 2 步。
```

示例 3：

![1926-6](https://assets.leetcode.com/uploads/2021/06/04/nearest3-grid.jpg)

```text
输入：maze = [[".","+"]], entrance = [0,0]
输出：-1
解释：这个迷宫中没有出口。
```

__提示：__

- `maze.length == m`
- `maze[i].length == n`
- `1 <= m, n <= 100`
- `maze[i][j]` 要么是 `'.'` ，要么是 `'+'` 。
- `entrance.length == 2`
- `0 <= entrancerow < m`
- `0 <= entrancecol < n`
- `entrance` 一定是空格子。

__思路:__

```text
BFS
可以将 maze 中的元素修改为一个不存在的值表示访问过
将入口加入队列
每次弹出队首, 并将 4 个方向可以走的位置加入队列
直到走到出口或者队列为空
每次加入队列的时候顺便记录下步数, 或者用一个变量记录步数, 每次遍历队列中的所有元素为一次步数
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int nearestExit(vector<vector<char>>& maze, vector<int>& entrance) 
    {
        int dir[]{-1, 0, 1, 0, -1}, m = maze.size(), n = maze.front().size();
        queue<vector<int>> q;
        q.push({entrance.front(), entrance.back(), 0});
        maze[entrance.front()][entrance.back()] = '/';
        while (!q.empty()) 
        {
            vector<int> cur = q.front();
            q.pop();
            if (cur[0] == 0 or cur[0] == m - 1 or cur[1] == 0 or cur[1] == n - 1) if (cur[2]) return cur[2];
            for (int i = 0; i < 4; i++) 
            {
                int x = cur[0] + dir[i], y = cur[1] + dir[i + 1];
                if (x > -1 and x < m and y > -1 and y < n and maze[x][y] == '.') 
                {
                    maze[x][y] = '/';
                    q.push({x, y, cur[2] + 1});
                }
            }
        }
        return -1;
    }
};


```

__Java__:

```Java
class Solution {
    public int nearestExit(char[][] maze, int[] entrance) {
        int dir[] = new int[]{-1, 0, 1, 0, -1}, m = maze.length, n = maze[0].length;
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{entrance[0], entrance[1], 0});
        maze[entrance[0]][entrance[1]] = '/';
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (cur[0] == 0 || cur[0] == m - 1 || cur[1] == 0 || cur[1] == n - 1) if (cur[2] != 0) return cur[2];
            for (int i = 0; i < 4; i++) {
                int x = cur[0] + dir[i], y = cur[1] + dir[i + 1];
                if (x > -1 && x < m && y > -1 && y < n && maze[x][y] == '.') {
                    maze[x][y] = '/';
                    queue.offer(new int[]{x, y, cur[2] + 1});
                }
            }
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def nearestExit(self, maze: List[List[str]], entrance: List[int]) -> int:
        d, queue, maze[entrance[0]][entrance[1]], m, n = [-1, 0, 1, 0, -1], deque([entrance + [0]]), '/', len(maze), len(maze[0])
        while queue:
            if (cur := queue.popleft())[0] == 0 or cur[0] == m - 1 or cur[1] == 0 or cur[1] == n - 1:
                if cur[2]:
                    return cur[2]
            for i in range(4):
                x, y = cur[0] + d[i], cur[1] + d[i + 1]
                if -1 < x < m and -1 < y < n and maze[x][y] == '.':
                    queue.append([x, y, cur[2] + 1])
                    maze[x][y] = '/'
        return -1
```
