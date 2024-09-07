# 2240 Number of Ways to Buy Pens and Pencils 买钢笔和铅笔的方案数

__Description:__

You are given an integer `total` indicating the amount of money you have. You are also given two integers `cost1` and `cost2` indicating the price of a pen and pencil respectively. You can spend __part or all__ of your money to buy multiple quantities (or none) of each kind of writing utensil.

Return the number of distinct ways you can buy some number of pens and pencils.

__Example:__

Example 1:

```text
Input: total = 20, cost1 = 10, cost2 = 5
Output: 9
Explanation: The price of a pen is 10 and the price of a pencil is 5.
```

- If you buy 0 pens, you can buy 0, 1, 2, 3, or 4 pencils.
- If you buy 1 pen, you can buy 0, 1, or 2 pencils.
- If you buy 2 pens, you cannot buy any pencils.

The total number of ways to buy pens and pencils is 5 + 3 + 1 = 9.

Example 2:

```text
Input: total = 5, cost1 = 10, cost2 = 10
Output: 1
Explanation: The price of both pens and pencils are 10, which cost more than total, so you cannot buy any writing utensils. Therefore, there is only 1 way: buy 0 pens and 0 pencils.
```

__Constraints:__

- `1 <= total, cost1, cost2 <= 10 ^ 6`

__题目描述:__

给你一个整数 `total` ，表示你拥有的总钱数。同时给你两个整数 `cost1` 和 `cost2` ，分别表示一支钢笔和一支铅笔的价格。你可以花费你部分或者全部的钱，去买任意数目的两种笔。

请你返回购买钢笔和铅笔的 不同方案数目 。

__示例:__

示例 1：

```text
输入：total = 20, cost1 = 10, cost2 = 5
输出：9
解释：一支钢笔的价格为 10 ，一支铅笔的价格为 5 。
```

- 如果你买 0 支钢笔，那么你可以买 0 ，1 ，2 ，3 或者 4 支铅笔。
- 如果你买 1 支钢笔，那么你可以买 0 ，1 或者 2 支铅笔。
- 如果你买 2 支钢笔，那么你没法买任何铅笔。

所以买钢笔和铅笔的总方案数为 5 + 3 + 1 = 9 种。

示例 2：

```text
输入：total = 5, cost1 = 10, cost2 = 10
输出：1
解释：钢笔和铅笔的价格都为 10 ，都比拥有的钱数多，所以你没法购买任何文具。所以只有 1 种方案：买 0 支钢笔和 0 支铅笔。
```

__提示：__

- `1 <= total, cost1, cost2 <= 10 ^ 6`

__思路:__

```text
数学
先计算全部买钢笔的方案数
然后逐渐减少买钢笔的数量, 计算剩余钱数能买铅笔的数量
时间复杂度为 O(N), 空间复杂度为 O(1), 其中 N = total / cost1
```

__代码:__

__C++__:

```C++
class Solution {
public:
    long long waysToBuyPensPencils(int total, int cost1, int cost2) {
        long long n = 1LL + total / cost1, result = n;
        for (long long i = 0LL; i < n; i++) result += (total - cost1 * i) / cost2;
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public long waysToBuyPensPencils(int total, int cost1, int cost2) {
        long n = 1L + total / cost1, result = n;
        for (long i = 0L; i < n; i++) result += (total - cost1 * i) / cost2;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def waysToBuyPensPencils(self, total: int, cost1: int, cost2: int) -> int:
        return sum((total - x * cost1) // cost2 + 1 for x in range(total // cost1 + 1))
```
