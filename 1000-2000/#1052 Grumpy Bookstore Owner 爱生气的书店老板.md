# 1052 Grumpy Bookstore Owner 爱生气的书店老板

__Description__:
There is a bookstore owner that has a store open for n minutes. Every minute, some number of customers enter the store. You are given an integer array customers of length n where customers[i] is the number of the customer that enters the store at the start of the ith minute and all those customers leave after the end of that minute.

On some minutes, the bookstore owner is grumpy. You are given a binary array grumpy where grumpy[i] is 1 if the bookstore owner is grumpy during the ith minute, and is 0 otherwise.

When the bookstore owner is grumpy, the customers of that minute are not satisfied, otherwise, they are satisfied.

The bookstore owner knows a secret technique to keep themselves not grumpy for minutes consecutive minutes, but can only use it once.

Return the maximum number of customers that can be satisfied throughout the day.

__Example:__

Example 1:

Input: customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3
Output: 16
Explanation: The bookstore owner keeps themselves not grumpy for the last 3 minutes.
The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.

Example 2:

Input: customers = [1], grumpy = [0], minutes = 1
Output: 1

__Constraints:__

n == customers.length == grumpy.length
1 <= minutes <= n <= 2 * 10^4
0 <= customers[i] <= 1000
grumpy[i] is either 0 or 1.

__题目描述__:
有一个书店老板，他的书店开了 n 分钟。每分钟都有一些顾客进入这家商店。给定一个长度为 n 的整数数组 customers ，其中 customers[i] 是在第 i 分钟开始时进入商店的顾客的编号，所有这些顾客在第 i 分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。

当书店老板生气时，那一分钟的顾客就会不满意，若老板不生气则顾客是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 minutes 分钟不生气，但却只能使用一次。

请你返回 这一天营业下来，最多有多少客户能够感到满意 。

__示例 :__

示例 1：

输入：customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3
输出：16
解释：书店老板在最后 3 分钟保持冷静。
感到满意的最大客户数量 = 1 + 1 + 1 + 1 + 7 + 5 = 16.

示例 2：

输入：customers = [1], grumpy = [0], minutes = 1
输出：1

__提示:__

n == customers.length == grumpy.length
1 <= minutes <= n <= 2 * 10^4
0 <= customers[i] <= 1000
grumpy[i] == 0 or 1

__思路__:

滑动窗口
求出不使用技能情况下的总营业额
使用滑动窗口记录使用技能能获得的最大差值
总营业额加上差值就是结果
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) 
    {
        int s = 0, n = customers.size(), diff = 0, result = INT_MIN;
        for (int i = 0; i < n; i++) 
        {
            s += customers[i] * (grumpy[i] ^ 1);
            diff += customers[i] * grumpy[i];
            if (i >= minutes) diff -= customers[i - minutes] * grumpy[i - minutes];
            result = max(diff, result);
        }
        return result + s;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int s = 0, n = customers.length, diff = 0, result = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            s += customers[i] * (grumpy[i] ^ 1);
            diff += customers[i] * grumpy[i];
            if (i >= minutes) diff -= customers[i - minutes] * grumpy[i - minutes];
            result = Math.max(diff, result);
        }
        return result + s;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSatisfied(self, customers: List[int], grumpy: List[int], minutes: int) -> int:
        s, n, diff, result = 0, len(customers), 0, -float('inf')
        for i in range(n):
            s += customers[i] * (grumpy[i] ^ 1)
            diff += customers[i] * grumpy[i]
            if i >= minutes:
                diff -= customers[i - minutes] * grumpy[i - minutes]
            result = max(result, diff)
        return result + s
```
