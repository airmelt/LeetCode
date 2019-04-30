__Description__:
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example:**
Example 1:
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.

Example 2:
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

__题目描述__:
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

**示例:**
示例 1:
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

示例 2:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

__思路__:
动态规划
前 i天的最大收益 = max[前 i天的价格 - min(前 i天价格)]
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        int purchase = (1 << 31) - 1;
        for (int i = 0; i < prices.size(); i++) {
            if (prices[i] < purchase) purchase = prices[i];
            if (prices[i] - purchase > result) result = prices[i] - purchase;
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int maxProfit(int[] prices) {
        int result = 0;
        int purchase = (1 << 31) - 1;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < purchase) purchase = prices[i];
            if (prices[i] - purchase > result) result = prices[i] - purchase;
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        purchase = (1 << 31) - 1
        for item in prices:
            purchase = min(item, purchase)
            result = max(item - purchase, result)
        return result
```
