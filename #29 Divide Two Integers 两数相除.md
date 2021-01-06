# 29 Divide Two Integers 两数相除

__Description__:
Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

__Example:__

Example 1:

Input: dividend = 10, divisor = 3
Output: 3

Example 2:

Input: dividend = 7, divisor = -3
Output: -2

__Note:__

Both dividend and divisor will be 32-bit signed integers.
The divisor will never be 0.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 2^31 − 1 when the division result overflows.

__题目描述__:
给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

__示例 :__

示例 1:

输入: dividend = 10, divisor = 3
输出: 3
示例 2:

输入: dividend = 7, divisor = -3
输出: -2

__说明 :__

被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。本题中，如果除法结果溢出，则返回 2^31 − 1。

__思路__:

1. 可以从减法去考虑, 比如 10 - 3 = 7 > 3, 计数器加 1; 7 - 3 = 4 > 3, 计数器加 1; 4 - 3 = 1 < 3, 计数器加 1并结束循环, 那么结果就是 3
时间复杂度O(n), 空间复杂度O(1)
2. 可以考虑用类似二进制的处理方法, 每次寻找被除数最接近 n \* 2 ^ i的数, 结果加上这个数, 最多 32次可以结束, 比如 100 / 3 -> 100 / 32 = 3, result = 32  -> 100 - 32 * 3 = 4 -> 4 / 3 = 1, result = 33
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int divide(int dividend, int divisor) 
    {
        if (dividend == 0) return 0;
        if (dividend == INT_MIN and divisor == -1) return INT_MAX;
        if (dividend == INT_MIN and divisor == 1) return INT_MIN;
        bool sign = (dividend ^ divisor) < 0;
        int result = 0;
        long a = abs((long)(dividend)), b = abs((long)(divisor));
        for (int i = 31; i > -1; i--)
        {
            if ((a >> i) >= b) 
            {
                result += 1 << i;
                a -= b << i;
            }
        }
        return sign ? -result : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int divide(int dividend, int divisor) {
        return (dividend == Integer.MIN_VALUE && divisor == -1) ? Integer.MAX_VALUE : dividend / divisor;
    }
}
```

__Python__:

```Python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        return 2147483647 if dividend == -2147483648 and divisor == -1 else abs(dividend) // abs(divisor) * (-1 if dividend < 0 else 1) * (-1 if divisor < 0 else 1)
```
