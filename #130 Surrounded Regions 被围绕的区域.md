__Description__:
Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

__Example:__
```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```
Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

__题目描述__:
给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

__示例 :__
```
X X X X
X O O X
X X O X
X O X X
```
运行你的函数后，矩阵变为：
```
X X X X
X X X X
X X X X
X O X X
```
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

__思路__:
DFS
先将与边界相连的 'O'修改为 '-'
再将所有剩下的 'O'修改为 'X'
最后将 '-'还原
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void solve(vector<vector<char>>& board) 
    {
        if (board.empty()) return;
        int row = board.size(), col = board[0].size();
        for (int i = 0; i < row; i++) 
        {
            helper(board, i, 0, row, col);
            helper(board, i, col - 1, row, col);
        }
        for (int j = 0; j < col; j++) 
        {
            helper(board, 0, j, row, col);
            helper(board, row - 1, j, row, col);
        }
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) board[i][j] = board[i][j] == 'O' ? 'X' : (board[i][j] == '-' ? 'O' : 'X');
    }
private:
    void helper(vector<vector<char>>& board, int i, int j, int row, int col) 
    {
        if (i < 0 || j < 0 || i >= row || j >= col || board[i][j] != 'O') return;
        board[i][j] = '-';
        helper(board, i - 1, j, row, col);
        helper(board, i + 1, j, row, col);
        helper(board, i, j - 1, row, col);
        helper(board, i, j + 1, row, col);
    }
};
```

__Java__:
```Java
class Solution {
    public void solve(char[][] board) {
        if (board == null || board.length == 0) return;
        int row = board.length, col = board[0].length;
        for (int i = 0; i < row; i++) {
            helper(board, i, 0, row, col);
            helper(board, i, col - 1, row, col);
        }
        for (int j = 0; j < col; j++) {
            helper(board, 0, j, row, col);
            helper(board, row - 1, j, row, col);
        }
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) board[i][j] = board[i][j] == 'O' ? 'X' : (board[i][j] == '-' ? 'O' : 'X');
    }
    private void helper(char[][] board, int i, int j, int row, int col) {
        if (i < 0 || j < 0 || i >= row || j >= col || board[i][j] != 'O') return;
        board[i][j] = '-';
        helper(board, i - 1, j, row, col);
        helper(board, i + 1, j, row, col);
        helper(board, i, j - 1, row, col);
        helper(board, i, j + 1, row, col);
    }
}
```

__Python__:
```Python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board or not board[0]:
            return
        board_row, board_col = len(board), len(board[0])
        
        def helper(board: List[List[str]], i: int, j: int, row: int=board_row, col: int=board_col) -> None:
            if i < 0 or j < 0 or i >= row or j >= col or board[i][j] != 'O':
                return;
            board[i][j] = '-'
            helper(board, i - 1, j)
            helper(board, i + 1, j)
            helper(board, i, j - 1)
            helper(board, i, j + 1)
            
        for i in range(board_row):
            helper(board, i, 0)
            helper(board, i, board_col - 1)
        for j in range(board_col):
            helper(board, 0, j)
            helper(board, board_row - 1, j)
        for i in range(board_row):
            for j in range(board_col):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                if board[i][j] == '-':
                    board[i][j] = 'O'
```