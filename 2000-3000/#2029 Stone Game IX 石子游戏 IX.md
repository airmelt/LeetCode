# 2029 Stone Game IX 石子游戏 IX

__Description:__

Alice and Bob continue their games with stones. There is a row of n stones, and each stone has an associated value. You are given an integer array `stones`, where `stones[i]` is the __value__ of the `i ^ th` stone.

Alice and Bob take turns, with __Alice__ starting first. On each turn, the player may remove any stone from `stones`. The player who removes a stone __loses__ if the __sum__ of the values of __all removed stones__ is divisible by `3`. Bob will win automatically if there are no remaining stones (even if it is Alice's turn).

Assuming both players play __optimally__, return `true` _if Alice wins and_ `false` _if Bob wins_.

__Example:__

Example 1:

```text
Input: stones = [2,1]
Output: true
Explanation: The game will be played as follows:
```

- Turn 1: Alice can remove either stone.
- Turn 2: Bob removes the remaining stone.

The sum of the removed stones is 1 + 2 = 3 and is divisible by 3. Therefore, Bob loses and Alice wins the game.

Example 2:

```text
Input: stones = [2]
Output: false
Explanation: Alice will remove the only stone, and the sum of the values on the removed stones is 2. 
Since all the stones are removed and the sum of values is not divisible by 3, Bob wins the game.
```

Example 3:

```text
Input: stones = [5,1,2,4,3]
Output: false
Explanation: Bob will always win. One possible way for Bob to win is shown below:
```

- Turn 1: Alice can remove the second stone with value 1. Sum of removed stones = 1.
- Turn 2: Bob removes the fifth stone with value 3. Sum of removed stones = 1 + 3 = 4.
- Turn 3: Alices removes the fourth stone with value 4. Sum of removed stones = 1 + 3 + 4 = 8.
- Turn 4: Bob removes the third stone with value 2. Sum of removed stones = 1 + 3 + 4 + 2 = 10.
- Turn 5: Alice removes the first stone with value 5. Sum of removed stones = 1 + 3 + 4 + 2 + 5 = 15.

Alice loses the game because the sum of the removed stones (15) is divisible by 3. Bob wins the game.

__Constraints:__

- `1 <= stones.length <= 10 ^ 5`
- `1 <= stones[i] <= 10 ^ 4`

__题目描述:__

Alice 和 Bob 再次设计了一款新的石子游戏。现有一行 n 个石子，每个石子都有一个关联的数字表示它的价值。给你一个整数数组 `stones` ，其中 `stones[i]` 是第 `i` 个石子的价值。

Alice 和 Bob 轮流进行自己的回合，__Alice__ 先手。每一回合，玩家需要从 `stones` 中移除任一石子。

- 如果玩家移除石子后，导致 __所有已移除石子__ 的价值 __总和__ 可以被 3 整除，那么该玩家就 __输掉游戏__ 。
- 如果不满足上一条，且移除后没有任何剩余的石子，那么 Bob 将会直接获胜（即便是在 Alice 的回合）。

假设两位玩家均采用 __最佳__ 决策。如果 Alice 获胜，返回 `true` ；如果 Bob 获胜，返回 `false` 。

__示例:__

示例 1：

```text
输入：stones = [2,1]
输出：true
解释：游戏进行如下：
```

- 回合 1：Alice 可以移除任意一个石子。
- 回合 2：Bob 移除剩下的石子。
已移除的石子的值总和为 1 + 2 = 3 且可以被 3 整除。因此，Bob 输，Alice 获胜。

示例 2：

```text
输入：stones = [2]
输出：false
解释：Alice 会移除唯一一个石子，已移除石子的值总和为 2 。 
由于所有石子都已移除，且值总和无法被 3 整除，Bob 获胜。
```

示例 3：

```text
输入：stones = [5,1,2,4,3]
输出：false
解释：Bob 总会获胜。其中一种可能的游戏进行方式如下：
```

- 回合 1：Alice 可以移除值为 1 的第 2 个石子。已移除石子值总和为 1 。
- 回合 2：Bob 可以移除值为 3 的第 5 个石子。已移除石子值总和为 = 1 + 3 = 4 。
- 回合 3：Alices 可以移除值为 4 的第 4 个石子。已移除石子值总和为 = 1 + 3 + 4 = 8 。
- 回合 4：Bob 可以移除值为 2 的第 3 个石子。已移除石子值总和为 = 1 + 3 + 4 + 2 = 10.
- 回合 5：Alice 可以移除值为 5 的第 1 个石子。已移除石子值总和为 = 1 + 3 + 4 + 2 + 5 = 15.
Alice 输掉游戏，因为已移除石子值总和（15）可以被 3 整除，Bob 获胜。

__提示：__

- `1 <= stones.length <= 10 ^ 5`
- `1 <= stones[i] <= 10 ^ 4`

__思路:__

```text
贪心
由于只关心石头模 3 的结果
按照模 3 统计石头的数量
如果 Alice 要赢, 第一步一定不能选 0
暂时不考虑 0
0 可以在除了第一次的任意时刻选择
1 和 2 是对称的
如果选择 1, Bob 为了不输游戏也必须选 1
那么就会构成一个 11212121212 的序列
如果选 2 就会构成一个 22121212121 的序列
当 count[0] 为偶数时, Alice 和 Bob 可以均匀分配 count[0], 此时如果需要 Alice 获胜, count[1] 和 count[2] 必须都大于 0, 选择较小的作为开头即可
当 count[0] 为奇数时, 需要保证开头的两个相同的选择完即 count[1] - count[2] > 2 或者 count[2] - count[1] > 2, 这时选择较多的开始即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool stoneGameIX(vector<int>& stones) 
    {
        vector<int> count(3);
        for (const auto& stone : stones) ++count[stone % 3];
        return !(count[0] & 1) ? (count[1] and count[2]) : count[2] > count[1] + 2 or count[1] > count[2] + 2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean stoneGameIX(int[] stones) {
        int[] count = new int[3];
        for (int stone : stones) ++count[stone % 3];
        return count[0] % 2 == 0 ? !(count[1] == 0 || count[2] == 0) : !(Math.abs(count[1] - count[2]) <= 2);
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameIX(self, stones: List[int]) -> bool:
        return bool(s[1] and s[2]) if not (s := Counter(i % 3 for i in stones))[0] & 1 else (s[2] > s[1] + 2 or s[1] > s[2] + 2)
```
