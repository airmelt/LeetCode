__Description__:
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

__Example:__
Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

Example 2:

Input: coins = [2], amount = 3
Output: -1

Example 3:

Input: coins = [1], amount = 0
Output: 0

Example 4:

Input: coins = [1], amount = 1
Output: 1

Example 5:

Input: coins = [1], amount = 2
Output: 2
 
__Constraints:__

1 <= coins.length <= 12
1 <= coins[i] <= 2^31 - 1
0 <= amount <= 10^4

__题目描述__:
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。

__示例 :__
示例 1：

输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1

示例 2：

输入：coins = [2], amount = 3
输出：-1

示例 3：

输入：coins = [1], amount = 0
输出：0

示例 4：

输入：coins = [1], amount = 1
输出：1

示例 5：

输入：coins = [1], amount = 2
输出：2
 
__提示：__

1 <= coins.length <= 12
1 <= coins[i] <= 2^31 - 1
0 <= amount <= 10^4

__思路__:
动态规划
设 dp[i]表示可以最少用 dp[i]个硬币组成 i块钱
初始条件 dp[0] = 0, dp[i] = +infinity
动态转移方程 dp[i] = min(dp[i], dp[i - coin] + 1), dp[i]可以由前面的值加上一个硬币得到
时间复杂度O(mn), 空间复杂度O(m), m为amount, n为数组 coins的长度

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int coinChange(vector<int>& coins, int amount) 
    {
        vector<int> dp(amount + 1);
        for (int i = 1; i < amount + 1; i++)
        {
            dp[i] = 0x3f3f3f3f;
            for (auto &coin : coins) if (i >= coin) dp[i] = min(dp[i], dp[i - coin] + 1);
        }
        return dp.back() == 0x3f3f3f3f ? -1 : dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int dp[] = new int[amount + 1];
        for (int i = 1; i < amount + 1; i++) {
            dp[i] = 0x3f3f3f3f;
            for (int coin : coins) if (i >= coin) dp[i] = Math.min(dp[i], dp[i - coin] + 1);
        } 
        return dp[amount] == 0x3f3f3f3f ? -1 : dp[amount];
    }
}
```

__Python__:
```Python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [0] + [float('inf')] * amount
        for i in range(1, amount + 1):
            dp[i] = min(dp[i - coin] if i - coin >= 0 else float('inf') for coin in coins) + 1
        return dp[-1] if dp[-1] != float('inf') else -1
```