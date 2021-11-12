# 909 Snakes and Ladders 蛇梯棋

__Description__:
You are given an n x n integer matrix board where the cells are labeled from 1 to n^2 in a Boustrophedon style starting from the bottom left of the board (i.e. board[n - 1][0]) and alternating direction each row.

You start on square 1 of the board. In each move, starting from square curr, do the following:

Choose a destination square next with a label in the range [curr + 1, min(curr + 6, n^2)].
This choice simulates the result of a standard 6-sided die roll: i.e., there are always at most 6 destinations, regardless of the size of the board.
If next has a snake or ladder, you must move to the destination of that snake or ladder. Otherwise, you move to next.
The game ends when you reach the square n^2.
A board square on row r and column c has a snake or ladder if board[r][c] != -1. The destination of that snake or ladder is board[r][c]. Squares 1 and n^2 do not have a snake or ladder.

Note that you only take a snake or ladder at most once per move. If the destination to a snake or ladder is the start of another snake or ladder, you do not follow the subsequent snake or ladder.

For example, suppose the board is [[-1,4],[-1,3]], and on the first move, your destination square is 2. You follow the ladder to square 3, but do not follow the subsequent ladder to 4.
Return the least number of moves required to reach the square n2. If it is not possible to reach the square, return -1.

__Example:__

Example 1:

![snakes](https://assets.leetcode.com/uploads/2018/09/23/snakes.png)

Input: board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
Output: 4
Explanation:
In the beginning, you start at square 1 (at row 5, column 0).
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 and must take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
This is the lowest possible number of moves to reach the last square, so return 4.

Example 2:

Input: board = [[-1,-1],[-1,3]]
Output: 1

__Constraints:__

n == board.length == board[i].length
2 <= n <= 20
grid[i][j] is either -1 or in the range [1, n^2].
The squares labeled 1 and n2 do not have any ladders or snakes.

__题目描述__:
给你一个大小为 n x n 的整数矩阵 board ，方格按从 1 到 n^2 编号，编号遵循 转行交替方式 ，从左下角开始 （即，从 board[n - 1][0] 开始）每一行交替方向。

玩家从棋盘上的方格 1 （总是在最后一行、第一列）开始出发。

每一回合，玩家需要从当前方格 curr 开始出发，按下述要求前进：

选定目标方格 next ，目标方格的编号符合范围 [curr + 1, min(curr + 6, n^2)] 。
该选择模拟了掷 六面体骰子 的情景，无论棋盘大小如何，玩家最多只能有 6 个目的地。
传送玩家：如果目标方格 next 处存在蛇或梯子，那么玩家会传送到蛇或梯子的目的地。否则，玩家传送到目标方格 next 。
当玩家到达编号 n^2 的方格时，游戏结束。
r 行 c 列的棋盘，按前述方法编号，棋盘格中可能存在 “蛇” 或 “梯子”；如果 board[r][c] != -1，那个蛇或梯子的目的地将会是 board[r][c]。编号为 1 和 n^2 的方格上没有蛇或梯子。

注意，玩家在每回合的前进过程中最多只能爬过蛇或梯子一次：就算目的地是另一条蛇或梯子的起点，玩家也 不能 继续移动。

举个例子，假设棋盘是 [[-1,4],[-1,3]] ，第一次移动，玩家的目标方格是 2 。那么这个玩家将会顺着梯子到达方格 3 ，但 不能 顺着方格 3 上的梯子前往方格 4 。
返回达到编号为 n^2 的方格所需的最少移动次数，如果不可能，则返回 -1。

__示例 :__

示例 1：

![蛇梯棋](https://assets.leetcode.com/uploads/2018/09/23/snakes.png)

输入：board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
输出：4
解释：
首先，从方格 1 [第 5 行，第 0 列] 开始。
先决定移动到方格 2 ，并必须爬过梯子移动到到方格 15 。
然后决定移动到方格 17 [第 3 行，第 4 列]，必须爬过蛇到方格 13 。
接着决定移动到方格 14 ，且必须通过梯子移动到方格 35 。
最后决定移动到方格 36 , 游戏结束。
可以证明需要至少 4 次移动才能到达最后一个方格，所以答案是 4 。

示例 2：

输入：board = [[-1,-1],[-1,3]]
输出：1

__提示:__

n == board.length == board[i].length
2 <= n <= 20
grid[i][j] 的值是 -1 或在范围 [1, n^2] 内
编号为 1 和 n2 的方格上没有蛇或梯子

__思路__:

BFS
每次模拟投骰子, i 从 1 遍历到 6, 将没有遍历过的节点加入队列
遇到梯子或者蛇就移动
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int snakesAndLadders(vector<vector<int>>& board) 
    {
        int n = board.size(), mask = (1 << 10) - 1, end = n * n;
        set<int> visited;
        queue<int> q;
        q.push((1 << 10) + 0);
        while (!q.empty()) 
        {
            int head = q.front(), cur = head >> 10, step = head & mask;
            q.pop();
            for (int i = 1; i < 7; i++) 
            {
                int target = cur + i, r = (target - 1) / n, x = n - r - 1, y = 0;
                if (target > end) break;
                if (r & 1) y = n - 1 - (target - 1) % n;
                else y = (target - 1) % n;
                if (board[x][y] != -1) target = board[x][y];
                if (target == end) return step + 1;
                if (!visited.count(target)) 
                {
                    visited.insert(target);
                    q.push((target << 10) + step + 1);
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
    public int snakesAndLadders(int[][] board) {
        int n = board.length, mask = (1 << 10) - 1, end = n * n;
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();
        queue.offer((1 << 10) + 0);
        while (!queue.isEmpty()) {
            int head = queue.poll(), cur = head >> 10, step = head & mask;
            for (int i = 1; i < 7; i++) {
                int target = cur + i, r = (target - 1) / n, x = n - r - 1, y = 0;
                if (target > end) break;
                if ((r & 1) != 0) y = n - 1 - (target - 1) % n;
                else y = (target - 1) % n;
                if (board[x][y] != -1) target = board[x][y];
                if (target == end) return step + 1;
                if (!visited.contains(target)) {
                    visited.add(target);
                    queue.offer((target << 10) + step + 1);
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
    def snakesAndLadders(self, board: List[List[int]]) -> int:
        n, q, visited = len(board), deque([(1, 0)]), set()
        while q:
            cur, step = q.popleft()
            for i in range(1, 7):
                if (target := cur + i) > n * n:
                    break
                (x, y) = (n - r - 1, n - 1 - (target - 1) % n) if (r := (target - 1) // n) & 1 else (n - r - 1, (target - 1) % n)
                if board[x][y] != -1:
                    target = board[x][y]
                if target == n * n:
                    return step + 1
                if target not in visited:
                    visited.add(target)
                    q.append((target, step + 1))
        return -1
```
