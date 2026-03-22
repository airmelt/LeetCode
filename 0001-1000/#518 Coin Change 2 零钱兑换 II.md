# 518 Coin Change 2 零钱兑换 II

__Description__:
You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

__Example:__

Example 1:

Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

Example 2:

Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.

Example 3:

Input: amount = 10, coins = [10]
Output: 1

__Note:__

You can assume that

0 <= amount <= 5000
1 <= coin <= 5000
the number of coins is less than 500
the answer is guaranteed to fit into signed 32-bit integer

__题目描述__:
给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

__示例 :__

示例 1:

输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1

示例 2:

输入: amount = 3, coins = [2]
输出: 0
解释: 只用面额2的硬币不能凑成总金额3。

示例 3:

输入: amount = 10, coins = [10]
输出: 1

__注意:__

你可以假设：

0 <= amount (总金额) <= 5000
1 <= coin (硬币面额) <= 5000
硬币种类不超过 500 种
结果符合 32 位符号整数

__思路__:

动态规划
dp[i]表示 [0, i]可以用 dp[i]种表示方式, dp[0] = 1
dp[i] = dp[i] + dp[i - coin], if i >= coin
时间复杂度O(mn), 空间复杂度O(n), m表示 coins数组的长度, n表示 amount

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int change(int amount, vector<int>& coins) 
    {
        vector<int> dp(amount + 1);
        dp[0] = 1;
        for (const auto& coin : coins) for (int i = 1; i < amount + 1; i++) if (i >= coin) dp[i] += dp[i - coin];
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int change(int amount, int[] coins) {
        int dp[] = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) for (int i = 1; i < amount + 1; i++) if (i >= coin) dp[i] += dp[i - coin];
        return dp[amount];
    }
}
```

__Python__:

```Python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [1] + [0] * amount
        for coin in coins:
            for i in range(1, amount + 1):
                if i >= coin:
                    dp[i] += dp[i - coin]
        return dp[-1]
```
