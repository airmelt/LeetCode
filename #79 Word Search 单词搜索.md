__Description__:
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

__Example:__

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.

__题目描述__:
给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

__示例 :__

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false

__提示：__

board 和 word 中只包含大写和小写英文字母。
1 <= board.length <= 200
1 <= board[i].length <= 200
1 <= word.length <= 10^3

__思路__:
回溯法
遍历 board从第一个 board[i][j] = word[0]的坐标开始向 4个方向查找
时间复杂度O(m ^ 2 * n ^ 2), 空间复杂度O(mn)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool exist(vector<vector<char>>& board, string word) 
    {
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[0].size(); j++) if (board[i][j] == word[0]) if (backtrack(board, word, i, j, board.size(), board[0].size(), 0, word.size())) return true;
        return false;
    }
private:
    bool backtrack(vector<vector<char>>& board, const string& word, const int i, const int j, const int n, const int m, int size, const int l) 
    {
        char temp = board[i][j];
        board[i][j] = '0';
        if (l == ++size) return true;
        if (i > 0 && board[i - 1][j] == word[size]) if (backtrack(board, word, i - 1, j, n, m, size, l)) return true;
        if (i < n - 1 && board[i + 1][j] == word[size]) if (backtrack(board, word, i + 1, j, n, m, size, l)) return true;
        if (j > 0 && board[i][j - 1] == word[size]) if (backtrack(board, word, i, j - 1, n, m, size, l)) return true;
        if (j < m - 1 && board[i][j + 1] == word[size]) if (backtrack(board, word, i, j + 1, n, m, size, l)) return true;
        board[i][j] = temp;
        return false;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++) for (int j = 0; j < board[0].length; j++) if (board[i][j] == word.charAt(0)) if (backtrack(board, word, i, j, board.length, board[0].length, 0)) return true;
        return false;
    }
    private boolean backtrack(char[][] board, String word, int i, int j, int n, int m, int size) {
        char temp = board[i][j];
        board[i][j] = '0';
        if (word.length() == ++size) return true;
        if (i > 0 && board[i - 1][j] == word.charAt(size)) if (backtrack(board, word, i - 1, j, board.length, board[0].length, size)) return true;
        if (i < n - 1 && board[i + 1][j] == word.charAt(size)) if (backtrack(board, word, i + 1, j, board.length, board[0].length, size)) return true;
        if (j > 0 && board[i][j - 1] == word.charAt(size)) if (backtrack(board, word, i, j - 1, board.length, board[0].length, size)) return true;
        if (j < m - 1 && board[i][j + 1] == word.charAt(size)) if (backtrack(board, word, i, j + 1, board.length, board[0].length, size)) return true;
        board[i][j] = temp;
        return false;
    }
}
```

__Python__:
```Python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        if not board or not word:
            return False
        visited, n, m = [[False for _ in range(len(board[0]))] for _ in range(len(board))], len(board), len(board[0])
        def backtrack(size: int, n: int, m: int, x: int, y: int, visited: List[List[bool]], board: List[List[str]], word: str) -> bool:
            if size == len(word):
                return True
            if x == n or x < 0 or y < 0 or y == m or board[x][y] != word[size]:
                return False
            if not visited[x][y]:
                visited[x][y] = True
                if backtrack(size + 1, n, m, x + 1, y, visited, board, word) or backtrack(size + 1, n, m, x - 1, y, visited, board, word) or backtrack(size + 1, n, m, x, y + 1, visited, board, word) or backtrack(size + 1, n, m, x, y - 1, visited, board, word):
                    return True
                visited[x][y] = False
            return False
        return any(any(backtrack(0, n, m, i, j, visited, board, word) for j in range(m)) for i in range(n))
```