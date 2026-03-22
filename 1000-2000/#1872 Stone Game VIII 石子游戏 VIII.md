# 1872 Stone Game VIII 石子游戏 VIII

__Description:__

Alice and Bob take turns playing a game, with Alice starting first.

There are `n` stones arranged in a row. On each player's turn, while the number of stones is __more than one__, they will do the following:

The game stops when only one stone is left in the row.

The __score difference__ between Alice and Bob is `(Alice's score - Bob's score)`. Alice's goal is to __maximize__ the score difference, and Bob's goal is the __minimize__ the score difference.

Given an integer array `stones` of length `n` where `stones[i]` represents the value of the `i ^ th` stone __from the left__, return _the __score difference__ between Alice and Bob if they both play __optimally__._

__Example:__

Example 1:

```text
Input: stones = [-1,2,-3,4,-5]
Output: 5
Explanation:
- Alice removes the first 4 stones, adds (-1) + 2 + (-3) + 4 = 2 to her score, and places a stone of
  value 2 on the left. stones = [2,-5].
- Bob removes the first 2 stones, adds 2 + (-5) = -3 to his score, and places a stone of value -3 on
  the left. stones = [-3].
The difference between their scores is 2 - (-3) = 5.
```

Example 2:

```text
Input: stones = [7,-6,5,10,5,-2,-6]
Output: 13
Explanation:
- Alice removes all stones, adds 7 + (-6) + 5 + 10 + 5 + (-2) + (-6) = 13 to her score, and places a
  stone of value 13 on the left. stones = [13].
The difference between their scores is 13 - 0 = 13.
```

Example 3:

```text
Input: stones = [-10,-12]
Output: -22
Explanation:
- Alice can only make one move, which is to remove both stones. She adds (-10) + (-12) = -22 to her
  score and places a stone of value -22 on the left. stones = [-22].
The difference between their scores is (-22) - 0 = -22.
```

__Constraints:__

- `n == stones.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 4 <= stones[i] <= 10 ^ 4`

__题目描述:__

Alice 和 Bob 玩一个游戏，两人轮流操作， Alice 先手 。

总共有 `n` 个石子排成一行。轮到某个玩家的回合时，如果石子的数目 __大于 1__ ，他将执行以下操作:

当只剩下 一个 石子时，游戏结束。

Alice 和 Bob 的 __分数之差__ 为 `(Alice 的分数 - Bob 的分数)` 。 Alice 的目标是 __最大化__ 分数差，Bob 的目标是 __最小化__ 分数差。

给你一个长度为 `n` 的整数数组 `stones` ，其中 `stones[i]` 是 __从左边起__ 第 `i` 个石子的价值。请你返回在双方都采用 __最优__ 策略的情况下，Alice 和 Bob 的 __分数之差__ 。

__示例:__

示例 1：

```text
输入：stones = [-1,2,-3,4,-5]
输出：5
解释：
- Alice 移除最左边的 4 个石子，得分增加 (-1) + 2 + (-3) + 4 = 2 ，并且将一个价值为 2 的石子放在最左边。stones = [2,-5] 。
- Bob 移除最左边的 2 个石子，得分增加 2 + (-5) = -3 ，并且将一个价值为 -3 的石子放在最左边。stones = [-3] 。
两者分数之差为 2 - (-3) = 5 。
```

示例 2：

```text
输入：stones = [7,-6,5,10,5,-2,-6]
输出：13
解释：
- Alice 移除所有石子，得分增加 7 + (-6) + 5 + 10 + 5 + (-2) + (-6) = 13 ，并且将一个价值为 13 的石子放在最左边。stones = [13] 。
两者分数之差为 13 - 0 = 13 。
```

示例 3：

```text
输入：stones = [-10,-12]
输出：-22
解释：
- Alice 只有一种操作，就是移除所有石子。得分增加 (-10) + (-12) = -22 ，并且将一个价值为 -22 的石子放在最左边。stones = [-22] 。
两者分数之差为 (-22) - 0 = -22 。
```

__提示：__

- `n == stones.length`
- `2 <= n <= 10 ^ 5`
- `-10 ^ 4 <= stones[i] <= 10 ^ 4`

__思路:__

```text
动态规划 ➕ 前缀和
两个人每次选择的分数都是前缀和
先计算出前缀和 pre
设 dp[i] 表示 Alice 选择 [i, n - 1] 区间时, 两者的最大差值
如果 Alice 不选 i 那么 dp[i] = dp[i + 1]
如果 Alice 选择了 i, 那么 Alice 的得分为 pre[i](如果 pre 从 0 开始 则为 pre[i + 1]), 这时 Bob 的选择区间为 [i + 1, n - 1] 实际上就是 dp[i + 1], 所以 dp[i] = pre[i] - dp[i + 1]
综上所述, dp[i] = max(dp[i + 1], pre[i] - dp[i + 1])
初始化 dp[n - 1] = pre[n - 1](如果 pre 从 0 开始则为 pre[n])
最后返回 dp[1], Alice 每次选择的 x > 1, 所以不能选择 dp[0]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int stoneGameVIII(vector<int>& stones) 
    {
        int n = stones.size();
        vector<int> pre, dp(n);
        partial_sum(stones.begin(), stones.end(), back_inserter(pre));
        dp.back() = pre.back();
        for (int i = n - 2; i > 0; i--) dp[i] = max(dp[i + 1], pre[i] - dp[i + 1]);
        return dp[1];
    }
};
```

__Java__:

```Java
class Solution {
    public int stoneGameVIII(int[] stones) {
        int n = stones.length, pre[] = new int[n + 1], dp[] = new int[n];
        for (int i = 0; i < n; i++) pre[i + 1] += pre[i] + stones[i];
        dp[n - 1] = pre[n];
        for (int i = n - 2; i > 0; i--) dp[i] = Math.max(dp[i + 1], pre[i + 1] - dp[i + 1]);
        return dp[1];
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameVIII(self, stones: List[int]) -> int:
        return reduce(lambda x, y: max(x, y - x), list(accumulate(stones))[:0:-1])
```
