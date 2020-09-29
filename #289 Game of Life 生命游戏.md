__Description__:
According to the [Wikipedia's article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

Any live cell with fewer than two live neighbors dies, as if caused by under-population.
Any live cell with two or three live neighbors lives on to the next generation.
Any live cell with more than three live neighbors dies, as if by over-population..
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

__Example:__

Input: 
```
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
```
Output: 
```
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

__Follow up:__

Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

__题目描述__:
根据百度百科 ，[生命游戏](https://baike.baidu.com/item/%E7%94%9F%E5%91%BD%E6%B8%B8%E6%88%8F/2926434?fr=aladdin)，简称为生命，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：1 即为活细胞（live），或 0 即为死细胞（dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
如果死细胞周围正好有三个活细胞，则该位置死细胞复活；
根据当前状态，写一个函数来计算面板上所有细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

__示例 :__

输入： 
```
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
```
输出：
```
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

__进阶：__
你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？

__思路__:
先记录周围 8个位置的 1的个数, 这里 count需要和 1进行与运算(&), 因为用最后一位记录存活状态
如果活细胞周围有 2个或者 3个活细胞, 则将该位置修改为 3(0b11),
如果死细胞周围有 3个活细胞, 则将该位置修改为 2(0b10),
这样利用二进制最后 1位用来记录存活状态, 用倒数第二位记录下一步的存活状态, 就可以在原数组上修改
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:
class Solution 
{
public:
    void gameOfLife(vector<vector<int>>& board) 
    {
        if (board.empty()) return;
        vector<int> dx{0, 0, 1, -1, 1, 1, -1, -1}, dy{1, -1, 0, 0, 1, -1, 1, -1};; 
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[0].size(); j++) 
        {
            int count = 0;
            for (int k = 0; k < 8; k++) 
            {
                int x = i + dx[k], y = j + dy[k];
                if (x < 0 or x > board.size() - 1 or y < 0 or y > board[0].size() - 1) continue;
                count += board[x][y] & 1;
            }
            if ((board[i][j] & 1) > 0 and count > 1 and count < 4) board[i][j] = 3;
            else if (count == 3) board[i][j] = 2;
        }
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[0].size(); j++) board[i][j] >>= 1;
    }
};
```

__Java__:
```Java
class Solution {
    public void gameOfLife(int[][] board) {
        if (board.length == 0) return;
        final int dx[] = {0, 0, 1, -1, 1, 1, -1, -1}, dy[] = {1, -1, 0, 0, 1, -1, 1, -1};; 
        for (int i = 0; i < board.length; i++) for (int j = 0; j < board[0].length; j++) {
            int count = 0;
            for (int k = 0; k < 8; k++) {
                int x = i + dx[k], y = j + dy[k];
                if (x < 0 || x > board.length - 1 || y < 0 || y > board[0].length - 1) continue;
                count += board[x][y] & 1;
            }
            if ((board[i][j] & 1) > 0 && count > 1 && count < 4) board[i][j] = 3;
            else if (count == 3) board[i][j] = 2;
        }
        for (int i = 0; i < board.length; i++) for (int j = 0; j < board[0].length; j++) board[i][j] >>= 1;
    }
}
```

__Python__:
```Python
class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board:
            return
        dx, dy = [0, 0, 1, -1, 1, 1, -1, -1], [1, -1, 0, 0, 1, -1, 1, -1]
        for i in range(len(board)):
            for j in range(len(board[0])):
                count = 0
                for k in range(8):
                    x, y = i + dx[k], j + dy[k]
                    if (x < 0 or x > len(board) - 1 or y < 0 or y > len(board[0]) - 1):
                        continue
                    count += (board[x][y] & 1)
                if board[i][j] & 1:
                    if count > 1 and count < 4:
                        board[i][j] = 3
                    elif count == 3:
                        board[i][j] = 2
        for i in range(len(board)):
            for j in range(len(board[0])):
                board[i][j] >>= 1
```