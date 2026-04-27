# 2706 Buy Two Chocolates 购买两块巧克力

__Description:__

You are given an integer array `prices` representing the prices of various chocolates in a store. You are also given a single integer `money`, which represents your initial amount of money.

You must buy exactly two chocolates in such a way that you still have some non-negative leftover money. You would like to minimize the sum of the prices of the two chocolates you buy.

Return _the amount of money you will have leftover after buying the two chocolates_. If there is no way for you to buy two chocolates without ending up in debt, return `money`. Note that the leftover must be non-negative.

__Example:__

Example 1:

```text
Input: prices = [1,2,2], money = 3
Output: 0
Explanation: Purchase the chocolates priced at 1 and 2 units respectively. You will have 3 - 3 = 0 units of money afterwards. Thus, we return 0.
```

Example 2:

```text
Input: prices = [3,2,3], money = 3
Output: 3
Explanation: You cannot buy 2 chocolates without going in debt, so we return 3.
```

__Constraints:__

- `2 <= prices.length <= 50`
- `1 <= prices[i] <= 100`
- `1 <= money <= 100`

__题目描述:__

给你一个整数数组 `prices` ，它表示一个商店里若干巧克力的价格。同时给你一个整数 `money` ，表示你一开始拥有的钱数。

你必须购买 恰好 两块巧克力，而且剩余的钱数必须是 非负数 。同时你想最小化购买两块巧克力的总花费。

请你返回在购买两块巧克力后，最多能剩下多少钱。如果购买任意两块巧克力都超过了你拥有的钱，请你返回 `money` 。注意剩余钱数必须是非负数。

__示例:__

示例 1：

```text
输入：prices = [1,2,2], money = 3
输出：0
解释：分别购买价格为 1 和 2 的巧克力。你剩下 3 - 3 = 0 块钱。所以我们返回 0 。
```

示例 2：

```text
输入：prices = [3,2,3], money = 3
输出：3
解释：购买任意 2 块巧克力都会超过你拥有的钱数，所以我们返回 3 。
```

__提示：__

- `2 <= prices.length <= 50`
- `1 <= prices[i] <= 100`
- `1 <= money <= 100`

__思路:__

```text
只要购买最便宜的两块巧克力
1. 排序
排序后选择前两个元素
如果比 money 少可以购买
否则直接返回 money
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
2. 遍历
用两个变量分别记录最大值和次大值
一边遍历一边更新
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int buyChoco(vector<int>& prices, int money) 
    {
        int first = INT_MAX, second = INT_MAX;
        for (const auto& price : prices)
        {
            if (price < first)
            {
                second = first;
                first = price;
            }
            else if (price < second) second = price;
        }
        return money < first + second ? money : money - first - second;
    }
};
```

__Java__:

```Java
class Solution {
    public int buyChoco(int[] prices, int money) {
        Arrays.sort(prices);
        return money - prices[0] - prices[1] < 0 ? money : money - prices[0] - prices[1];
    }
}
```

__Python__:

```Python
class Solution:
    def buyChoco(self, prices: List[int], money: int) -> int:
        return money if (result := money - sum(sorted(prices)[:2])) < 0 else result
```
