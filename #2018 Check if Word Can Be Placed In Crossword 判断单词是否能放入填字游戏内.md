# 2018 Check if Word Can Be Placed In Crossword 判断单词是否能放入填字游戏内

__Description:__

You are given an `m x n` matrix `board`, representing the __current__ state of a crossword puzzle. The crossword contains lowercase English letters (from solved words), `' '` to represent any __empty__ cells, and `'#'` to represent any __blocked__ cells.

A word can be placed horizontally (left to right or right to left) or vertically (top to bottom or bottom to top) in the board if:

- It does not occupy a cell containing the character `'#'`.
- The cell each letter is placed in must either be `' '` (empty) or __match__ the letter already on the `board`.
- There must not be any empty cells `' '` or other lowercase letters __directly left or right__ of the word if the word was placed __horizontally__.
- There must not be any empty cells `' '` or other lowercase letters __directly above or below__ the word if the word was placed __vertically__.

Given a string `word`, return `true` _if_ `word` _can be placed in_ `board`_, or_ `false` ___otherwise___.

__Example:__

Example 1:

![2018-1](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex1-1.png)

```text
Input: board = [["#", " ", "#"], [" ", " ", "#"], ["#", "c", " "]], word = "abc"
Output: true
Explanation: The word "abc" can be placed as shown above (top to bottom).
```

Example 2:

![2018-2](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex2-1.png)

```text
Input: board = [[" ", "#", "a"], [" ", "#", "c"], [" ", "#", "a"]], word = "ac"
Output: false
Explanation: It is impossible to place the word because there will always be a space/letter above or below it.
```

Example 3:

![2018-3](https://assets.leetcode.com/uploads/2021/10/04/crossword-ex3-1.png)

```text
Input: board = [["#", " ", "#"], [" ", " ", "#"], ["#", " ", "c"]], word = "ca"
Output: true
Explanation: The word "ca" can be placed as shown above (right to left).
```

__Constraints:__

- `m == board.length`
- `n == board[i].length`
- `1 <= m * n <= 2 * 10 ^ 5`
- `board[i][j]` will be `' '`, `'#'`, or a lowercase English letter.
- `1 <= word.length <= max(m, n)`
- `word` will contain only lowercase English letters.

__题目描述:__

给你一个 `m x n` 的矩阵 `board` ，它代表一个填字游戏 __当前__ 的状态。填字游戏格子中包含小写英文字母（已填入的单词），表示 __空__ 格的 `' '` 和表示 __障碍__ 格子的 `'#'` 。

如果满足以下条件，那么我们可以 水平 （从左到右 或者 从右到左）或 竖直 （从上到下 或者 从下到上）填入一个单词：

- 该单词不占据任何 `'#'` 对应的格子。
- 每个字母对应的格子要么是 `' '` （空格）要么与 `board` 中已有字母 __匹配__ 。
- 如果单词是 __水平__ 放置的，那么该单词左边和右边 __相邻__ 格子不能为 `' '` 或小写英文字母。
- 如果单词是 __竖直__ 放置的，那么该单词上边和下边 __相邻__ 格子不能为 `' '` 或小写英文字母。

给你一个字符串 `word` ，如果 `word` 可以被放入 `board` 中，请你返回 `true` ，否则请返回 `false` 。

__示例:__

示例 1：

![2018-4](https://assets.leetcode.com/uploads/2021/09/18/crossword-1.png)

```text
输入：board = [["#", " ", "#"], [" ", " ", "#"], ["#", "c", " "]], word = "abc"
输出：true
解释：单词 "abc" 可以如上图放置（从上往下）。
```

示例 2：

![2018-5](https://assets.leetcode.com/uploads/2021/09/18/c2.png)

```text
输入：board = [[" ", "#", "a"], [" ", "#", "c"], [" ", "#", "a"]], word = "ac"
输出：false
解释：无法放置单词，因为放置该单词后上方或者下方相邻格会有空格。
```

示例 3：

![2018-6](https://assets.leetcode.com/uploads/2021/09/18/crossword-2.png)

```text
输入：board = [["#", " ", "#"], [" ", " ", "#"], ["#", " ", "c"]], word = "ca"
输出：true
解释：单词 "ca" 可以如上图放置（从右到左）。
```

__提示：__

- `m == board.length`
- `n == board[i].length`
- `1 <= m * n <= 2 * 10 ^ 5`
- `board[i][j]` 可能为 `' '` ， `'#'` 或者一个小写英文字母。
- `1 <= word.length <= max(m, n)`
- `word` 只包含小写英文字母。

__思路:__

```text
DFS
找到成对的 '#' 的位置
分别判断水平和竖直方向是否能放入单词
同时可以从顺序和逆序两个方向放入单词
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    bool placeWordInCrossword(vector<vector<char>>& board, string word) {
        int m = board.size(), n = board.front().size(), t = word.size();
        for (int i = 0, cur = 0; i < m; i++, cur = 0) 
        {
            for (int j = 0; j <= n; j++) 
            {
                if (j < n and board[i][j] != '#') continue;
                if (j - cur == t and (check(board, word, i, j, cur, true, true) or check(board, word, i, j, cur, true, false))) return true;
                else cur = j + 1;
            }
        }
        for (int j = 0, cur = 0; j < n; j++, cur = 0) 
        {
            for (int i = 0; i <= m; i++) 
            {
                if (i < m and board[i][j] != '#') continue;
                if (i - cur == t and (check(board, word, i, j, cur, false, true) or check(board, word, i, j, cur, false, false))) return true;
                else cur = i + 1;
            }
        }
        return false;
    }
private:
    bool check(vector<vector<char>>& board, string& word, int x, int y, int cur, bool is_row, bool reverse) 
    {
        int idx = reverse ? word.size() - 1 : 0;
        if (is_row) {for (int j = cur; j < y; j++, idx += reverse ? -1 : 1) if (board[x][j] != ' ' and board[x][j] != word[idx]) return false;}
        else {for (int i = cur; i < x; i++, idx += reverse ? -1 : 1) if (board[i][y] != ' ' and board[i][y] != word[idx]) return false;}
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean placeWordInCrossword(char[][] board, String word) {
        int m = board.length, n = board[0].length, t = word.length();
        for (int i = 0, cur = 0; i < m; i++, cur = 0) {
            for (int j = 0; j <= n; j++) {
                if (j < n && board[i][j] != '#') continue;
                if (j - cur == t && (check(board, word, i, j, cur, true, true) || check(board, word, i, j, cur, true, false))) return true;
                else cur = j + 1;
            }
        }
        for (int j = 0, cur = 0; j < n; j++, cur = 0) {
            for (int i = 0; i <= m; i++) {
                if (i < m && board[i][j] != '#') continue;
                if (i - cur == t && (check(board, word, i, j, cur, false, true) || check(board, word, i, j, cur, false, false))) return true;
                else cur = i + 1;
            }
        }
        return false;
    }

    private boolean check(char[][] board, String word, int x, int y, int cur, boolean isRow, boolean reverse) {
        int idx = reverse ? word.length() - 1 : 0;
        if (isRow) {for (int j = cur; j < y; j++, idx += reverse ? -1 : 1) if (board[x][j] != ' ' && board[x][j] != word.charAt(idx)) return false;}
        else {for (int i = cur; i < x; i++, idx += reverse ? -1 : 1) if (board[i][y] != ' ' && board[i][y] != word.charAt(idx)) return false;}
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def placeWordInCrossword(self, board: List[List[str]], word: str) -> bool:
        m, n, t = len(board), len(board[0]), len(word)
        for i in range(m):
            cur = 0
            for j in range(n + 1):
                if j < n and board[i][j] != '#':
                    continue
                if j - cur == t and (all(x == ' ' or x == y for x, y in zip(board[i][cur:j], word)) or all(x == ' ' or x == y for x, y in zip(board[i][cur:j], word[::-1]))):
                        return True
                else:
                    cur = j + 1
        for j in range(n):
            cur = 0
            for i in range(m + 1):
                if i < m and board[i][j] != '#':
                    continue
                if i - cur == t and (all(x == ' ' or x == y for x, y in zip((board[s][j] for s in range(cur, i)), word)) or all(x == ' ' or x == y for x, y in zip((board[s][j] for s in range(cur, i)), word[::-1]))):
                    return True
                else:
                    cur = i + 1
        return False
```
