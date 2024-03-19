# 2038 Remove Colored Pieces if Both Neighbors are the Same Color 如果相邻两个颜色均相同则删除当前颜色

__Description:__

There are `n` pieces arranged in a line, and each piece is colored either by `'A'` or by `'B'`. You are given a string `colors` of length `n` where `colors[i]` is the color of the `i ^ th` piece.

Alice and Bob are playing a game where they take alternating turns removing pieces from the line. In this game, Alice moves first.

- Alice is only allowed to remove a piece colored `'A'` if __both its neighbors__ are also colored `'A'`. She is __not allowed__ to remove pieces that are colored `'B'`.
- Bob is only allowed to remove a piece colored `'B'` if __both its neighbors__ are also colored `'B'`. He is __not allowed__ to remove pieces that are colored `'A'`.
- Alice and Bob __cannot__ remove pieces from the edge of the line.
- If a player cannot make a move on their turn, that player __loses__ and the other player __wins__.

Assuming Alice and Bob play optimally, return `true` _if Alice wins, or return_ `false` _if Bob wins_.

__Example:__

Example 1:

```text
Input: colors = "AAABABB"
Output: true
Explanation:
AAABABB -> AABABB
Alice moves first.
She removes the second 'A' from the left since that is the only 'A' whose neighbors are both 'A'.

Now it's Bob's turn.
Bob cannot make a move on his turn since there are no 'B's whose neighbors are both 'B'.
Thus, Alice wins, so return true.
```

Example 2:

```text
Input: colors = "AA"
Output: false
Explanation:
Alice has her turn first.
There are only two 'A's and both are on the edge of the line, so she cannot move on her turn.
Thus, Bob wins, so return false.
```

Example 3:

```text
Input: colors = "ABBBBBBBAAA"
Output: false
Explanation:
ABBBBBBBAAA -> ABBBBBBBAA
Alice moves first.
Her only option is to remove the second to last 'A' from the right.

ABBBBBBBAA -> ABBBBBBAA
Next is Bob's turn.
He has many options for which 'B' piece to remove. He can pick any.

On Alice's second turn, she has no more pieces that she can remove.
Thus, Bob wins, so return false.
```

__Constraints:__

- `1 <= colors.length <= 10 ^ 5`
- `colors` consists of only the letters `'A'` and `'B'`

__题目描述:__

总共有 `n` 个颜色片段排成一列，每个颜色片段要么是 `'A'` 要么是 `'B'` 。给你一个长度为 `n` 的字符串 `colors` ，其中 `colors[i]` 表示第 `i` 个颜色片段的颜色。

Alice 和 Bob 在玩一个游戏，他们 轮流 从这个字符串中删除颜色。Alice 先手 。

- 如果一个颜色片段为 `'A'` 且 __相邻两个颜色__ 都是颜色 `'A'` ，那么 Alice 可以删除该颜色片段。Alice __不可以__ 删除任何颜色 `'B'` 片段。
- 如果一个颜色片段为 `'B'` 且 __相邻两个颜色__ 都是颜色 `'B'` ，那么 Bob 可以删除该颜色片段。Bob __不可以__ 删除任何颜色 `'A'` 片段。
- Alice 和 Bob __不能__ 从字符串两端删除颜色片段。
- 如果其中一人无法继续操作，则该玩家 _输_ 掉游戏且另一玩家 __获胜__ 。

假设 Alice 和 Bob 都采用最优策略，如果 Alice 获胜，请返回 `true`，否则 Bob 获胜，返回 `false`。

__示例:__

示例 1：

```text
输入：colors = "AAABABB"
输出：true
解释：
AAABABB -> AABABB
Alice 先操作。
她删除从左数第二个 'A' ，这也是唯一一个相邻颜色片段都是 'A' 的 'A' 。

现在轮到 Bob 操作。
Bob 无法执行任何操作，因为没有相邻位置都是 'B' 的颜色片段 'B' 。
因此，Alice 获胜，返回 true 。
```

示例 2：

```text
输入：colors = "AA"
输出：false
解释：
Alice 先操作。
只有 2 个 'A' 且它们都在字符串的两端，所以她无法执行任何操作。
因此，Bob 获胜，返回 false 。
```

示例 3：

```text
输入：colors = "ABBBBBBBAAA"
输出：false
解释：
ABBBBBBBAAA -> ABBBBBBBAA
Alice 先操作。
她唯一的选择是删除从右数起第二个 'A' 。

ABBBBBBBAA -> ABBBBBBAA
接下来轮到 Bob 操作。
他有许多选择，他可以选择任何一个 'B' 删除。

然后轮到 Alice 操作，她无法删除任何片段。
所以 Bob 获胜，返回 false 。
```

__提示：__

- `1 <= colors.length <= 10 ^ 5`
- `colors` 只包含字母 `'A'` 和 `'B'`

__思路:__

```text
贪心
由于 Alice 和 Bob 互相不影响
只用计算连续 'AAA' 或者 'BBB' 的个数即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool winnerOfGame(string colors) 
    {
        int n = colors.size(), a = 0, b = 0;
        for (int i = 1; i < n - 1; i++) 
        {
            a += (colors[i] == 'A' and colors[i - 1] == 'A' and colors[i + 1] == 'A');
            b += (colors[i] == 'B' and colors[i - 1] == 'B' and colors[i + 1] == 'B');
        }
        return a > b;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean winnerOfGame(String colors) {
        int n = colors.length(), a = 0, b = 0;
        for (int i = 1; i < n - 1; i++) {
            if (colors.charAt(i) == 'A' && colors.charAt(i - 1) == 'A' && colors.charAt(i + 1) == 'A') ++a;
            if (colors.charAt(i) == 'B' && colors.charAt(i - 1) == 'B' && colors.charAt(i + 1) == 'B') ++b;
        }
        return a > b;
    }
}
```

__Python__:

```Python
class Solution:
    def winnerOfGame(self, colors: str) -> bool:
        return sum(colors[i - 1:i + 2] == 'AAA' for i in range(1, len(colors) - 1)) > sum(colors[i - 1:i + 2] == 'BBB' for i in range(1, len(colors) - 1))
```
