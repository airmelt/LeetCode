# 1510 Stone Game IV IV石子游戏IV

__Description:__

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are `n` stones in a pile. On each player's turn, that player makes a _move_ consisting of removing __any__ non-zero __square number__ of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer `n`, return `true` if and only if Alice wins the game otherwise return `false`, assuming both players play optimally.

__Example:__

Example 1:

```text
Input: n = 1
Output: true
Explanation: Alice can remove 1 stone winning the game because Bob doesn't have any moves.
```

Example 2:

```text
Input: n = 2
Output: false
Explanation: Alice can only remove 1 stone, after that Bob removes the last one winning the game (2 -> 1 -> 0).
```

Example 3:

```text
Input: n = 4
Output: true
Explanation: n is already a perfect square, Alice can win with one move, removing 4 stones (4 -> 0).
```

__Constraints:__

- `1 <= n <= 10 ^ 5`

__题目描述:__

Alice 和 Bob 两个人轮流玩一个游戏，Alice 先手。

一开始，有 `n` 个石子堆在一起。每个人轮流操作，正在操作的玩家可以从石子堆里拿走 __任意__ 非零 __平方数__ 个石子。

如果石子堆里没有石子了，则无法操作的玩家输掉游戏。

给你正整数 `n` ，且已知两个人都采取最优策略。如果 Alice 会赢得比赛，那么返回 `True` ，否则返回 `False` 。

__示例:__

示例 1：

```text
输入：n = 1
输出：true
解释：Alice 拿走 1 个石子并赢得胜利，因为 Bob 无法进行任何操作。
```

示例 2：

```text
输入：n = 2
输出：false
解释：Alice 只能拿走 1 个石子，然后 Bob 拿走最后一个石子并赢得胜利（2 -> 1 -> 0）。
```

示例 3：

```text
输入：n = 4
输出：true
解释：n 已经是一个平方数，Alice 可以一次全拿掉 4 个石子并赢得胜利（4 -> 0）。
```

示例 4：

```text
输入：n = 7
输出：false
解释：当 Bob 采取最优策略时，Alice 无法赢得比赛。
如果 Alice 一开始拿走 4 个石子， Bob 会拿走 1 个石子，然后 Alice 只能拿走 1 个石子，Bob 拿走最后一个石子并赢得胜利（7 -> 3 -> 2 -> 1 -> 0）。
如果 Alice 一开始拿走 1 个石子， Bob 会拿走 4 个石子，然后 Alice 只能拿走 1 个石子，Bob 拿走最后一个石子并赢得胜利（7 -> 6 -> 2 -> 1 -> 0）。
```

示例 5：

```text
输入：n = 17
输出：false
解释：如果 Bob 采取最优策略，Alice 无法赢得胜利。
```

__提示：__

- `1 <= n <= 10 ^ 5`

__思路:__

```text
动态规划
假设 dp[i] 表示有 i 颗棋子, Alice 是否胜利
当 dp[i] = false 时, dp[i + j ^ 2] = true, 其中 0 < j <= n - i
其他情况都为 false
初始化 dp = [false] * (n + 1)
时间复杂度为 O(N ^ 3 / 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    bool dp[100005];
public:
    bool winnerSquareGame(int n) 
    {
        memset(dp, 0, 100005);
        for (int i = 0, j = 1; i <= n; i++, j = 1) if (!dp[i]) while (i + j * j <= n) dp[i + j * j++] = 1;
        return dp[n];
    }
};
```

__Java__:

```Java
class Solution {
    public boolean winnerSquareGame(int n) {
        boolean[] dp = new boolean[n + 1];
        for (int i = 0, j = 1; i <= n; i++, j = 1) if (!dp[i]) while (i + j * j <= n) dp[i + j * j++] = true;
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        dp = [False] * (n + 1)
        for i in range(n + 1):
            j = 1
            if not dp[i]:
                while i + j * j <= n:
                    dp[j * j + i] = True
                    j += 1
        return dp[-1]
```
