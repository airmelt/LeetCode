# 36 Valid Sudoku 有效的数独

__Description__:
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

Each row must contain the digits 1-9 without repetition.
Each column must contain the digits 1-9 without repetition.
Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

![Valid Sudoku](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

__Example:__

Example 1:

Input:

```text
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

Output: true

Example 2:

Input:

```text
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.

__Note:__

A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
The given board contain only digits 1-9 and the character '.'.
The given board size is always 9x9.

__题目描述__:
判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

![有效的数独](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 '.' 表示。

__示例 :__

示例 1:

输入:

```text
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

输出: true

示例 2:

输入:

```text
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。

__说明:__

一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。
给定数独序列只包含数字 1-9 和字符 '.' 。
给定数独永远是 9x9 形式的。

__思路__:

1. 3次遍历逐行, 逐列, 和对每个盒子都进行一次遍历
2. 方法 1的三次遍历可以用一次遍历完成, row的下标对应最外层遍历下标 i, col的下标对应内层遍历下标 j, 盒子的下标可以用 (i / 3) \* 3 + j / 3找到
3. 由于下标最大为 9, 可以使用位运算代替建立数组, 加快运算速度, 用二进制上的 1表示该位已经访问过
时间复杂度O(1), 空间复杂度O(1), 最多遍历 81(\* 3)次, 最多建立 9 \* 9数组 3个

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isValidSudoku(vector<vector<char>>& board) 
    {
        int row[9][9]{0}, col[9][9]{0}, box[9][9]{0};
        for (int i = 0; i < 9; i++) 
        {
            for (int j = 0; j < 9; j++) 
            {
                if (board[i][j] != '.') 
                {
                    int num = board[i][j] - '1', box_index = i / 3 * 3 + j / 3;
                    if (row[i][num] or col[j][num] or box[box_index][num]) return false;
                    row[i][num] = 1;
                    col[j][num] = 1;
                    box[box_index][num] = 1;
                }
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        for (int i = 0; i < 9; i++) {
            int row = 0, col = 0, box = 0;
            for (int j = 0; j < 9; j++) {
                int r = board[i][j] - '0', c = board[j][i] - '0', b = board[(i / 3) * 3 + j / 3][(i % 3) * 3 + j % 3] - '0';
                if (r > 0) row = helper(r, row);
                if (c > 0) col = helper(c, col);
                if (b > 0) box = helper(b, box);
                if (row == -1 || col == -1 || box == -1) return false;
            }
        }
        return true;
    }

    private int helper(int n, int val) {
        return ((val >> n) & 1) == 1 ? -1 : val ^ (1 << n);
    }
}
```

__Python__:

```Python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        def helper(n: int, val: int):
            return -1 if ((val >> n) & 1) == 1 else val ^ (1 << n)
        for i in range(9):
            row = col = box = 0
            for j in range(9):
                r, c, b = ord(board[i][j]) - 48, ord(board[j][i]) - 48, ord(board[(i // 3) * 3 + j // 3][(i % 3) * 3 + j % 3]) - 48
                row, col, box = helper(r, row) if r > 0 else row, helper(c, col) if c > 0 else col, helper(b, box) if b > 0 else box
                if row == -1 or col == -1 or box == -1:
                    return False
        return True
```
