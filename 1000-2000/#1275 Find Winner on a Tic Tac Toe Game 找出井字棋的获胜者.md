# 1275 Find Winner on a Tic Tac Toe Game 找出井字棋的获胜者

__Description__:
Tic-tac-toe is played by two players A and B on a 3 x 3 grid.

Here are the rules of Tic-Tac-Toe:

Players take turns placing characters into empty squares (" ").
The first player A always places "X" characters, while the second player B always places "O" characters.
"X" and "O" characters are always placed into empty squares, never on filled ones.
The game ends when there are 3 of the same (non-empty) character filling any row, column, or diagonal.
The game also ends if all squares are non-empty.
No more moves can be played if the game is over.
Given an array moves where each element is another array of size 2 corresponding to the row and column of the grid where they mark their respective character in the order in which A and B play.

Return the winner of the game if it exists (A or B), in case the game ends in a draw return "Draw", if there are still movements to play return "Pending".

You can assume that moves is valid (It follows the rules of Tic-Tac-Toe), the grid is initially empty and A will play first.

__Example:__

Example 1:

Input: moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
Output: "A"
Explanation: "A" wins, he always plays first.

```text
"X  "    "X  "    "X  "    "X  "    "X  "
"   " -> "   " -> " X " -> " X " -> " X "
"   "    "O  "    "O  "    "OO "    "OOX"
```

Example 2:

Input: moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
Output: "B"
Explanation: "B" wins.

```text
"X  "    "X  "    "XX "    "XXO"    "XXO"    "XXO"
"   " -> " O " -> " O " -> " O " -> "XO " -> "XO " 
"   "    "   "    "   "    "   "    "   "    "O  "
```

Example 3:

Input: moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
Output: "Draw"
Explanation: The game ends in a draw since there are no moves to make.

```text
"XXO"
"OOX"
"XOX"
```

Example 4:

Input: moves = [[0,0],[1,1]]
Output: "Pending"
Explanation: The game has not finished yet.

```text
"X  "
" O "
"   "
```

__Constraints:__

1 <= moves.length <= 9
moves[i].length == 2
0 <= moves[i][j] <= 2
There are no repeated elements on moves.
moves follow the rules of tic tac toe.

__题目描述__:
A 和 B 在一个 3 x 3 的网格上玩井字棋。

井字棋游戏的规则如下：

玩家轮流将棋子放在空方格 (" ") 上。
第一个玩家 A 总是用 "X" 作为棋子，而第二个玩家 B 总是用 "O" 作为棋子。
"X" 和 "O" 只能放在空方格中，而不能放在已经被占用的方格上。
只要有 3 个相同的（非空）棋子排成一条直线（行、列、对角线）时，游戏结束。
如果所有方块都放满棋子（不为空），游戏也会结束。
游戏结束后，棋子无法再进行任何移动。
给你一个数组 moves，其中每个元素是大小为 2 的另一个数组（元素分别对应网格的行和列），它按照 A 和 B 的行动顺序（先 A 后 B）记录了两人各自的棋子位置。

如果游戏存在获胜者（A 或 B），就返回该游戏的获胜者；如果游戏以平局结束，则返回 "Draw"；如果仍会有行动（游戏未结束），则返回 "Pending"。

你可以假设 moves 都 有效（遵循井字棋规则），网格最初是空的，A 将先行动。

__示例 :__

示例 1：

输入：moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
输出："A"
解释："A" 获胜，他总是先走。

```text
"X  "    "X  "    "X  "    "X  "    "X  "
"   " -> "   " -> " X " -> " X " -> " X "
"   "    "O  "    "O  "    "OO "    "OOX"
```

示例 2：

输入：moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
输出："B"
解释："B" 获胜。

```text
"X  "    "X  "    "XX "    "XXO"    "XXO"    "XXO"
"   " -> " O " -> " O " -> " O " -> "XO " -> "XO " 
"   "    "   "    "   "    "   "    "   "    "O  "
```

示例 3：

输入：moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
输出："Draw"
输出：由于没有办法再行动，游戏以平局结束。

```text
"XXO"
"OOX"
"XOX"
```

示例 4：

输入：moves = [[0,0],[1,1]]
输出："Pending"
解释：游戏还没有结束。

```text
"X  "
" O "
"   "
```

__提示：__

1 <= moves.length <= 9
moves[i].length == 2
0 <= moves[i][j] <= 2
moves 里没有重复的元素。
moves 遵循井字棋的规则。

__思路__:

1. 按照井字棋的规则填充 1或者 2, 判断是否胜利即可
时间复杂度O(n), 空间复杂度O(1)
2. 转化为二进制, 用 & mask判断, mask为胜利的条件
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string tictactoe(vector<vector<int>>& moves) 
    {
        int a = 0, b = 0, win[] = {448, 56, 7, 292, 146, 73, 273, 84}, n = moves.size();
        for (int i = 0; i < n; i += 2) a += pow(2, moves[i][0] * 3 + moves[i][1]);
        for (int i = 1; i < n; i += 2) b += pow(2, moves[i][0] * 3 + moves[i][1]);
        for (auto w : win)
        {
            if ((a & w) == w) return "A";
            if ((b & w) == w) return "B";
        }
        return n == 9 ? "Draw" : "Pending";
    }
};
```

__Java__:

```Java
class Solution {
    public String tictactoe(int[][] moves) {
        int[][] record = new int[3][3];
        for (int i = 0; i < moves.length; i++) record[moves[i][0]][moves[i][1]] = i % 2 == 0 ? 1 : 2;
        if (isVictory(record, 1)) return "A";
        if (isVictory(record, 2)) return "B";
        return moves.length == 9 ? "Draw" : "Pending";
    }

    private boolean isVictory(int[][] record, int player) {
        return record[0][0] == record[0][1] && record[0][1] == record[0][2] && record[0][2] == player || record[1][0] == record[1][1] && record[1][1] == record[1][2] && record[1][2] == player || record[2][0] == record[2][1] && record[2][1] == record[2][2] && record[2][2] == player || record[0][0] == record[1][0] && record[1][0] == record[2][0] && record[2][0] == player || record[0][1] == record[1][1] && record[1][1] == record[2][1] && record[2][1] == player || record[0][2] == record[1][2] && record[1][2] == record[2][2] && record[2][2] == player || record[0][0] == record[1][1] && record[1][1] == record[2][2] && record[2][2] == player || record[0][2] == record[1][1] && record[1][1] == record[2][0] && record[2][0] == player;
    }
}
```

__Python__:

```Python
class Solution:
    def tictactoe(self, moves: List[List[int]]) -> str:
        A, B, f = moves[0::2], moves[1::2], lambda x: sum((2 ** (i * 3 + j) for i, j in x))
        a, b, win = f(A), f(B), [448, 56, 7, 292, 146, 73, 273, 84]
        for w in win:
            if a & w == w:
                return "A"
            if b & w == w:
                return "B"
        return "Draw" if len(moves) == 9 else "Pending"
```
