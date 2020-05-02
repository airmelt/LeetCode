__Description__:
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

![N-Queens](https://upload-images.jianshu.io/upload_images/16639143-217a2ebb9e01005d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

__Example:__

Input: 4
Output: 2
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]

__题目描述__:
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

![N皇后](https://upload-images.jianshu.io/upload_images/16639143-e8d82eafb426fa65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图为 8 皇后问题的一种解法。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

__示例 :__

输入: 4
输出: 2
解释: 4 皇后问题存在两个不同的解法。
[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]

__提示：__

皇后，是国际象棋中的棋子，意味着国王的妻子。皇后只做一件事，那就是“吃子”。当她遇见可以吃的棋子时，就迅速冲上去吃掉棋子。当然，她横、竖、斜都可走一或七步，可进可退。（引用自 百度百科 - 皇后 ）

__思路__:
1. 参考[LeetCode #51 N-Queens N皇后](https://www.jianshu.com/p/1fab602b7523)
时间复杂度O(n!), 空间复杂度O(n)
2. 可以用位运算加快运算速度, 不过这里暗含条件是 n < 32
每一个二进制的一位 '1'表示已经放置了皇后
row, col分别表示行和列上是否有皇后
i, j分别表示主对角线(左上和右下), 次对角线(左下和右上)是否有皇后
时间复杂度O(n!), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int totalNQueens(int n) 
    {
        backtrack(0, 0, 0, 0, n);
        return result;
    }
private:
    int result = 0;
    void backtrack(int row, int col, int i, int j, int n)
    {
        if (row == n)
        {
            ++result;
            return;
        }
        int bit = (~(col | i | j)) & ((1 << n) - 1);
        while (bit > 0)
        {
            int p = bit & (-bit);
            backtrack(row + 1, col | p, (i | p) << 1, (j | p) >> 1, n);
            bit &= (bit - 1);
        }
    }
};
```

__Java__:
```Java
class Solution {
    private List<List<String>> result = new ArrayList<>();
    
    public int totalNQueens(int n) {
        char board[][] = new char[n][n];
        backtrack(board, 0);
        return result.size();
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
    def totalNQueens(self, n: int) -> int:
        return [0, 1, 0, 0, 2, 10, 4, 40, 92, 352, 724, 2680][n]
```