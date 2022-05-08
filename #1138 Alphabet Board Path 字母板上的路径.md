# 1138 Alphabet Board Path 字母板上的路径

__Description__:
On an alphabet board, we start at position (0, 0), corresponding to character board[0][0].

Here, board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"], as shown in the diagram below.

![azboard](https://assets.leetcode.com/uploads/2019/07/28/azboard.png)

We may make the following moves:

'U' moves our position up one row, if the position exists on the board;
'D' moves our position down one row, if the position exists on the board;
'L' moves our position left one column, if the position exists on the board;
'R' moves our position right one column, if the position exists on the board;
'!' adds the character board[r][c] at our current position (r, c) to the answer.
(Here, the only positions that exist on the board are positions with letters on them.)

Return a sequence of moves that makes our answer equal to target in the minimum number of moves.  You may return any path that does so.

__Example:__

Example 1:

Input: target = "leet"
Output: "DDR!UURRR!!DDD!"

Example 2:

Input: target = "code"
Output: "RR!DDRR!UUL!R!"

__Constraints:__

1 <= target.length <= 100
target consists only of English lowercase letters.

__题目描述__:
我们从一块字母板上的位置 (0, 0) 出发，该坐标对应的字符为 board[0][0]。

在本题里，字母板为board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]，如下所示。

![字母板](https://assets.leetcode.com/uploads/2019/07/28/azboard.png)

我们可以按下面的指令规则行动：

如果方格存在，'U' 意味着将我们的位置上移一行；
如果方格存在，'D' 意味着将我们的位置下移一行；
如果方格存在，'L' 意味着将我们的位置左移一列；
如果方格存在，'R' 意味着将我们的位置右移一列；
'!' 会把在我们当前位置 (r, c) 的字符 board[r][c] 添加到答案中。
（注意，字母板上只存在有字母的位置。）

返回指令序列，用最小的行动次数让答案和目标 target 相同。你可以返回任何达成目标的路径。

__示例 :__

示例 1：

输入：target = "leet"
输出："DDR!UURRR!!DDD!"

示例 2：

输入：target = "code"
输出："RR!DDRR!UUL!R!"

__提示:__

1 <= target.length <= 100
target 仅含有小写英文字母。

__思路__:

模拟
从上一个字母出发用取余和商的方式找到需要移动的次数
由于 'z' 位于比较特殊的位置, 优先考虑左上右下的方式进行移动
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution
{
public:
    string alphabetBoardPath(string target) 
    {
        int x = 0, y = 0;
        string result;
        for (const auto& c : target) 
        {
            int row = (c - 'a') / 5, col = (c - 'a') % 5;
            while (y and --y > col) result += 'L';
            while (row > x++) result += 'D';
            while (x and --x > row) result += 'U';
            while (col > y++) result += 'R';
            result += '!';
        } 
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String alphabetBoardPath(String target) {
        int x = 0, y = 0;
        StringBuilder result = new StringBuilder();
        for (char c : target.toCharArray()) {
            int row = (c - 'a') / 5, col = (c - 'a') % 5;
            while (y > 0 && --y > col) result.append('L');
            while (row > x++) result.append('D');
            while (x > 0 && --x > row) result.append('U');
            while (col > y++) result.append('R');
            result.append('!');
        } 
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def alphabetBoardPath(self, target: str) -> str:
        x, y, result = 0, 0, ""
        for c in target:
            row, col, x, y = x, y, *divmod(ord(c) - ord('a'), 5)
            result += 'L' * (col - y) + 'D' * (x - row) + 'U' * (row - x) + 'R' * (y - col) + '!'
        return result
```
