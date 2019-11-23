__Description__:
On an 8 x 8 chessboard, there is one white rook.  There also may be empty squares, white bishops, and black pawns.  These are given as characters 'R', '.', 'B', and 'p' respectively. Uppercase characters represent white pieces, and lowercase characters represent black pieces.

The rook moves as in the rules of Chess: it chooses one of four cardinal directions (north, east, west, and south), then moves in that direction until it chooses to stop, reaches the edge of the board, or captures an opposite colored pawn by moving to the same square it occupies.  Also, rooks cannot move into the same square as other friendly bishops.

Return the number of pawns the rook can capture in one move.

__Example:__
Example 1:
![Chessboard  1](https://upload-images.jianshu.io/upload_images/16639143-70376cde87ff812d.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
Output: 3
Explanation: 
In this example the rook is able to capture all the pawns.

Example 2:
![Chessboard 2](https://upload-images.jianshu.io/upload_images/16639143-98ae4bc15cd3ee18.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: [[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
Output: 0
Explanation: 
Bishops are blocking the rook to capture any pawn.

Example 3:
![Chessboard  3](https://upload-images.jianshu.io/upload_images/16639143-704327c4aed5f241.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: [[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]
Output: 3
Explanation: 
The rook can capture the pawns at positions b5, d6 and f5.
 
__Note:__

board.length == board[i].length == 8
board[i][j] is either 'R', '.', 'B', or 'p'
There is exactly one cell with board[i][j] == 'R'

__题目描述__:
在一个 8 x 8 的棋盘上，有一个白色车（rook）。也可能有空方块，白色的象（bishop）和黑色的卒（pawn）。它们分别以字符 “R”，“.”，“B” 和 “p” 给出。大写字符表示白棋，小写字符表示黑棋。

车按国际象棋中的规则移动：它选择四个基本方向中的一个（北，东，西和南），然后朝那个方向移动，直到它选择停止、到达棋盘的边缘或移动到同一方格来捕获该方格上颜色相反的卒。另外，车不能与其他友方（白色）象进入同一个方格。

返回车能够在一次移动中捕获到的卒的数量。
 
__示例 :__
示例 1：
![棋盘  1](https://upload-images.jianshu.io/upload_images/16639143-78d221af51f0ff3c.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","R",".",".",".","p"],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释：
在本例中，车能够捕获所有的卒。

示例 2：
![棋盘  2](https://upload-images.jianshu.io/upload_images/16639143-dc0cfba4879fd2ed.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：[[".",".",".",".",".",".",".","."],[".","p","p","p","p","p",".","."],[".","p","p","B","p","p",".","."],[".","p","B","R","B","p",".","."],[".","p","p","B","p","p",".","."],[".","p","p","p","p","p",".","."],[".",".",".",".",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：0
解释：
象阻止了车捕获任何卒。

示例 3：
![棋盘  3](https://upload-images.jianshu.io/upload_images/16639143-345623ffff6ddb1f.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：[[".",".",".",".",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".","p",".",".",".","."],["p","p",".","R",".","p","B","."],[".",".",".",".",".",".",".","."],[".",".",".","B",".",".",".","."],[".",".",".","p",".",".",".","."],[".",".",".",".",".",".",".","."]]
输出：3
解释： 
车可以捕获位置 b5，d6 和 f5 的卒。
 
__提示：__

board.length == board[i].length == 8
board[i][j] 可以是 'R'，'.'，'B' 或 'p'
只有一个格子上存在 board[i][j] == 'R'

__思路__:
首先遍历找到车的位置, 然后在车的同一行和同一列找没有被挡住的兵即可
时间复杂度O(1), 空间复杂度O(1), 最多遍历 64个元素

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numRookCaptures(vector<vector<char>>& board) 
    {
        int i = 0, j = 0, result = 0;
        for (int m = 0; m < 8; m++) for (int n = 0; n < 8; n++) if (board[m][n] == 'R')
                {
                    i = m;
                    j = n;
                    break;
                }
        for (int left = j - 1; left > -1; left--)
        {
            if (board[i][left] == 'B') break;
            if (board[i][left] == 'p')
            {
                result++;
                break;
            }
        }
        for (int right = j + 1; right < 8; right++)
        {
            if (board[i][right] == 'B') break;
            if (board[i][right] == 'p')
            {
                result++;
                break;
            }
        }
        for (int up = i - 1; up > -1; up--)
        {
            if (board[up][j] == 'B') break;
            if (board[up][j] == 'p')
            {
                result++;
                break;
            }
        }
        for (int down = i + 1; down < 8; down++)
        {
            if (board[down][j] == 'B') break;
            if (board[down][j] == 'p')
            {
                result++;
                break;
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int numRookCaptures(char[][] board) {
        int i = 0, j = 0, result = 0;
        for (int m = 0; m < 8; m++) for (int n = 0; n < 8; n++) if (board[m][n] == 'R') {
                    i = m;
                    j = n;
                    break;
                }
        for (int left = j - 1; left > -1; left--) {
            if (board[i][left] == 'B') break;
            if (board[i][left] == 'p') {
                result++;
                break;
            }
        }
        for (int right = j + 1; right < 8; right++) {
            if (board[i][right] == 'B') break;
            if (board[i][right] == 'p') {
                result++;
                break;
            }
        }
        for (int up = i - 1; up > -1; up--) {
            if (board[up][j] == 'B') break;
            if (board[up][j] == 'p') {
                result++;
                break;
            }
        }
        for (int down = i + 1; down < 8; down++) {
            if (board[down][j] == 'B') break;
            if (board[down][j] == 'p') {
                result++;
                break;
            }
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def numRookCaptures(self, board: List[List[str]]) -> int:
        result, i, j = 0, 0, 0
        for m in range(8):
            for n in range(8):
                if board[m][n] == 'R':
                    i, j = m, n
                    break
        for m in range(i - 1, -1, -1):
            if board[m][j] == 'p':
                result += 1
                break
            elif board[m][j] == 'B':
                break
        for m in range(i + 1, 8):
            if board[m][j] == 'p':
                result += 1
                break
            elif board[m][j] == 'B':
                break
        for n in range(j + 1, 8):
            if board[i][n] == 'p':
                result += 1
                break
            elif board[i][n] == 'B':
                break
        for n in range(j - 1, -1, -1):
            if board[i][n] == 'p':
                result += 1
                break
            elif board[i][n] == 'B':
                break
        return result
```