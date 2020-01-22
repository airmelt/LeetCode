__Description__:
Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

__Note:__

Only the space character ' ' is considered as whitespace character.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. If the numerical value is out of the range of representable values, INT_MAX (231 − 1) or INT_MIN (−231) is returned.

__Example:__
Example 1:

Input: "42"
Output: 42

Example 2:

Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.

Example 3:

Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

Example 4:

Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.

Example 5:

Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.

__题目描述__:
请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

__说明：__

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

__示例 :__
示例 1:

输入: "42"
输出: 42

示例 2:

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

示例 3:

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

示例 4:

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。

示例 5:

输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。

__思路__:
记录下标, 先找到第一个不为空格的字符, 记录正负, 再记录数值, 可用 long或者字符串记录数字, 最后再与 32位整数比较即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int myAtoi(string str) 
    {
        int index = 0, positive = 1, n = str.size();
        long temp = 0;
        while (index < n && str[index] == ' ') ++index;
        if (index == n) return 0;
        if (str[index] == '+') ++index;
        else if (str[index] == '-')
        {
            positive = -1;
            ++index;
        }
        while (index < n && str[index] >= '0' && str[index] <= '9')
        {
            temp *= 10;
            temp += str[index++] - '0';
            if (temp > INT_MAX) 
            {
                if (positive == 1) return INT_MAX;
                else return INT_MIN;
            }
        }
        return temp * positive;
    }
};
```

__Java__:
```Java
class Solution {
    public int myAtoi(String str) {
        int index = 0, positive = 1, n = str.length();
        long temp = 0;
        while (index < n && str.charAt(index) == ' ') ++index;
        if (index == n) return 0;
        if (str.charAt(index) == '+') ++index;
        else if (str.charAt(index) == '-')
        {
            positive = -1;
            ++index;
        }
        while (index < n && str.charAt(index) >= '0' && str.charAt(index) <= '9')
        {
            temp *= 10;
            temp += str.charAt(index++) - '0';
            if (temp > Integer.MAX_VALUE) 
            {
                if (positive == 1) return Integer.MAX_VALUE;
                else return Integer.MIN_VALUE;
            }
        }
        return (int)temp * positive;
    }
}
```

__Python__:
```Python
class Solution:
    def myAtoi(self, str: str) -> int:
        return max(min(int(*re.findall('^[\+\-]?\d+', str.lstrip())), (1 << 31) - 1), -(1 << 31))
```