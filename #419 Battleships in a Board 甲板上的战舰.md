# 419 Battleships in a Board 甲板上的战舰

__Description__:
Given an 2D board, count how many battleships are in it. The battleships are represented with 'X's, empty slots are represented with '.'s. You may assume the following rules:
You receive a valid board, made of only battleships or empty slots.
Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape 1xN (1 row, N columns) or Nx1 (N rows, 1 column), where N can be of any size.
At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.

__Example:__

```C
X..X
...X
...X
```

In the above board there are 2 battleships.
Invalid Example:

```C
...X
XXXX
...X
```

This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

__Follow up:__

Could you do it in one-pass, using only O(1) extra memory and without modifying the value of the board?

__题目描述__:
给定一个二维的甲板， 请计算其中有多少艘战舰。 战舰用 'X'表示，空位用 '.'表示。 你需要遵守以下规则：

给你一个有效的甲板，仅由战舰或者空位组成。
战舰只能水平或者垂直放置。换句话说,战舰只能由 1xN (1 行, N 列)组成，或者 Nx1 (N 行, 1 列)组成，其中N可以是任意大小。
两艘战舰之间至少有一个水平或垂直的空位分隔 - 即没有相邻的战舰。

__示例 :__

```C
X..X
...X
...X
```

在上面的甲板中有2艘战舰。

无效样例 :

```C
...X
XXXX
...X
```

你不会收到这样的无效甲板 - 因为战舰之间至少会有一个空位将它们分开。

__进阶:__

你可以用一次扫描算法，只使用O(1)额外空间，并且不修改甲板的值来解决这个问题吗？

__思路__:

只用检查战舰的左上角, 这时左边为边界或者 '.', 上边为边界或者 '.'
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countBattleships(vector<vector<char>>& board) 
    {
        int result = 0;
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[0].size(); j++) if (board[i][j] == 'X' && (i == 0 || board[i - 1][j] == '.') && (j == 0 || board[i][j - 1] == '.')) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countBattleships(char[][] board) {
        int result = 0;
        for (int i = 0; i < board.length; i++) for (int j = 0; j < board[0].length; j++) if (board[i][j] == 'X' && (i == 0 || board[i - 1][j] == '.') && (j == 0 || board[i][j - 1] == '.')) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countBattleships(self, board: List[List[str]]) -> int:
        return sum(1 for i in range(len(board)) for j in range(len(board[0])) if board[i][j] == 'X' and (not i or board[i - 1][j] == '.') and (not j or board[i][j - 1] == '.'))
```
