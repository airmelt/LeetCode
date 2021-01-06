# 123 Best Time to Buy and Sell Stock III 买卖股票的最佳时机 III

__Description__:
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

__Note:__
You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

__Example:__

Example 1:

Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

Example 2:

Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.

Example 3:

Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

__题目描述__:
给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

__注意:__
你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

__示例 :__

示例 1:

输入: [3,3,5,0,0,3,1,4]
输出: 6
解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。

示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

示例 3:

输入: [7,6,4,3,1]
输出: 0
解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。

__思路__:

动态规划
dp[i][k][b]表示在第 i天还有 k次购买机会, b为 bool量表示是否持有股票
如 dp[2][3][1]表示第二天, 还有 3次购买机会, 手上持有股票; dp[1][1][0]表示第一天, 还有 1次操作机会, 手上没有股票
转移方程:
dp[i][k][0] = max(dp[i - 1][k][0], dp[i - 1][k][1] + price)
dp[i][k][1] = max(dp[i - 1][k][1], dp[i - 1][k - 1][0] - price)
卖出股票就加上今天的售价, 买入股票就减去今天的售价
初始条件(base):
dp[-1][k][0] = 0, 即还没开始买入股票之前收益是 0
dp[-1][k][1] = -inf, 即不可能在还没开始买入股票之前就持有股票
dp[i][0][0] = 0, 如果没有购买机会, 那收益就是 0
dp[i][0][0] = -inf, 如果没有购买机会, 不能持有股票
在这道题, k = 2, 可以穷举所有状态, 可以将空间压缩到 O(1)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxProfit(vector<int>& prices) 
    {
        int dp_i10 = 0, dp_i11 = INT_MIN, dp_i20 = 0, dp_i21 = INT_MIN;
        for (auto price : prices) 
        {
            dp_i20 = max(dp_i20, dp_i21 + price);
            dp_i21 = max(dp_i21, dp_i10 - price);
            dp_i10 = max(dp_i10, dp_i11 + price);
            dp_i11 = max(dp_i11, -price);
        }
        return dp_i20;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProfit(int[] prices) {
        int dp_i10 = 0, dp_i11 = Integer.MIN_VALUE, dp_i20 = 0, dp_i21 = Integer.MIN_VALUE;
        for (int price : prices) {
            dp_i20 = Math.max(dp_i20, dp_i21 + price);
            dp_i21 = Math.max(dp_i21, dp_i10 - price);
            dp_i10 = Math.max(dp_i10, dp_i11 + price);
            dp_i11 = Math.max(dp_i11, -price);
        }
        return dp_i20;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp_i10, dp_i11, dp_i20, dp_i21 = 0, -float('inf'), 0, -float('inf')
        for price in prices:
            dp_i20, dp_i21, dp_i10, dp_i11 =max(dp_i20, dp_i21 + price), max(dp_i21, dp_i10 - price), max(dp_i10, dp_i11 + price), max(dp_i11, -price)
        return dp_i20
```
