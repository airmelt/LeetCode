# 7 Reverse Integer 整数反转

__Description__:
Given a 32-bit signed integer, reverse digits of an integer.

__Example__:

Example 1:

Input: 123
Output: 321

Example 2:

Input: -123
Output: -321

Example 3:

Input: 120
Output: 21

__Note__:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

__题目描述__:
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

__示例__:

示例 1:

输入: 123
输出: 321

示例 2:

输入: -123
输出: -321

示例 3:

输入: 120
输出: 21

__注意__:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

__思路__:

整型范围: -2147483648～2147483647

1. 可以用字符串反转, 注意判断是否溢出即可
2. 用取余和整数除法(地板除)
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int reverse(int x) 
    {
        int result = 0;
        while (x != 0) 
        {
            int last = x % 10;
            x /= 10;
            if (result > INT_MAX / 10 or (result == INT_MAX / 10 && last > 7)) return 0;
            if (result < INT_MIN / 10 or (rev == INT_MIN / 10 && last < -8)) return 0;
            result = result * 10 + last;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int last = x % 10;
            x /= 10;
            if (result > Integer.MAX_VALUE / 10 || (result == Integer.MAX_VALUE / 10 && last > 7)) return 0;
            if (result < Integer.MIN_VALUE / 10 || (result == Integer.MIN_VALUE / 10 && last == -9)) return 0;
            result = 10 * result + last;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def reverse(self, x: int) -> int:
        if not x:
            return 0
        str_x = str(x)
        x = ''
        if (str_x[0] == '-'):
            x += '-'
            x += str_x[::-1].lstrip('0').rstrip('-')
        else:
            x += str_x[::-1].lstrip('0')
        x = int(x)
        if x > 2 ** 31 - 1 or x < -2 ** 31:
            return 0
        return x
```
