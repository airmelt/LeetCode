# 1025 Divisor Game 除数博弈

__Description__:
Alice and Bob take turns playing a game, with Alice starting first.

Initially, there is a number N on the chalkboard.  On each player's turn, that player makes a move consisting of:

Choosing any x with 0 < x < N and N % x == 0.
Replacing the number N on the chalkboard with N - x.
Also, if a player cannot make a move, they lose the game.

Return True if and only if Alice wins the game, assuming both players play optimally.

__Example:__

Example 1:

Input: 2
Output: true
Explanation: Alice chooses 1, and Bob has no more moves.

Example 2:

Input: 3
Output: false
Explanation: Alice chooses 1, Bob chooses 1, and Alice has no more moves.

__Note:__

1 <= N <= 1000

__题目描述__:
爱丽丝和鲍勃一起玩游戏，他们轮流行动。爱丽丝先手开局。

最初，黑板上有一个数字 N 。在每个玩家的回合，玩家需要执行以下操作：

选出任一 x，满足 0 < x < N 且 N % x == 0 。
用 N - x 替换黑板上的数字 N 。
如果玩家无法执行这些操作，就会输掉游戏。

只有在爱丽丝在游戏中取得胜利时才返回 True，否则返回 false。假设两个玩家都以最佳状态参与游戏。

__示例 :__

示例 1：

输入：2
输出：true
解释：爱丽丝选择 1，鲍勃无法进行操作。

示例 2：

输入：3
输出：false
解释：爱丽丝选择 1，鲍勃也选择 1，然后爱丽丝无法进行操作。

__提示：__

1 <= N <= 1000

__思路__:
只要拿到偶数必胜, 因为当前的数如果是奇数, 奇数的因子中只有奇数, 必然在进行操作之后会剩下偶数. 当拿到偶数的时候, 每次都取 1, 这样对手永远只能拿奇数, 最后偶数会剩下 2, 必胜
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool divisorGame(int N) 
    {
        return !(N & 1);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean divisorGame(int N) {
        return (N & 1) == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def divisorGame(self, N: int) -> bool:
        return (N & 1) == 0
```
