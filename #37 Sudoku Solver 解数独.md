__Description__:
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
Empty cells are indicated by the character '.'.

![A sudoku puzzle...](https://upload-images.jianshu.io/upload_images/16639143-54e5fa13d98357c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![...and its solution numbers marked in red.](https://upload-images.jianshu.io/upload_images/16639143-836093cfecce8b6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Note:__

The given board contain only digits 1-9 and the character '.'.
You may assume that the given Sudoku puzzle will have a single unique solution.
The given board size is always 9x9.

__题目描述__:
编写一个程序，通过已填充的空格来解决数独问题。

一个数独的解法需遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
空白格用 '.' 表示。

![一个数独。](https://upload-images.jianshu.io/upload_images/16639143-891537358cb8473c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![答案被标成红色。](https://upload-images.jianshu.io/upload_images/16639143-0b1ca721a5ad4a7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__说明:__

给定的数独序列只包含数字 1-9 和字符 '.' 。
你可以假设给定的数独只有唯一解。
给定数独永远是 9x9 形式的。

__思路__:
回溯法
1. 初始化条件, 在这里是给出题目中已经存在的数字
2. 做选择, 从 1-9中选出满足 row, col, box的进入下一层函数选择
3. 撤销选择
4. 满足条件的加入结果
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void solveSudoku(vector<vector<char>>& board) 
    {
        bool row[9][9]{false}, col[9][9]{false}, box[9][9]{false};
        for (int i = 0; i < 9; I++)
        {
            for (int j = 0; j < 9; j++)
            {
                int num = board[i][j] - '1';
                if (num > -1)
                {
                    row[i][num] = true;
                    col[j][num] = true;
                    box[i / 3 * 3 + j / 3][num] = true;
                }
            }
        }
        helper(board, row, col, box, 0, 0);
    }
private:
    bool helper(vector<vector<char>>& board, bool row[9][9], bool col[9][9], bool box[9][9], int i, int j)
    {
        if (j == 9)
        {
            j = 0;
            ++I;
            if (i == 9) return true;
        }
        if (board[i][j] == '.')
        {
            for (int num = 0; num < 9; num++)
            {
                bool valid = !(row[i][num] || col[j][num] || box[i / 3 * 3 + j / 3][num]);
                if (valid)
                {
                    row[i][num] = true;
                    col[j][num] = true;
                    box[i / 3 * 3 + j / 3][num] = true;
                    board[i][j] = (char)(num + '1');
                    if (helper(board, row, col, box, i, j + 1)) return true;
                    board[i][j] = '.';
                    row[i][num] = false;
                    col[j][num] = false;
                    box[i / 3 * 3 + j / 3][num] = false;
                }
            }
        }
        else return helper(board, row, col, box, i, j + 1);
        return false;
    }
};
```

__Java__:
```Java
class Solution {
    public void solveSudoku(char[][] board) {
        boolean row[][] = new boolean[9][9], col[][] = new boolean[9][9], box[][] = new boolean[9][9];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                int num = board[i][j] - '1';
                if (num > -1) {
                    row[i][num] = true;
                    col[j][num] = true;
                    box[i / 3 * 3 + j / 3][num] = true;
                }
            }
        }
        helper(board, row, col, box, 0, 0);
    }
    
    private boolean helper(char[][]board, boolean[][]row, boolean[][]col, boolean[][]box, int i, int j){
        if (j == 9) {
            j = 0;
            ++I;
            if (i == 9) return true;
        }
        if (board[i][j] == '.') {
            for (int num = 0; num < 9; num++) {
                boolean valid = !(row[i][num] || col[j][num] || box[i / 3 * 3 + j / 3][num]);
                if (valid) {
                    row[i][num] = true;
                    col[j][num] = true;
                    box[i / 3 * 3 + j / 3][num] = true;
                    board[i][j] = (char)('1' + num);
                    if (helper(board, row, col, box, i, j + 1)) return true;
                    board[i][j] = '.';
                    row[i][num] = false;
                    col[j][num] = false;
                    box[i / 3 * 3 + j / 3][num] = false;
                }
            }
        } else return helper(board, row, col, box, i, j + 1);
        return false;
    }
}
```

__Python__:
```Python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        row, col, box = [[False for _ in range(9)] for _ in range(9)], [[False for _ in range(9)] for _ in range(9)], [[False for _ in range(9)] for _ in range(9)]
        for i in range(9):
            for j in range(9):
                num = ord(board[i][j]) - ord('1')
                if num > -1:
                    row[i][num], col[j][num], box[i // 3 * 3 + j // 3][num] = True, True, True
        def trackback(board: List[List[str]], row: List[List[bool]], col: List[List[bool]], box: List[List[bool]], i: int, j: int) -> bool:
            if j == 9:
                j = 0
                i += 1
                if i == 9:
                    return True
            if board[i][j] == '.':
                for num in range(9):
                    valid = not (row[i][num] or col[j][num] or box[i // 3 * 3 + j // 3][num])
                    if valid:
                        row[i][num], col[j][num], box[i // 3 * 3 + j // 3][num], board[i][j] = True, True, True, chr(num + ord('1'))
                        if trackback(board, row, col, box, i, j + 1):
                            return True
                        board[i][j], row[i][num], col[j][num], box[i // 3 * 3 + j // 3][num] = '.', False, False, False
            else:
                return trackback(board, row, col, box, i, j + 1)
            return False
        trackback(board, row, col, box, 0, 0)
```