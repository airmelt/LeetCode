# 3175 Find The First Player to win K Games in a Row 找到连续赢 K 场比赛的第一位玩家

__Description:__

A competition consists of `n` players numbered from `0` to `n - 1`.

You are given an integer array `skills` of size `n` and a __positive__ integer `k`, where `skills[i]` is the skill level of player `i`. All integers in `skills` are __unique__.

All players are standing in a queue in order from player `0` to player `n - 1`.

The competition process is as follows:

- The first two players in the queue play a game, and the player with the __higher__ skill level wins.
- After the game, the winner stays at the beginning of the queue, and the loser goes to the end of it.

The winner of the competition is the __first__ player who wins `k` games __in a row__.

Return the initial index of the _winning_ player.

__Example:__

Example 1:

```text
Input: skills = [4,2,6,3,9], k = 2

Output: 2

Explanation:

Initially, the queue of players is [0,1,2,3,4]. The following process happens:

Players 0 and 1 play a game, since the skill of player 0 is higher than that of player 1, player 0 wins. The resulting queue is [0,2,3,4,1].
Players 0 and 2 play a game, since the skill of player 2 is higher than that of player 0, player 2 wins. The resulting queue is [2,3,4,1,0].
Players 2 and 3 play a game, since the skill of player 2 is higher than that of player 3, player 2 wins. The resulting queue is [2,4,1,0,3].
Player 2 won k = 2 games in a row, so the winner is player 2.
```

Example 2:

```text
Input: skills = [2,5,4], k = 3

Output: 1

Explanation:

Initially, the queue of players is [0,1,2]. The following process happens:

Players 0 and 1 play a game, since the skill of player 1 is higher than that of player 0, player 1 wins. The resulting queue is [1,2,0].
Players 1 and 2 play a game, since the skill of player 1 is higher than that of player 2, player 1 wins. The resulting queue is [1,0,2].
Players 1 and 0 play a game, since the skill of player 1 is higher than that of player 0, player 1 wins. The resulting queue is [1,2,0].
Player 1 won k = 3 games in a row, so the winner is player 1.
```

__Constraints:__

- `n == skills.length`
- `1 <= n <= 10 ^ 5`
- `1 <= k <= 10 ^ 9`
- `1 <= skills[i] <= 10 ^ 6`
- All integers in `skills` are unique.

__题目描述:__

有 `n` 位玩家在进行比赛，玩家编号依次为 `0` 到 `n - 1` 。

给你一个长度为 `n` 的整数数组 `skills` 和一个 __正__ 整数 `k` ，其中 `skills[i]` 是第 `i` 位玩家的技能等级。`skills` 中所有整数 __互不相同__ 。

所有玩家从编号 `0` 到 `n - 1` 排成一列。

比赛进行方式如下：

- 队列中最前面两名玩家进行一场比赛，技能等级 __更高__ 的玩家胜出。
- 比赛后，获胜者保持在队列的开头，而失败者排到队列的末尾。

这个比赛的赢家是 __第一位连续__ 赢下 `k` 场比赛的玩家。

请你返回这个比赛的赢家编号。

__示例:__

示例 1：

```text
输入：skills = [4,2,6,3,9], k = 2

输出：2

解释：

一开始，队列里的玩家为 [0,1,2,3,4] 。比赛过程如下：

玩家 0 和 1 进行一场比赛，玩家 0 的技能等级高于玩家 1 ，玩家 0 胜出，队列变为 [0,2,3,4,1] 。
玩家 0 和 2 进行一场比赛，玩家 2 的技能等级高于玩家 0 ，玩家 2 胜出，队列变为 [2,3,4,1,0] 。
玩家 2 和 3 进行一场比赛，玩家 2 的技能等级高于玩家 3 ，玩家 2 胜出，队列变为 [2,4,1,0,3] 。
玩家 2 连续赢了 k = 2 场比赛，所以赢家是玩家 2 。
```

示例 2：

```text
输入：skills = [2,5,4], k = 3

输出：1

解释：

一开始，队列里的玩家为 [0,1,2] 。比赛过程如下：

玩家 0 和 1 进行一场比赛，玩家 1 的技能等级高于玩家 0 ，玩家 1 胜出，队列变为 [1,2,0] 。
玩家 1 和 2 进行一场比赛，玩家 1 的技能等级高于玩家 2 ，玩家 1 胜出，队列变为 [1,0,2] 。
玩家 1 和 0 进行一场比赛，玩家 1 的技能等级高于玩家 0 ，玩家 1 胜出，队列变为 [1,2,0] 。
玩家 1 连续赢了 k = 3 场比赛，所以赢家是玩家 1 。
```

__提示：__

- `n == skills.length`
- `1 <= n <= 10 ^ 5`
- `1 <= k <= 10 ^ 9`
- `1 <= skills[i] <= 10 ^ 6`
- `skills` 中的整数互不相同。

__思路:__

```text
模拟
因为胜者可以一直比赛
所以如果 k 比 n 还大实际上就是返回最大 skill 的下标
比赛的时候从左往右遍历
如果当前的选手比之前的选手更厉害
记录更厉害的选手的下标并重置胜利次数
如果胜利次数已经为 k 提前结束遍历
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findWinningPlayer(vector<int>& skills, int k) 
    {
        int win = 0, mx = 0, n = skills.size();
        for (int i = 1; i < n and win != k; i++) 
        {
            if (skills[i] > skills[mx]) 
            {
                mx = i;
                win = 0;
            }
            ++win;
        }
        return mx;
    }
};
```

__Java__:

```Java
class Solution {
    public int findWinningPlayer(int[] skills, int k) {
        int win = 0, mx = 0, n = skills.length;
        for (int i = 1; i < n && win != k; i++) {
            if (skills[i] > skills[mx]) {
                mx = i;
                win = 0;
            }
            ++win;
        }
        return mx;
    }
}
```

__Python__:

```Python
class Solution:
    def findWinningPlayer(self, skills: List[int], k: int) -> int:
        max_i = win = 0
        for i, skill in enumerate(skills[1:], 1):
            if skill > skills[max_i]:
                max_i, win = i, 0
            win += 1
            if win == k:
                break
        return max_i
```
