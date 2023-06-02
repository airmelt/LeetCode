# 1648 Sell Diminishing-Valued Colored Balls 销售价值减少的颜色球

__Description:__

You have an `inventory` of different colored balls, and there is a customer that wants `orders` balls of __any__ color.

The customer weirdly values the colored balls. Each colored ball's value is the number of balls __of that color__ you currently have in your `inventory`. For example, if you own `6` yellow balls, the customer would pay `6` for the first yellow ball. After the transaction, there are only `5` yellow balls left, so the next yellow ball is then valued at `5` (i.e., the value of the balls decreases as you sell more to the customer).

You are given an integer array, `inventory`, where `inventory[i]` represents the number of balls of the `i ^ th` color that you initially own. You are also given an integer `orders`, which represents the total number of balls that the customer wants. You can sell the balls __in any order__.

Return _the __maximum__ total value that you can attain after selling_ `orders` _colored balls_. As the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1648-1](https://assets.leetcode.com/uploads/2020/11/05/jj.gif)

```text
Input: inventory = [2,5], orders = 4
Output: 14
Explanation: Sell the 1st color 1 time (2) and the 2nd color 3 times (5 + 4 + 3).
The maximum total value is 2 + 5 + 4 + 3 = 14.
```

Example 2:

```text
Input: inventory = [3,5], orders = 6
Output: 19
Explanation: Sell the 1st color 2 times (3 + 2) and the 2nd color 4 times (5 + 4 + 3 + 2).
The maximum total value is 3 + 2 + 5 + 4 + 3 + 2 = 19.
```

__Constraints:__

- `1 <= inventory.length <= 10 ^ 5`
- `1 <= inventory[i] <= 10 ^ 9`
- `1 <= orders <= min(sum(inventory[i]), 10 ^ 9)`

__题目描述:__

You have an `inventory` of different colored balls, and there is a customer that wants `orders` balls of __any__ color.

The customer weirdly values the colored balls. Each colored ball's value is the number of balls __of that color__ you currently have in your `inventory`. For example, if you own `6` yellow balls, the customer would pay `6` for the first yellow ball. After the transaction, there are only `5` yellow balls left, so the next yellow ball is then valued at `5` (i.e., the value of the balls decreases as you sell more to the customer).

You are given an integer array, `inventory`, where `inventory[i]` represents the number of balls of the `i ^ th` color that you initially own. You are also given an integer `orders`, which represents the total number of balls that the customer wants. You can sell the balls __in any order__.

Return _the __maximum__ total value that you can attain after selling_ `orders` _colored balls_. As the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1648-2](https://assets.leetcode.com/uploads/2020/11/05/jj.gif)

```text
Input: inventory = [2,5], orders = 4
Output: 14
Explanation: Sell the 1st color 1 time (2) and the 2nd color 3 times (5 + 4 + 3).
The maximum total value is 2 + 5 + 4 + 3 = 14.
```

Example 2:

```text
Input: inventory = [3,5], orders = 6
Output: 19
Explanation: Sell the 1st color 2 times (3 + 2) and the 2nd color 4 times (5 + 4 + 3 + 2).
The maximum total value is 3 + 2 + 5 + 4 + 3 + 2 = 19.
```

__Constraints:__

- `1 <= inventory.length <= 10 ^ 5`
- `1 <= inventory[i] <= 10 ^ 9`
- `1 <= orders <= min(sum(inventory[i]), 10 ^ 9)`

__思路:__

```text
二分
通过在区间 [0, max(inventory)] 二分查找找到一个值
使得 inventory 所有大于这个值的总和恰好小于 orders
剩下的部分可以由最后一个值和 orders 差值补齐
比如 inventory = [2, 5], orders = 4
那么我们可以找到一个值 2, 使得 inventory 所有大于 2 的总和恰好小于 4
那么剩下的部分就是 [2, 2] 还需要 1 个球
总得分再加上 1 个 2 即可
求和需要用等差数列求和公式快速计算
时间复杂度为 O(NlogM), 空间复杂度为 O(1), M 为 max(inventory)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxProfit(vector<int>& inventory, int orders) 
    {
        int MOD = 1e9 + 7, right = *max_element(inventory.begin(), inventory.end()), left = 0;
        while (left <= right) 
        {
            int mid = left + ((right - left) >> 1);
            long total = accumulate(inventory.begin(), inventory.end(), 0L, [&](long acc, int i) { return acc + max(i - mid, 0); });
            if (total >= orders) left = mid + 1;
            else right = mid - 1;
        }
        long result = 0;
        for (int i : inventory) 
        {
            if (i > left) 
            {
                result += (i + left + 1) * 1L * (i - left) >> 1;
                result %= MOD;
                orders -= i - left;
            }
        }
        result += left * 1L * orders;
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxProfit(int[] inventory, int orders) {
        int MOD = 1_000_000_007, right = 0, left = 0;
        for (int i : inventory) right = Math.max(right, i);
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (check(inventory, orders, mid)) left = mid + 1;
            else right = mid - 1;
        }
        long result = 0;
        for (int i : inventory) {
            if (i > left) {
                result += (i + left + 1) * 1L * (i - left) >> 1;
                result %= MOD;
                orders -= i - left;
            }
        }
        result += left * 1L * orders;
        return (int) (result % MOD);
    }
    
    private boolean check(int[] inventory, int orders, int mid) {
        int s = 0;
        for (int i : inventory) if ((s = s + Math.max(i - mid, 0)) > orders) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProfit(self, inventory: List[int], orders: int) -> int:
        MOD, arr, result = 10 ** 9 + 7, list((c := Counter(inventory)).keys()), 0
        arr.sort()
        while orders and arr:
            count, second_last = c[last := arr[-1]], 0 if len(arr) <= 1 else arr[-2]
            if (last - second_last) * count <= orders:
                orders -= (last - second_last) * count
                result += ((second_last + 1 + last) * (last - second_last) >> 1) * count
                c[second_last] += count
            else:
                result += ((last * (take := orders // count)) - (take * (take - 1) >> 1)) * count + (last - take) * (orders % count)
                break
            arr.pop()
        return result % MOD
```
