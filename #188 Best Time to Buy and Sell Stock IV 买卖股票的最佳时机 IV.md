__Description__:
Say you have an array for which the i-th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most k transactions.

__Note:__
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

__Example:__
Example 1:

Input: [2,4,1], k = 2
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.

Example 2:

Input: [3,2,6,5,0,3], k = 2
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.
             Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.

__题目描述__:
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

__注意: __
你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

__示例 :__
示例 1:

输入: [2,4,1], k = 2
输出: 2
解释: 在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。

示例 2:

输入: [3,2,6,5,0,3], k = 2
输出: 7
解释: 在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。

__思路__:
参考[LeetCode #123 Best Time to Buy and Sell Stock III 买卖股票的最佳时机 III](https://www.jianshu.com/p/afd580831d79)
注意在 k > n / 2时, 由于买入卖出需要 2天时间, 这时可以简化为无限次购买, 可用贪心求解
时间复杂度O(nk), 空间复杂度O(k)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxProfit(int k, vector<int>& prices) 
    {
        int n = prices.size(), dp = 0;
        if (k > n / 2) 
        {
            for (int i = 0; i < n - 1; i++) if (prices[i + 1] > prices[i]) dp += prices[i + 1] - prices[i];
            return dp;
        }
        vector<int> buy(k + 1, 0);
        vector<int> sell(k + 1, INT_MIN);
        for (int i = 0; i < n; i++) 
        {
            for (int j = 1; j < k + 1; j++) 
            {
                buy[j] = max(buy[j], sell[j] + prices[i]);
                sell[j] = max(sell[j], buy[j - 1] - prices[i]);
            }
        }
        return buy[k];
    }
};
```

__Java__:
```Java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if (k > n / 2) {
            int dp = 0, pre = Integer.MIN_VALUE;
            for (int price : prices) {
                int temp = dp;
                dp = Math.max(dp, pre + price);
                pre = Math.max(pre, temp - price);
            }
            return dp;
        }
        int dp[][] = new int[k + 1][2];
        for (int i = 0; i < k + 1; i++) dp[i][1] = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < k + 1; j++) {
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
            }
        }
        return dp[k][0];
    }
}
```

__Python__:
```Python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        def maxProfit_k_inf(prices: List[int]) -> int:
            dp_i_0, dp_i_1 = 0, -float('inf')
            for price in prices:
                dp_i_0, dp_i_1 = max(dp_i_0, dp_i_1 + price), max(dp_i_1, dp_i_0 - price)
            return dp_i_0
        if k > n // 2:
            return maxProfit_k_inf(prices)
        
        dp = [[0] * (k + 1), [-float('inf')] * (k + 1)]
        for i in range(n):
            for j in range(1, k + 1):
                dp[0][j], dp[1][j] = max(dp[0][j], dp[1][j] + prices[i]), max(dp[1][j], dp[0][j - 1] - prices[i])
        return dp[0][k]
```