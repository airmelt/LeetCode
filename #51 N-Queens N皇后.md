# 51 N-Queens N皇后

__Description__:
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

![N-Queens](https://upload-images.jianshu.io/upload_images/16639143-217a2ebb9e01005d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

__Example:__

Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.

__题目描述__:
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

![N皇后](https://upload-images.jianshu.io/upload_images/16639143-e8d82eafb426fa65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图为 8 皇后问题的一种解法。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

__示例 :__

输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。

__提示：__

皇后，是国际象棋中的棋子，意味着国王的妻子。皇后只做一件事，那就是“吃子”。当她遇见可以吃的棋子时，就迅速冲上去吃掉棋子。当然，她横、竖、斜都可走一或七步，可进可退。（引用自 百度百科 - 皇后 ）

__思路__:
回溯法

1. 判断是否有效
2. 做选择
3. 回溯
4. 撤销选择
时间复杂度O(n!), 空间复杂度O(n ^ 2)
这里的空间复杂度可以简化到 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> solveNQueens(int n) 
    {
        vector<string> board(n, string(n, '.'));
        backtrack(board, 0);
        return result;
    }
private:
    vector<vector<string>> result;
    
    void backtrack(vector<string> board, int row)
    {
        if (row == board.size())
        {
            result.push_back(board);
            return;
        }
        for (int col = 0; col < board.size(); col++)
        {
            if (!is_valid(board, row, col)) continue;
            board[row][col] = 'Q';
            backtrack(board, row + 1);
            board[row][col] = '.';
        }
    }
    
    bool is_valid(vector<string> board, int row, int col)
    {
        for (int i = 0; i < board.size(); i++) if (board[i][col] == 'Q') return false;
        for (int i = row - 1, j = col - 1; i > -1 && j > -1; i--, j--) if (board[i][j] == 'Q') return false;
        for (int i = row - 1, j = col + 1; i > -1 && j < board.size(); i--, j++) if (board[i][j] == 'Q') return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    private List<List<String>> result = new ArrayList<>();
    
    public List<List<String>> solveNQueens(int n) {
        char board[][] = new char[n][n];
        backtrack(board, 0);
        return result;
    }
    
    private void backtrack(char[][] board, int row) {
        if (row == board.length) {
            List<String> list = new ArrayList<>(row);
            for (int i = 0; i < row; i++) list.add(new String(board[i]));
            result.add(list);
            return;
        }
        Arrays.fill(board[row], '.');
        for (int col = 0; col < board.length; col++) {
            if (!isValid(board, row, col)) continue;
            board[row][col] = 'Q';
            backtrack(board, row + 1);
            board[row][col] = '.';
        }
    }
    
    private boolean isValid(char[][] board, int row, int col) {
        for (int i = 0; i < board.length; i++) if (board[i][col] == 'Q') return false;
        for (int i = row - 1, j = col - 1; i > -1 && j > -1; i--, j--) if (board[i][j] == 'Q') return false;
        for (int i = row - 1, j = col + 1; i > -1 && j < board.length; i--, j++) if (board[i][j] == 'Q') return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        result, board = [], [['.' for _ in range(n)] for __ in range(n)]
        def is_valid(board: List[List[str]], row: int, col: int) -> bool:
            for i in range(n):
                if board[i][col] == 'Q':
                    return False
            i, j = row - 1, col - 1
            while i > -1 and j > -1:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1
            i, j = row - 1, col + 1
            while i > -1 and j < n:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j += 1
            return True
        def backtrack(board: List[List[str]], row: int) -> None:
            if row == n:
                temp = []
                for i in range(n):
                    temp.append(''.join(board[i]))
                result.append(temp[:])
                return
            for col in range(n):
                if not is_valid(board, row, col):
                    continue
                board[row][col] = 'Q'
                backtrack(board, row + 1)
                board[row][col] = '.'
        backtrack(board, 0)
        return result
```
