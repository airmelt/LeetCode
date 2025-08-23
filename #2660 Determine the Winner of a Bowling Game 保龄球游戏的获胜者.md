# 2660 Determine the Winner of a Bowling Game 保龄球游戏的获胜者

__Description:__

You are given two __0-indexed__ integer arrays `player1` and `player2`, representing the number of pins that player 1 and player 2 hit in a bowling game, respectively.

The bowling game consists of `n` turns, and the number of pins in each turn is exactly 10.

Assume a player hits `xi` pins in the i ^ th turn. The value of the i ^ th turn for the player is:

- `2xi` if the player hits 10 pins _in either (i - 1) ^ th or (i - 2) ^ th turn_ .
- Otherwise, it is `xi`.

The __score__ of the player is the sum of the values of their `n` turns.

Return

- 1 if the score of player 1 is more than the score of player 2,
- 2 if the score of player 2 is more than the score of player 1, and
- 0 in case of a draw.

__Example:__

Example 1:

```text
Input: player1 = [5,10,3,2], player2 = [6,5,7,3]

Output: 1

Explanation:

The score of player 1 is 5 + 10 + 2*3 + 2*2 = 25.

The score of player 2 is 6 + 5 + 7 + 3 = 21.
```

Example 2:

```text
Input: player1 = [3,5,7,6], player2 = [8,10,10,2]

Output: 2

Explanation:

The score of player 1 is 3 + 5 + 7 + 6 = 21.

The score of player 2 is 8 + 10 + 2*10 + 2*2 = 42.
```

Example 3:

```text
Input: player1 = [2,3], player2 = [4,1]

Output: 0

Explanation:

The score of player1 is 2 + 3 = 5.

The score of player2 is 4 + 1 = 5.
```

Example 4:

```text
Input: player1 = [1,1,1,10,10,10,10], player2 = [10,10,10,10,1,1,1]

Output: 2

Explanation:

The score of player1 is 1 + 1 + 1 + 10 + 2*10 + 2*10 + 2*10 = 73.

The score of player2 is 10 + 2*10 + 2*10 + 2*10 + 2*1 + 2*1 + 1 = 75.
```

__Constraints:__

- `n == player1.length == player2.length`
- `1 <= n <= 1000`
- `0 <= player1[i], player2[i] <= 10`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `player1` 和 `player2` ，分别表示玩家 1 和玩家 2 击中的瓶数。

保龄球比赛由 `n` 轮组成，每轮的瓶数恰好为 `10` 。

假设玩家在第 `i` 轮中击中 `xi` 个瓶子。玩家第 `i` 轮的价值为:

- 如果玩家在该轮的前两轮的任何一轮中击中了 `10` 个瓶子，则为 `2xi` 。
- 否则，为 `xi` 。

玩家的得分是其 `n` 轮价值的总和。

返回

- 如果玩家 1 的得分高于玩家 2 的得分，则为 `1` ；
- 如果玩家 2 的得分高于玩家 1 的得分，则为 `2` ；
- 如果平局，则为 `0` 。

__示例:__

示例 1：

```text
输入：player1 = [5,10,3,2], player2 = [6,5,7,3]

输出：1

解释：

玩家 1 的分数为 5 + 10 + 2*3 + 2*2 = 25。

玩家 2 的分数为 6 + 5 + 7 + 3 = 21。
```

示例 2：

```text
输入：player1 = [3,5,7,6], player2 = [8,10,10,2]

输出：2

解释：

玩家 1 的分数为 3 + 5 + 7 + 6 = 21。

玩家 2 的分数为 8 + 10 + 2*10 + 2*2 = 42。
```

示例 3：

```text
输入：player1 = [2,3], player2 = [4,1]

输出：0

解释：

玩家 1 的分数为 2 + 3 = 5。

玩家 2 的分数为 4 + 1 = 5。
```

示例 4：

```text
输入：player1 = [1,1,1,10,10,10,10], player2 = [10,10,10,10,1,1,1]

输出：2

解释：

玩家 1 的分数为 1 + 1 + 1 + 10 + 2*10 + 2*10 + 2*10 = 73。

玩家 2 的分数为 is 10 + 2*10 + 2*10 + 2*10 + 2*1 + 2*1 + 1 = 75。
```

__提示：__

- `n == player1.length == player2.length`
- `1 <= n <= 1000`
- `0 <= player1[i], player2[i] <= 10`

__思路:__

```text
模拟
分别计算两个人的分数
如果有分数是 10, 则将前两次的分数再加一次
然后返回分数较高的玩家
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int isWinner(vector<int>& player1, vector<int>& player2) 
    {
        auto score = [](auto player) -> int
        {
            int result = accumulate(player.begin(), player.end(), 0), n = player.size();
            for (int i = 1; i < n; i++) if (i > 0 and player[i - 1] == 10 or (i > 1 and player[i - 2] == 10)) result += player[i];
            return result;
        };
        int s1 = score(player1), s2 = score(player2);
        return s1 == s2 ? 0 : s1 > s2 ? 1 : 2;
    }
};
```

__Java__:

```Java
class Solution {
    public int isWinner(int[] player1, int[] player2) {
        int s1 = score(player1), s2 = score(player2);
        return s1 == s2 ? 0 : s1 > s2 ? 1 : 2;
    }

    private int score(int[] player) {
        int result = Arrays.stream(player).sum(), n = player.length;
        for (int i = 1; i < n; i++) if (i > 0 && player[i - 1] == 10 || (i > 1 && player[i - 2] == 10)) result += player[i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def isWinner(self, player1: List[int], player2: List[int]) -> int:
        return 0 if (s1 := sum([(v << 1) if (i and player1[i - 1] == 10) or (i > 1 and (player1[i - 1] == 10 or player1[i - 2] == 10)) else v for i, v in enumerate(player1)])) == (s2 := sum([(v << 1) if (i and player2[i - 1] == 10) or (i > 1 and (player2[i - 1] == 10 or player2[i - 2] == 10)) else v for i, v in enumerate(player2)])) else 1 if s1 > s2 else 2
```
