# 1958 Check if Move is Legal 检查操作是否合法

__Description:__

You are given a __0-indexed__ `8 x 8` grid `board`, where `board[r][c]` represents the cell `(r, c)` on a game board. On the board, free cells are represented by `'.'`, white cells are represented by `'W'`, and black cells are represented by `'B'`.

Each move in this game consists of choosing a free cell and changing it to the color you are playing as (either white or black). However, a move is only legal if, after changing it, the cell becomes the endpoint of a good line (horizontal, vertical, or diagonal).

A good line is a line of three or more cells (including the endpoints) where the endpoints of the line are one color, and the remaining cells in the middle are the opposite color (no cells in the line are free). You can find examples for good lines in the figure below:

![1958-1](https://assets.leetcode.com/uploads/2021/07/22/goodlines5.png)

Given two integers `rMove` and `cMove` and a character `color` representing the color you are playing as (white or black), return `true` _if changing cell_ `(rMove, cMove)` _to color_ `color` _is a __legal__ move, or_ `false` _if it is not legal_.

__Example:__

Example 1:

![1958-2](https://assets.leetcode.com/uploads/2021/07/10/grid11.png)

```text
Input: board = [[".",".",".","B",".",".",".","."],[".",".",".","W",".",".",".","."],[".",".",".","W",".",".",".","."],[".",".",".","W",".",".",".","."],["W","B","B",".","W","W","W","B"],[".",".",".","B",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","W",".",".",".","."]], rMove = 4, cMove = 3, color = "B"
Output: true
Explanation: '.', 'W', and 'B' are represented by the colors blue, white, and black respectively, and cell (rMove, cMove) is marked with an 'X'.
The two good lines with the chosen cell as an endpoint are annotated above with the red rectangles.
```

Example 2:

![1958-3](https://assets.leetcode.com/uploads/2021/07/10/grid2.png)

```text
Input: board = [[".",".",".",".",".",".",".","."],[".","B",".",".","W",".",".","."],[".",".","W",".",".",".",".","."],[".",".",".","W","B",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".","B","W",".","."],[".",".",".",".",".",".","W","."],[".",".",".",".",".",".",".","B"]], rMove = 4, cMove = 4, color = "W"
Output: false
Explanation: While there are good lines with the chosen cell as a middle cell, there are no good lines with the chosen cell as an endpoint.
```

__Constraints:__

- `board.length == board[r].length == 8`
- `0 <= rMove, cMove < 8`
- `board[rMove][cMove] == '.'`
- `color` is either `'B'` or `'W'`.

__题目描述:__

给你一个下标从 __0__ 开始的 `8 x 8` 网格 `board` ，其中 `board[r][c]` 表示游戏棋盘上的格子 `(r, c)` 。棋盘上空格用 `'.'` 表示，白色格子用 `'W'` 表示，黑色格子用 `'B'` 表示。

游戏中每次操作步骤为：选择一个空格子，将它变成你正在执行的颜色（要么白色，要么黑色）。但是，合法 操作必须满足：涂色后这个格子是 好线段的一个端点 （好线段可以是水平的，竖直的或者是对角线）。

好线段 指的是一个包含 三个或者更多格子（包含端点格子）的线段，线段两个端点格子为 同一种颜色 ，且中间剩余格子的颜色都为 另一种颜色 （线段上不能有任何空格子）。你可以在下图找到好线段的例子：

![1958-4](https://assets.leetcode.com/uploads/2021/07/22/goodlines5.png)

给你两个整数 `rMove` 和 `cMove` 以及一个字符 `color` ，表示你正在执行操作的颜色（白或者黑），如果将格子 `(rMove, cMove)` 变成颜色 `color` 后，是一个 __合法__ 操作，那么返回 `true` ，如果不是合法操作返回 `false` 。

__示例:__

示例 1：

![1958-5](https://assets.leetcode.com/uploads/2021/07/10/grid11.png)

```text
输入：board = [[".",".",".","B",".",".",".","."],[".",".",".","W",".",".",".","."],[".",".",".","W",".",".",".","."],[".",".",".","W",".",".",".","."],["W","B","B",".","W","W","W","B"],[".",".",".","B",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","W",".",".",".","."]], rMove = 4, cMove = 3, color = "B"
输出：true
解释：'.'，'W' 和 'B' 分别用颜色蓝色，白色和黑色表示。格子 (rMove, cMove) 用 'X' 标记。
以选中格子为端点的两个好线段在上图中用红色矩形标注出来了。
```

示例 2：

![1958-6](https://assets.leetcode.com/uploads/2021/07/10/grid2.png)

```text
输入：board = [[".",".",".",".",".",".",".","."],[".","B",".",".","W",".",".","."],[".",".","W",".",".",".",".","."],[".",".",".","W","B",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".","B","W",".","."],[".",".",".",".",".",".","W","."],[".",".",".",".",".",".",".","B"]], rMove = 4, cMove = 4, color = "W"
输出：false
解释：虽然选中格子涂色后，棋盘上产生了好线段，但选中格子是作为中间格子，没有产生以选中格子为端点的好线段。
```

__提示：__

- `board.length == board[r].length == 8`
- `0 <= rMove, cMove < 8`
- `board[rMove][cMove] == '.'`
- `color` 要么是 `'B'` 要么是 `'W'` 。

__思路:__

```text
模拟
枚举 8 方向
路径上要求不为空格而且不能与 color 相同
统计路径长度，如果长度小于 3 则不合法
否则合法
时间复杂度为 O(MAX(M, N)), 空间复杂度为 O(1), M, N 分别为 board 的行数和列数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkMove(vector<vector<char>>& board, int rMove, int cMove, char color) 
    {
        for (const auto& [dx, dy]: {pair{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}}) 
        {
            for (int x = rMove + dx, y = cMove + dy, cur = 2; -1 < x and x < 8 and -1 < y and y < 8; x += dx, y += dy, ++cur) 
            {
                if (board[x][y] == '.' or cur < 3 and board[x][y] == color) break;
                else if (board[x][y] == color) return true;
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkMove(char[][] board, int rMove, int cMove, char color) {
        int[][] dir = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}, {1, 1}, {-1, -1}, {1, -1}, {-1, 1}};
        for (int[] d : dir) {
            for (int x = rMove + d[0], y = cMove + d[1], cur = 2; -1 < x && x < 8 && -1 < y && y < 8; x += d[0], y += d[1], ++cur) {
                if (board[x][y] == '.' || cur < 3 && board[x][y] == color) break;
                else if (board[x][y] == color) return true;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def checkMove(self, board: List[List[str]], rMove: int, cMove: int, color: str) -> bool:
        for dx, dy in [[-1, -1], [-1, 0], [-1, 1], [0, -1], [0, 1], [1, -1], [1, 0], [1, 1]]:
            x, y, cur = rMove + dx, cMove + dy, 2
            while -1 < x < 8 and -1 < y < 8:
                if board[x][y] == '.' or cur < 3 and board[x][y] == color:
                    break
                elif board[x][y] == color:
                    return True
                x += dx
                y += dy
                cur += 1
        return False
```
