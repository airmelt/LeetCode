# 1475 Final Prices With a Special Discount in a Shop 商品折扣后的最终价格

__Description:__

You are given an integer array  `prices` where  `prices[i]` is the price of the  `i ^ th` item in a shop.

There is a special discount for items in the shop. If you buy the  `i ^ th` item, then you will receive a discount equivalent to  `prices[j]` where  `j` is the minimum index such that  `j > i` and  `prices[j] <= prices[i]`. Otherwise, you will not receive any discount at all.

Return an integer array  `answer` where  `answer[i]` is the final price you will pay for the  `i ^ th` item of the shop, considering the special discount.

__Example:__

Example 1:

```text
Input: prices = [8,4,6,2,3]
Output: [4,2,4,2,3]
Explanation: 
For item 0 with price[0]=8 you will receive a discount equivalent to prices[1]=4, therefore, the final price you will pay is 8 - 4 = 4.
For item 1 with price[1]=4 you will receive a discount equivalent to prices[3]=2, therefore, the final price you will pay is 4 - 2 = 2.
For item 2 with price[2]=6 you will receive a discount equivalent to prices[3]=2, therefore, the final price you will pay is 6 - 2 = 4.
For items 3 and 4 you will not receive any discount at all.
```

Example 2:

```text
Input: prices = [1,2,3,4,5]
Output: [1,2,3,4,5]
Explanation: In this case, for all items, you will not receive any discount at all.
```

Example 3:

```text
Input: prices = [10,1,1,6]
Output: [9,0,1,6]
```

__Constraints:__

- `1 <= prices.length <= 500`
- `1 <= prices[i] <= 1000`

__题目描述:__

给你一个数组  `prices` ，其中  `prices[i]` 是商店里第  `i` 件商品的价格。

商店里正在进行促销活动，如果你要买第  `i` 件商品，那么你可以得到与  `prices[j]` 相等的折扣，其中  `j` 是满足  `j > i` 且  `prices[j] <= prices[i]` 的 __最小下标__ ，如果没有满足条件的  `j` ，你将没有任何折扣。

请你返回一个数组，数组中第  `i` 个元素是折扣后你购买商品  `i` 最终需要支付的价格。

__示例:__

示例 1：

```text
输入：prices = [8,4,6,2,3]
输出：[4,2,4,2,3]
解释：
商品 0 的价格为 price[0]=8 ，你将得到 prices[1]=4 的折扣，所以最终价格为 8 - 4 = 4 。
商品 1 的价格为 price[1]=4 ，你将得到 prices[3]=2 的折扣，所以最终价格为 4 - 2 = 2 。
商品 2 的价格为 price[2]=6 ，你将得到 prices[3]=2 的折扣，所以最终价格为 6 - 2 = 4 。
商品 3 和 4 都没有折扣。
```

示例 2：

```text
输入：prices = [1,2,3,4,5]
输出：[1,2,3,4,5]
解释：在这个例子中，所有商品都没有折扣。
```

示例 3：

```text
输入：prices = [10,1,1,6]
输出：[9,0,1,6]
```

__提示：__

- `1 <= prices.length <= 500`
- `1 <= prices[i] <= 10 ^ 3`

__思路:__

```text
1. 暴力法
对每个元素求最近的小于它的值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 单调栈
从后往前遍历数组
如果当前元素比栈顶小, 弹出栈顶
如果栈不为空, 当前价格减去栈顶, 否则减 0
然后将当前价格插入栈
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> finalPrices(vector<int>& prices)
    {
        stack<int> s;
        int n = prices.size();
        vector<int> result(n);
        for (int i = n - 1; i > -1; i--) 
        {
            while (!s.empty() and s.top() > prices[i]) s.pop();
            result[i] = prices[i] - (s.empty() ? 0 : s.top());
            s.push(prices[i]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] finalPrices(int[] prices) {
        Stack<Integer> stack = new Stack<>();
        int n = prices.length, result[] = new int[n];
        for (int i = n - 1; i > -1; i--) {
            while (!stack.isEmpty() && stack.peek() > prices[i]) stack.pop();
            result[i] = prices[i] - (stack.isEmpty() ? 0 : stack.peek());
            stack.push(prices[i]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def finalPrices(self, prices: List[int]) -> List[int]:
        stack, result = [], [0] * (n := len(prices))
        for i in range(n - 1, -1, -1):
            while stack and stack[-1] > prices[i]:
                stack.pop()
            result[i] = prices[i] - (stack[-1] if stack else 0)
            stack.append(prices[i])
        return result
```
