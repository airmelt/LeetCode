# 714 Best Time to Buy and Sell Stock with Transaction Fee 买卖股票的最佳时机含手续费

__Description__:
You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

__Note:__
You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

__Example:__

Example 1:

Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:

- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

Example 2:

Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6

__Constraints:__

1 <= prices.length <= 5 \* 10^4
1 <= prices[i] < 5 \* 10^4
0 <= fee < 5 \* 10^4

__题目描述__:
给定一个整数数组 prices，其中第 i 个元素代表了第 i 天的股票价格 ；非负整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

__注意:__
这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

__示例 :__

示例 1:

输入: prices = [1, 3, 2, 8, 4, 9], fee = 2
输出: 8
解释: 能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8.

__说明:__

0 < prices.length <= 50000.
0 < prices[i] < 50000.
0 <= fee < 50000.

__思路__:

动态规划
对于每一天只有两种状态, 要么手上持有股票, 要么当天卖出股票并支付手续费
设 dp[i][0] 表示当天不持有股票时的收益, dp[i][1] 表示当天持有股票时的收益
dp[i][0] = max(dp[i - 1][1] + prices[i] - fee, dp[i - 1][0]), 当天没有持有股票时, 收益为前一天没有持有股票和当天卖出股票并减去手续费的收益的较大值
dp[i][1] = max(dp[i - 1][0] - prices[i], dp[i - 1][1]), 当天持有股票, 收益为前一天持有股票和当天买入股票收益的较大值
最后返回 dp[n][0] 表示手上没有股票的最大收益
初始化 dp[0][0] = 0, dp[0][1] = -prices[0]
注意到只需要前一天的收益的记录, 可以将空间复杂度压缩到 O(1)
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxProfit(vector<int>& prices, int fee) 
    {
        int a = 0, b = -prices.front(), c = 0, d = 0, n = prices.size();
        for (int i = 1; i < n; i++) 
        {
            a = max(d - prices[i], b);
            c = max(b + prices[i] - fee, d);
            d = c;
            b = a;
        }
        return c;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int a = 0, b = -prices[0], c = 0, d = 0, n = prices.length;
        for (int i = 1; i < n; i++) {
            a = Math.max(d - prices[i], b);
            c = Math.max(b + prices[i] - fee, d);
            d = c;
            b = a;
        }
        return c;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        a, b, c, d, n = 0, -prices[0], 0, 0, len(prices)
        for i in range(1, n):
            a, b, c, d = max(d - prices[i], b), max(d - prices[i], b), max(b + prices[i] - fee, d), max(b + prices[i] - fee, d)
        return c
```
