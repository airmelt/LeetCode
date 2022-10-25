# 1359 Count All Valid Pickup and Delivery Options 有效的快递序列数目

__Description:__

Given n orders, each order consist in pickup and delivery services.

Count all valid pickup/delivery possible sequences such that delivery(i) is always after of pickup(i).

Since the answer may be too large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 1
Output: 1
Explanation: Unique order (P1, D1), Delivery 1 always is after of Pickup 1.

Example 2:

Input: n = 2
Output: 6
Explanation: All possible orders:
(P1,P2,D1,D2), (P1,P2,D2,D1), (P1,D1,P2,D2), (P2,P1,D1,D2), (P2,P1,D2,D1) and (P2,D2,P1,D1).
This is an invalid order (P1,D2,P2,D1) because Pickup 2 is after of Delivery 2.

Example 3:

Input: n = 3
Output: 90

__Constraints:__

1 <= n <= 500

__题目描述：__

给你 n 笔订单，每笔订单都需要快递服务。

请你统计所有有效的 收件/配送 序列的数目，确保第 i 个物品的配送服务 delivery(i) 总是在其收件服务 pickup(i) 之后。

由于答案可能很大，请返回答案对 10^9 + 7 取余的结果。

__示例：__

示例 1：

输入：n = 1
输出：1
解释：只有一种序列 (P1, D1)，物品 1 的配送服务（D1）在物品 1 的收件服务（P1）后。

示例 2：

输入：n = 2
输出：6
解释：所有可能的序列包括：
(P1,P2,D1,D2)，(P1,P2,D2,D1)，(P1,D1,P2,D2)，(P2,P1,D1,D2)，(P2,P1,D2,D1) 和 (P2,D2,P1,D1)。
(P1,D2,P2,D1) 是一个无效的序列，因为物品 2 的收件服务（P2）不应在物品 2 的配送服务（D2）之后。

示例 3：

输入：n = 3
输出：90

__提示：__

1 <= n <= 500

__思路：__

数学
设 f(n) 表示 n 件快递的排列数
f(n) 可由 f(n - 1) 加上第 n 件快递得到
前 n - 1 件快递一共有 2(n - 1) 个序列
由于发件一定要在收件之后
要么两个次序在一起, 要么被 2(n - 1) 个序列分开
前者 2(n - 1) 加上一头一尾两个位置, 一共有 2n - 1 个位置能放
后者相当于在上面的 2n - 1 个位置中选出 2 个不相邻的位置, 共有 (2n - 1)(2n - 2) / 2 种方式
综上所述, f(n) = n(2n - 1)f(n - 1)
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int countOrders(int n) 
    {
        return n == 1 ? 1 : (int64_t)countOrders(n - 1) * ((n << 1) - 1) * n % (int)(1e9 + 7);
    }
};
```

__Java__:

```Java
class Solution {
    public int countOrders(int n) {
        long result = 1, mod = 1_000_000_007;
        for (int i = 2; i <= n; i++) result = result * ((i << 1) - 1) % mod * i % mod;
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def countOrders(self, n: int) -> int:
        return 1 if n == 1 else self.countOrders(n - 1) * ((n << 1) - 1) * n % (10 ** 9 + 7)
```
