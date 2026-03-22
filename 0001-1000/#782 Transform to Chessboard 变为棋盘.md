# 782 Transform to Chessboard 变为棋盘

__Description__:
You are given an n x n binary grid board. In each move, you can swap any two rows with each other, or any two columns with each other.

Return the minimum number of moves to transform the board into a chessboard board. If the task is impossible, return -1.

A chessboard board is a board where no 0's and no 1's are 4-directionally adjacent.

__Example:__

Example 1:

![chessboard 1](https://assets.leetcode.com/uploads/2021/06/29/chessboard1-grid.jpg)

Input: board = [[0,1,1,0],[0,1,1,0],[1,0,0,1],[1,0,0,1]]
Output: 2
Explanation: One potential sequence of moves is shown.
The first move swaps the first and second column.
The second move swaps the second and third row.

Example 2:

![chessboard 2](https://assets.leetcode.com/uploads/2021/06/29/chessboard2-grid.jpg)

Input: board = [[0,1],[1,0]]
Output: 0
Explanation: Also note that the board with 0 in the top left corner, is also a valid chessboard.

Example 3:

![chessboard 3](https://assets.leetcode.com/uploads/2021/06/29/chessboard3-grid.jpg)

Input: board = [[1,0],[1,0]]
Output: -1
Explanation: No matter what sequence of moves you make, you cannot end with a valid chessboard.

__Constraints:__

n == board.length
n == board[i].length
2 <= n <= 30
board[i][j] is either 0 or 1.

__题目描述__:
一个 N x N的 board 仅由 0 和 1 组成 。每次移动，你能任意交换两列或是两行的位置。

输出将这个矩阵变为 “棋盘” 所需的最小移动次数。“棋盘” 是指任意一格的上下左右四个方向的值均与本身不同的矩阵。如果不存在可行的变换，输出 -1。

__示例 :__

示例 1:

输入: board = [[0,1,1,0],[0,1,1,0],[1,0,0,1],[1,0,0,1]]
输出: 2
解释:
一种可行的变换方式如下，从左到右：

```text
0110     1010     1010
0110 --> 1010 --> 0101
1001     0101     1010
1001     0101     0101
```

第一次移动交换了第一列和第二列。
第二次移动交换了第二行和第三行。

示例 2:

输入: board = [[0, 1], [1, 0]]
输出: 0
解释:
注意左上角的格值为0时也是合法的棋盘，如：

```text
01
10
```

也是合法的棋盘.

示例 3:

输入: board = [[1, 0], [1, 0]]
输出: -1
解释:
任意的变换都不能使这个输入变为合法的棋盘。

__提示:__

board 是方阵，且行列数的范围是[2, 30]。
board[i][j] 将只包含 0或 1。

__思路__:

数学
每一行必须要为 '0101010' 或者 '1010101', 即 '1' 和 '0' 交替出现
所以对于任何 2 * 2 的小方阵, 4 个角都必须为全 '1' 或者 全 '0' 或者 2 个 '1' 和 2 个 '0'
如果 n 为奇数, 那么, 第一行的和等于 sum(n + 1) >> 1 或者 sum(n - 1) >> 1, 其余行要么和第一行相同, 要么和第一行各位完全相反, 且相反的行数刚好等于 sum(n + 1) >> 1 或者 sum(n - 1) >> 1, 此时, 最少步数为, ((n - 第一行与 '010...' 的不同个数 if sum(第一行) > (n >> 1) else 第一行与 '010...' 的不同个数) + (n - 第一列与 '010...' 的不同个数 if sum(第一列) > (n >> 1) else 第一列与 '010...' 的不同个数)) >> 1
如果 n 为偶数, 那么, 第一行的和等于 sum(n) >> 1, 其余行要么和第一行相同, 要么和第一行各位完全相反, 且相反的行数刚好等于 sum(n) >> 1, 此时, 最少步数为, (min(第一行与 '0101...' 的不同个数，第一行与 '1010...' 的不同个数) + min(第一列与 '0101...' 的不同个数，第一列与 '1010...' 的不同个数)) >> 1
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int movesToChessboard(vector<vector<int>>& board) 
    {
        int n = board.size(), row = accumulate(board.front().begin(), board.front().end(), 0), col = 0, r = 0, c = 0;
        for (int i = 0; i < n; i++) 
        {
            col += board[i].back();
            r += (board.front()[i] == (i & 1) ? 0 : 1);
            c += (board[i].back() == (i & 1) ? 0 : 1);
            for (int j = 0; j < n; j++) if (board.front().front() ^ board[i].front() ^ board.front()[j] ^ board[i][j]) return -1;
        }
        return ((n & 1) ? (((row == (n >> 1) or row == (n >> 1) + 1) and (col == (n >> 1) or col == (n >> 1) + 1)) ? (((row << 1) > n ? n - r : r) + ((col << 1) > n ? n - c : c) >> 1) : -1) : (((row << 1) == n and (col << 1) == n) ? ((min(r, n - r) + min(c, n - c)) >> 1) : -1));
    }
};
```

__Java__:

```Java
class Solution {
    public int movesToChessboard(int[][] board) {
        int n = board.length, row = 0, col = 0, r = 0, c = 0;
        for (int i = 0; i < n; i++) {
            row += board[0][i];
            col += board[i][0];
            r += (board[0][i] == (i & 1) ? 0 : 1);
            c += (board[i][0] == (i & 1) ? 0 : 1);
            for (int j = 0; j < n; j++) if ((board[0][0] ^ board[i][0] ^ board[0][j] ^ board[i][j]) != 0) return -1;
        }
        return ((n & 1) == 0 ? (((row << 1) == n && (col << 1) == n) ? ((Math.min(r, n - r) + Math.min(c, n - c)) >> 1) : -1) : (((row == (n >> 1) || row == (n >> 1) + 1) && (col == (n >> 1) || col == (n >> 1) + 1)) ? (((row << 1) > n ? n - r : r) + ((col << 1) > n ? n - c : c) >> 1) : -1));
    }
}
```

__Python__:

```Python
class Solution:
    def movesToChessboard(self, board: List[List[int]]) -> int:
        return -1 if any(board[0][0] ^ board[i][0] ^ board[0][j] ^ board[i][j] for i in range(len(board)) for j in range(len(board))) or ((len(board) & 1 and not ((sum(board[0]) == (len(board) >> 1) or sum(board[0]) == (len(board) >> 1) + 1) and (sum(board[i][0] for i in range(len(board))) == (len(board) >> 1) or sum(board[i][0] for i in range(len(board))) == (len(board) >> 1) + 1))) or (not (len(board) & 1) and not ((sum(board[0]) << 1) == len(board) and (sum(board[i][0] for i in range(len(board))) << 1) == len(board)))) else ((len(board) - sum(board[0][i] != (i & 1) for i in range(len(board))) if (sum(board[0]) << 1) > len(board) else sum(board[0][i] != (i & 1) for i in range(len(board)))) + (len(board) - sum(board[i][0] != (i & 1) for i in range(len(board))) if (sum(board[i][0] for i in range(len(board))) << 1) > len(board) else sum(board[i][0] != (i & 1) for i in range(len(board))))) >> 1 if len(board) & 1 else (min(sum(board[0][i] != (i & 1) for i in range(len(board))), len(board) - sum(board[0][i] != (i & 1) for i in range(len(board)))) + min(sum(board[i][0] != (i & 1) for i in range(len(board))), len(board) - sum(board[i][0] != (i & 1) for i in range(len(board))))) >> 1
```
