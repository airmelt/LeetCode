# 794 Valid Tic-Tac-Toe State 有效的井字游戏

__Description__:
Given a Tic-Tac-Toe board as a string array board, return true if and only if it is possible to reach this board position during the course of a valid tic-tac-toe game.

The board is a 3 x 3 array that consists of characters ' ', 'X', and 'O'. The ' ' character represents an empty square.

Here are the rules of Tic-Tac-Toe:

Players take turns placing characters into empty squares ' '.
The first player always places 'X' characters, while the second player always places 'O' characters.
'X' and 'O' characters are always placed into empty squares, never filled ones.
The game ends when there are three of the same (non-empty) character filling any row, column, or diagonal.
The game also ends if all squares are non-empty.
No more moves can be played if the game is over.

__Example:__

Example 1:

![tictactoe 1](https://assets.leetcode.com/uploads/2021/05/15/tictactoe1-grid.jpg)

Input: board = ["O  ","   ","   "]
Output: false
Explanation: The first player always plays "X".

Example 2:

![tictactoe 2](https://assets.leetcode.com/uploads/2021/05/15/tictactoe2-grid.jpg)

Input: board = ["XOX"," X ","   "]
Output: false
Explanation: Players take turns making moves.

Example 3:

![tictactoe 3](https://assets.leetcode.com/uploads/2021/05/15/tictactoe3-grid.jpg)

Input: board = ["XXX","   ","OOO"]
Output: false

Example 4:

![tictactoe 4](https://assets.leetcode.com/uploads/2021/05/15/tictactoe4-grid.jpg)

Input: board = ["XOX","O O","XOX"]
Output: true

__Constraints:__

board.length == 3
board[i].length == 3
board[i][j] is either 'X', 'O', or ' '.

__题目描述__:
用字符串数组作为井字游戏的游戏板 board。当且仅当在井字游戏过程中，玩家有可能将字符放置成游戏板所显示的状态时，才返回 true。

该游戏板是一个 3 x 3 数组，由字符 " "，"X" 和 "O" 组成。字符 " " 代表一个空位。

以下是井字游戏的规则：

玩家轮流将字符放入空位（" "）中。
第一个玩家总是放字符 “X”，且第二个玩家总是放字符 “O”。
“X” 和 “O” 只允许放置在空位中，不允许对已放有字符的位置进行填充。
当有 3 个相同（且非空）的字符填充任何行、列或对角线时，游戏结束。
当所有位置非空时，也算为游戏结束。
如果游戏结束，玩家不允许再放置字符。

__示例 :__

示例 1:
输入: board = ["O  ", "   ", "   "]
输出: false
解释: 第一个玩家总是放置“X”。

示例 2:
输入: board = ["XOX", " X ", "   "]
输出: false
解释: 玩家应该是轮流放置的。

示例 3:
输入: board = ["XXX", "   ", "OOO"]
输出: false

示例 4:
输入: board = ["XOX", "O O", "XOX"]
输出: true

__说明:__

游戏板 board 是长度为 3 的字符串数组，其中每个字符串 board[i] 的长度为 3。
 board[i][j] 是集合 {" ", "X", "O"} 中的一个字符。

__思路__:

模拟
先统计 'X' 和 'O' 的数目, 需要满足 x == o or x - 1 == o
然后判断胜者, 若 'X' 胜出, 'X' 先放, 所以 x - 1 == o
若 'O' 胜出, 则 x == o
判断胜利的条件, 给定字符, 遍历行和列, 看是否有行列全与给定字符相等, 然后查看主对角线 (board[0][0], board[1][1], board[2][2])和副对角线 (board[0][2], board[1][1], board[2][0])
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validTicTacToe(vector<string>& board) 
    {
        int x = 0, o = 0;
        for (const auto& row: board) 
        {
            for (const auto& c: row) 
            {
                if (c == 'X') ++x;
                if (c == 'O') ++o;
            }
        }
        return (o == x or o == x - 1) and !(win(board, 'X') and o != x - 1) and !(win(board, 'O') and o != x);
    }
private:
    bool win(vector<string>& board, char c) 
    {
        return (c == board[0][2] and c == board[1][2] and c == board[2][2]) or (c == board[0][1] and c == board[1][1] and c == board[2][1]) or (c == board[0][0] and c == board[1][0] and c == board[2][0]) or (c == board[0][0] and c == board[0][1] and c == board[0][2]) or (c == board[1][0] and c == board[1][1] and c == board[1][2]) or (c == board[2][0] and c == board[2][1] and c == board[2][2]) or (c == board[0][0] and c == board[1][1] and c == board[2][2]) or (c == board[0][2] and c == board[1][1] and c == board[2][0]);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validTicTacToe(String[] board) {
        int x = 0, o = 0;
        for (String row: board) {
            for (char c: row.toCharArray()) {
                if (c == 'X') ++x;
                if (c == 'O') ++o;
            }
        }
        if (o != x && o != x - 1) return false;
        if (win(board, 'X') && o != x - 1) return false;
        if (win(board, 'O') && o != x) return false;
        return true;
    }

    private boolean win(String[] board, char c) {
        for (int i = 0; i < 3; i++) {
            if (c == board[0].charAt(i) && c == board[1].charAt(i) && c == board[2].charAt(i)) return true;
            if (c == board[i].charAt(0) && c == board[i].charAt(1) && c == board[i].charAt(2)) return true;
        }
        if (c == board[0].charAt(0) && c == board[1].charAt(1) && c == board[2].charAt(2)) return true;
        if (c == board[0].charAt(2) && c == board[1].charAt(1) && c == board[2].charAt(0)) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def validTicTacToe(self, board: List[str]) -> bool:
        return ((o := ''.join(board).count('O')) == (x := ''.join(board).count('X')) or o == x - 1) and not ((any(row == 'XXX' for row in board) or any(''.join(column) == 'XXX' for column in zip(*board)) or (board[0][0] + board[1][1] + board[2][2] == 'XXX') or (board[0][2] + board[1][1] + board[2][0] == 'XXX')) and o != x - 1) and not ((any(row == 'OOO' for row in board) or any(''.join(column) == 'OOO' for column in zip(*board)) or (board[0][0] + board[1][1] + board[2][2] == 'OOO') or (board[0][2] + board[1][1] + board[2][0] == 'OOO')) and o != x)
```
