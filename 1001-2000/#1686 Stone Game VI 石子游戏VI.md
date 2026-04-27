# 1686 Stone Game VI 石子游戏VI

__Description:__

Alice and Bob take turns playing a game, with Alice starting first.

There are `n` stones in a pile. On each player's turn, they can __remove__ a stone from the pile and receive points based on the stone's value. Alice and Bob may __value the stones differently__.

You are given two integer arrays of length `n`, `aliceValues` and `bobValues`. Each `aliceValues[i]` and `bobValues[i]` represents how Alice and Bob, respectively, value the `i ^ th` stone.

The winner is the person with the most points after all the stones are chosen. If both players have the same amount of points, the game results in a draw. Both players will play optimally. Both players know the other's values.

Determine the result of the game, and:

- If Alice wins, return `1`.
- If Bob wins, return `-1`.
- If the game results in a draw, return `0`.

__Example:__

Example 1:

```text
Input: aliceValues = [1,3], bobValues = [2,1]
Output: 1
Explanation:
If Alice takes stone 1 (0-indexed) first, Alice will receive 3 points.
Bob can only choose stone 0, and will only receive 2 points.
Alice wins.
```

Example 2:

```text
Input: aliceValues = [1,2], bobValues = [3,1]
Output: 0
Explanation:
If Alice takes stone 0, and Bob takes stone 1, they will both have 1 point.
Draw.
```

Example 3:

```text
Input: aliceValues = [2,4,3], bobValues = [1,6,7]
Output: -1
Explanation:
Regardless of how Alice plays, Bob will be able to have more points than Alice.
For example, if Alice takes stone 1, Bob can take stone 2, and Alice takes stone 0, Alice will have 6 points to Bob's 7.
Bob wins.
```

__Constraints:__

- `n == aliceValues.length == bobValues.length`
- `1 <= n <= 10 ^ 5`
- `1 <= aliceValues[i], bobValues[i] <= 100`

__题目描述:__

Alice 和 Bob 轮流玩一个游戏，Alice 先手。

一堆石子里总共有 `n` 个石子，轮到某个玩家时，他可以 __移出__ 一个石子并得到这个石子的价值。Alice 和 Bob 对石子价值有 __不一样的的评判标准__ 。双方都知道对方的评判标准。

给你两个长度为 `n` 的整数数组 `aliceValues` 和 `bobValues` 。 `aliceValues[i]` 和 `bobValues[i]` 分别表示 Alice 和 Bob 认为第 `i` 个石子的价值。

所有石子都被取完后，得分较高的人为胜者。如果两个玩家得分相同，那么为平局。两位玩家都会采用 最优策略 进行游戏。

请你推断游戏的结果，用如下的方式表示：

- 如果 Alice 赢，返回 `1` 。
- 如果 Bob 赢，返回 `-1` 。
- 如果游戏平局，返回 `0` 。

__示例:__

示例 1：

```text
输入：aliceValues = [1,3], bobValues = [2,1]
输出：1
解释：
如果 Alice 拿石子 1 （下标从 0开始），那么 Alice 可以得到 3 分。
Bob 只能选择石子 0 ，得到 2 分。
Alice 获胜。
```

示例 2：

```text
输入：aliceValues = [1,2], bobValues = [3,1]
输出：0
解释：
Alice 拿石子 0 ， Bob 拿石子 1 ，他们得分都为 1 分。
打平。
```

示例 3：

```text
输入：aliceValues = [2,4,3], bobValues = [1,6,7]
输出：-1
解释：
不管 Alice 怎么操作，Bob 都可以得到比 Alice 更高的得分。
比方说，Alice 拿石子 1 ，Bob 拿石子 2 ， Alice 拿石子 0 ，Alice 会得到 6 分而 Bob 得分为 7 分。
Bob 会获胜。
```

__提示：__

- `n == aliceValues.length == bobValues.length`
- `1 <= n <= 10 ^ 5`
- `1 <= aliceValues[i], bobValues[i] <= 100`

__思路:__

```text
贪心
对于两个石头, 价值分别为 a1, b1 和 a2, b2
拿第一个石头价值为 a1 - b2, 拿第二个石头价值为 a2 - b1
若 a1 - b2 > a2 - b1, 即 a1 + b1 > a2 + b2
所以记录 a[i] + b[i] 的总和, 从大到小排序, 每次取最大的
bob 拿的就是剩下的
所以减去 b[i] 的总和就是价值差
时间复杂度为 O(N), 空间复杂度为 O(1), 可以用 aliceValues 数组记录总和
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int stoneGameVI(vector<int>& aliceValues, vector<int>& bobValues) 
    {
        int n = aliceValues.size(), result = 0;
        for (int i = 0; i < n; i++) 
        {
            aliceValues[i] += bobValues[i];
            result -= bobValues[i];
        }
        sort(aliceValues.begin(), aliceValues.end());
        for (int i = n - 1; i > -1; i -= 2) result += aliceValues[i];
        return !result ? 0 : (result > 0 ? 1 : -1);
    }
};
```

__Java__:

```Java
class Solution {
    public int stoneGameVI(int[] aliceValues, int[] bobValues) {
        int n = aliceValues.length, result = 0;
        for (int i = 0; i < n; i++) {
            aliceValues[i] += bobValues[i];
            result -= bobValues[i];
        }
        Arrays.sort(aliceValues);
        for (int i = n - 1; i > -1; i -= 2) result += aliceValues[i];
        return result == 0 ? 0 : (result > 0 ? 1 : -1);
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameVI(self, aliceValues: List[int], bobValues: List[int]) -> int:
        return 0 if not (result := sum(sorted([(a + b) for a, b in zip(aliceValues, bobValues)], reverse=True)[::2]) - sum(bobValues)) else -1 if result < 0 else 1
```
