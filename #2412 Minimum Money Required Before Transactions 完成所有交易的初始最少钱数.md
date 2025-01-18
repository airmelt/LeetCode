# 2412 Minimum Money Required Before Transactions 完成所有交易的初始最少钱数

__Description:__

You are given a __0-indexed__ 2D integer array `transactions`, where `transactions[i] = [cost_i, cashback_i]`.

The array describes transactions, where each transaction must be completed exactly once in __some order__. At any given moment, you have a certain amount of `money`. In order to complete transaction `i`, `money >= cost_i` must hold true. After performing a transaction, `money` becomes `money - cost_i + cashback_i`.

Return _the minimum amount of_ `money` _required before any transaction so that all of the transactions can be completed __regardless of the order__ of the transactions._

__Example:__

Example 1:

```text
Input: transactions = [[2,1],[5,0],[4,2]]
Output: 10
Explanation:
Starting with money = 10, the transactions can be performed in any order.
It can be shown that starting with money < 10 will fail to complete all transactions in some order.
```

Example 2:

```text
Input: transactions = [[3,0],[0,3]]
Output: 3
Explanation:
```

- If transactions are in the order [[3,0],[0,3]], the minimum money required to complete the transactions is 3.
- If transactions are in the order [[0,3],[3,0]], the minimum money required to complete the transactions is 0.

Thus, starting with money = 3, the transactions can be performed in any order.

__Constraints:__

- `1 <= transactions.length <= 10 ^ 5`
- `transactions[i].length == 2`
- `0 <= cost_i, cashback_i <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `transactions`，其中 `transactions[i] = [cost_i, cashback_i]` 。

数组描述了若干笔交易。其中每笔交易必须以 __某种顺序__ 恰好完成一次。在任意一个时刻，你有一定数目的钱 `money` ，为了完成交易 `i` ， `money >= cost_i` 这个条件必须为真。执行交易后，你的钱数 `money` 变成 `money - cost_i + cashback_i` 。

请你返回 __任意一种__ 交易顺序下，你都能完成所有交易的最少钱数 `money` 是多少。

__示例:__

示例 1：

```text
输入：transactions = [[2,1],[5,0],[4,2]]
输出：10
解释：
刚开始 money = 10 ，交易可以以任意顺序进行。
可以证明如果 money < 10 ，那么某些交易无法进行。
```

示例 2：

```text
输入：transactions = [[3,0],[0,3]]
输出：3
解释：
```

- 如果交易执行的顺序是 [[3,0],[0,3]] ，完成所有交易需要的最少钱数是 3 。
- 如果交易执行的顺序是 [[0,3],[3,0]] ，完成所有交易需要的最少钱数
是 0 。

所以，刚开始钱数为 3 ，任意顺序下交易都可以全部完成。

__提示：__

- `1 <= transactions.length <= 10 ^ 5`
- `transactions[i].length == 2`
- `0 <= cost_i, cashback_i <= 10 ^ 9`

__思路:__

```text
贪心
将所有亏的钱数加起来, 即 total = sum(max(cost - cashback, 0))
然后再加上最大的亏的钱数即可, 即 mx = max(min(cost, cashback))
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minimumMoney(vector<vector<int>>& transactions) 
    {
        long long total = 0LL, mx = 0LL;
        for (const auto& t : transactions) 
        {
            total += max((long long)t.front() - t.back(), 0LL);
            mx = max(mx, (long long)min(t.front(), t.back()));
        }
        return total + mx;
    }
};
```

__Java__:

```Java
class Solution {
    public long minimumMoney(int[][] transactions) {
        long total = 0L, mx = 0L;
        for (int[] t : transactions) {
            total += Math.max(t[0] - t[1], 0L);
            mx = Math.max(mx, Math.min(t[0], t[1]));
        }
        return total + mx;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMoney(self, transactions: List[List[int]]) -> int:
        total = mx = 0
        for cost, cashback in transactions:
            total += max(cost - cashback, 0)
            mx = max(mx, min(cost, cashback))
        return total + mx
```
