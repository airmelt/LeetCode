# 529 Minesweeper 扫雷游戏

__Description__:
Let's play the minesweeper game (Wikipedia, online game)!

You are given a 2D char matrix representing the game board. 'M' represents an unrevealed mine, 'E' represents an unrevealed empty square, 'B' represents a revealed blank square that has no adjacent (above, below, left, right, and all 4 diagonals) mines, digit ('1' to '8') represents how many mines are adjacent to this revealed square, and finally 'X' represents a revealed mine.

Now given the next click position (row and column indices) among all the unrevealed squares ('M' or 'E'), return the board after revealing this position according to the following rules:

If a mine ('M') is revealed, then the game is over - change it to 'X'.
If an empty square ('E') with no adjacent mines is revealed, then change it to revealed blank ('B') and all of its adjacent unrevealed squares should be revealed recursively.
If an empty square ('E') with at least one adjacent mine is revealed, then change it to a digit ('1' to '8') representing the number of adjacent mines.
Return the board when no more squares will be revealed.

__Example:__

Example 1:

Input:

```text
[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]
```

Click : [3,0]

Output:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

Explanation:

![minesweeper_example_1](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_1.png)

Example 2:

Input:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

Click : [1,2]

Output:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

Explanation:
![minesweeper_example_2](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_2.png)

__Note:__

The range of the input matrix's height and width is [1,50].
The click position will only be an unrevealed square ('M' or 'E'), which also means the input board contains at least one clickable square.
The input board won't be a stage when game is over (some mines have been revealed).
For simplicity, not mentioned rules should be ignored in this problem. For example, you don't need to reveal all the unrevealed mines when the game is over, consider any cases that you will win the game or flag any squares.

__题目描述__:
让我们一起来玩扫雷游戏！

给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。

现在给出在所有未挖出的方块中（'M'或者'E'）的下一个点击位置（行和列索引），根据以下规则，返回相应位置被点击后对应的面板：

如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X'。
如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的未挖出方块都应该被递归地揭露。
如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量。
如果在此次点击中，若无更多方块可被揭露，则返回面板。

__示例 :__

示例 1：

输入:

```text
[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]
```

Click : [3,0]

输出:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

解释:

![minesweeper_example_1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/minesweeper_example_1.png)

示例 2：

输入:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

Click : [1,2]

输出:

```text
[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

解释:

![minesweeper_example_2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/minesweeper_example_2.png)

__注意:__

输入矩阵的宽和高的范围为 [1,50]。
点击的位置只能是未被挖出的方块 ('M' 或者 'E')，这也意味着面板至少包含一个可点击的方块。
输入面板不会是游戏结束的状态（即有地雷已被挖出）。
简单起见，未提及的规则在这个问题中可被忽略。例如，当游戏结束时你不需要挖出所有地雷，考虑所有你可能赢得游戏或标记方块的情况。

__思路__:

dfs
如果搜索到 'M', 表示搜索到雷
如果搜索到 'B', 向 8 个方向再递归搜索, 直到超过边界
搜索周围有几个雷, 将这个点标记为个数, 停止搜索
时间复杂度 O(mn), 空间复杂度 O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<int> dx = {-1, -1, -1, 0, 0, 1, 1, 1}, dy = {-1, 0, 1, -1, 1, -1, 0, 1};
    int m, n;
    
    int count(vector<vector<char>>& board, int x, int y) 
    {
        if (x < 0 or x >= m or y < 0 or y >= n) return 0;
        if (board[x][y] == 'M')  return 1;
        return 0;
    }
    
    void dfs(vector<vector<char>>& board, int x, int y) 
    {
        if (x < 0 or x >= m or y < 0 or y >= n or board[x][y] == 'B') return;
        int c = 0;
        for (int i = 0; i < 8; i++) c += count(board, x + dx[i], y + dy[i]);
        if (c != 0) board[x][y] = (char)(c + '0');
        else 
        {
            board[x][y] = 'B';
            for (int i = 0; i < 8; i++) dfs(board, x + dx[i], y + dy[i]);
        }
    }
public:
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) 
    {
        m = board.size();
        n = board[0].size();
        int x = click.front(), y = click.back();
        if (board[x][y] == 'M') board[x][y] = 'X';
        else dfs(board, x, y);
        return board;
    }
};
```

__Java__:

```Java
class Solution {
    private int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
    private int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};
    private int m, n;
    
    public char[][] updateBoard(char[][] board, int[] click) {
        int x = click[0], y = click[1];
        m = board.length;
        n = board[0].length;
        if (board[x][y] == 'M') board[x][y] = 'X';
        else dfs(board, x, y);
        return board;
    }
    
    private int count(char[][] board, int x, int y) {
        if (x < 0 || x >= m || y < 0 || y >= n) return 0;
        if (board[x][y] == 'M')  return 1;
        return 0;
    }
    
    private void dfs(char[][] board, int x, int y) {
        if (x < 0 || x >= m || y < 0 || y >= n || board[x][y] == 'B') return;
        int c = 0;
        for (int i = 0; i < 8; i++) c += count(board, x + dx[i], y + dy[i]);
        if (c != 0) board[x][y] = (char)(c + '0');
        else {
            board[x][y] = 'B';
            for (int i = 0; i < 8; i++) dfs(board, x + dx[i], y + dy[i]);
        }
    }
}
```

__Python__:

```Python
class Solution:
    def updateBoard(self, board, click):
        d, m, n, (x, y) = [(-1, 0), (0, -1), (0, 1), (-1, -1), (1, 0), (1, 1), (-1, 1), (1, -1)], len(board), len(board[0]), click
        def count_num(x, y):
            if x < 0 or x >= m or y < 0 or y >= n:
                return 0
            if board[x][y] == 'M':
                return 1
            else:
                return 0
        if x < 0 or x >= m or y < 0 or y >= n or board[x][y] == 'B':
            return
        if board[x][y] == 'M':
            board[x][y] = 'X'
        if board[x][y] == 'E':
            count = sum(count_num(x + dx, y + dy) for dx, dy in d)
            if count != 0:
                board[x][y] = str(count)
            else:
                board[x][y] = 'B'
                for dx, dy in d:
                    self.updateBoard(board, (x + dx, y + dy))     
        return board
```
