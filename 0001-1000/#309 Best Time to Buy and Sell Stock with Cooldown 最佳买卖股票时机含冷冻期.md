# 309 Best Time to Buy and Sell Stock with Cooldown 最佳买卖股票时机含冷冻期

__Description__:
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

__Example:__

Input: [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]

__题目描述__:
给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

__示例 :__

输入: [1,2,3,0,2]
输出: 3
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]

__思路__:

动态规划
dp[i][0]表示第 i天没有股票的收益, dp[i][1]表示第 i天持有股票的收益
这里因为有冷却期, 所以购买的时候要从前 2天开始
转移方程:
dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
dp[i][1] = max(dp[i - 1][1], dp[i - 2][0] - prices[i])
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxProfit(vector<int>& prices) 
    {
        int dp_i_0 = 0, dp_i_1 = INT_MIN, dp_pre_0 = 0;
        for (int i = 0; i < prices.size(); i++) 
        {
            int temp = dp_i_0;
            dp_i_0 = max(dp_i_0, dp_i_1 + prices[i]);
            dp_i_1 = max(dp_i_1, dp_pre_0 - prices[i]);
            dp_pre_0 = temp;
        }
        return dp_i_0;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProfit(int[] prices) {
        int a = 0, b = Integer.MIN_VALUE, c = 0;
        for (int i = 0; i < prices.length; i++) {
            int t = a;
            a = Math.max(a, b + prices[i]);
            b = Math.max(b, c - prices[i]);
            c = t;
        }
        return a;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        a, b, c = 0, -float('inf'), 0
        for i in range(len(prices)):
            a, b, c = max(a, b + prices[i]), max(b, c - prices[i]), a
        return a
```
