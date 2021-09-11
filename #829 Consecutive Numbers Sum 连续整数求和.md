# 829 Consecutive Numbers Sum 连续整数求和

__Description__:
Given an integer n, return the number of ways you can write n as the sum of consecutive positive integers.

__Example:__

Example 1:

Input: n = 5
Output: 2
Explanation: 5 = 2 + 3

Example 2:

Input: n = 9
Output: 3
Explanation: 9 = 4 + 5 = 2 + 3 + 4

Example 3:

Input: n = 15
Output: 4
Explanation: 15 = 8 + 7 = 4 + 5 + 6 = 1 + 2 + 3 + 4 + 5

__Constraints:__

1 <= n <= 10^9

__题目描述__:
给定一个正整数 N，试求有多少组连续正整数满足所有数字之和为 N?

__示例 :__

示例 1:

输入: 5
输出: 2
解释: 5 = 5 = 2 + 3，共有两组连续整数([5],[2,3])求和后为 5。

示例 2:

输入: 9
输出: 3
解释: 9 = 9 = 4 + 5 = 2 + 3 + 4

示例 3:

输入: 15
输出: 4
解释: 15 = 15 = 8 + 7 = 4 + 5 + 6 = 1 + 2 + 3 + 4 + 5

__说明:__
1 <= N <= 10 ^ 9

__思路__:

数学
本题在 Google kickstart 上有一个变体, 参考 [Alien Generator - Kick Start (codingcompetitions.withgoogle.com)](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000435c44/00000000007ec1cb)
令 n = x + (x + 1) + (x + 2) + ... + (x + k) = kx + k * (k + 1) / 2
那么有 2n = k \* (2x + k + 1), 利用整除的性质可以在 O(n ^ 1/2) 求解
在 2 ~ n ^ 1 / 2 范围内查找 (n - i \* (i - 1) / 2) % i == 0 的 i 的个数
另外首先 n 本身可以构成 n
若 2 个数可以构成 n, n / 2 + n / 2 + 1 = n, 即 (n - 1) % 2 == 0
若 3 个数可以构成 n, n / 3 + n / 3 + 1 + n / 3 + 2 = n, 即 (n - 1 - 2) % 3 == 0
以此类推, 可以用迭代的方式求解
时间复杂度为 O(n ^ 1/2), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int consecutiveNumbersSum(int n) 
    {
        int result = 1;
        for (int i = 2; i < sqrt(n << 1); ++i) if (!((n - (i * (i - 1) >> 1)) % i)) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int consecutiveNumbersSum(int n) {
        int result = 0;
        for (int i = 1; n > 0; n -= i++) result += (n % i == 0 ? 1 : 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def consecutiveNumbersSum(self, n: int) -> int:
        result, i = 0, 1
        while n > 0:
            result += not (n % i)
            n -= i
            i += 1
        return result
```
