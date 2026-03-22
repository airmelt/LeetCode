# 166 Fraction to Recurring Decimal 分数到小数

__Description__:
Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.

If multiple answers are possible, just return any of them.

__Example:__

Example 1:

Input: numerator = 1, denominator = 2
Output: "0.5"

Example 2:

Input: numerator = 2, denominator = 1
Output: "2"

Example 3:

Input: numerator = 2, denominator = 3
Output: "0.(6)"

__题目描述__:
给定两个整数，分别表示分数的分子 numerator 和分母 denominator，以字符串形式返回小数。

如果小数部分为循环小数，则将循环的部分括在括号内。

__示例 :__

示例 1:

输入: numerator = 1, denominator = 2
输出: "0.5"

示例 2:

输入: numerator = 2, denominator = 1
输出: "2"

示例 3:

输入: numerator = 2, denominator = 3
输出: "0.(6)"

__思路__:

模拟除法运算

1. 如果分子为 0直接输出 0
2. 用异或决定是否添加负号, 将分子分母取绝对值
3. 用一个变量 remain记录取余结果, 每次循环 remain自乘 10之后去➗分子, remain记录取余结果
4. 用 hash记录 remain出现的位置和值, 一旦出现过, 说明进入循环, 可以添加括号
5. 注意边界条件
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string fractionToDecimal(int numerator, int denominator) 
    {
        if (!numerator or !denominator) return "0";
        string result = "";
        if (numerator < 0 ^ denominator < 0) result += "-";
        long long up = llabs(static_cast<long long>(numerator)), down = llabs(static_cast<long long>(denominator));
        result += to_string(up / down);
        long long remain = up % down;
        if (!remain) return result;
        result += ".";
        unordered_map<int,int> m;
        while (remain) 
        {
            if (m.find(remain) != m.end()) 
            {
                result.insert(m[remain], "(");
                result += ")";
                break;
            }
            m[remain] = result.size();
            remain *= 10;
            result += to_string(remain / down);
            remain %= down;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if (numerator == 0 || denominator == 0) return "0";
        StringBuilder result = new StringBuilder();
        if (numerator < 0 ^ denominator < 0) result.append("-");
        long up = Math.abs(Long.valueOf(numerator)), down = Math.abs(Long.valueOf(denominator));
        result.append(String.valueOf(up / down));
        long remain = up % down;
        if (remain == 0) return result.toString();
        result.append(".");
        Map<Long, Integer> map = new HashMap<>();
        while (remain != 0) {
            if (map.containsKey(remain)) {
                result.insert(map.get(remain), "(");
                result.append(")");
                break;
            }
            map.put(remain, result.length());
            remain *= 10;
            result.append(String.valueOf(remain / down));
            remain %= down;
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def fractionToDecimal(self, numerator: int, denominator: int) -> str:
        if not numerator:
            return '0'
        result = ''
        if (numerator < 0) ^ (denominator < 0):
            result += '-'
        numerator, denominator = abs(numerator), abs(denominator)
        remain = numerator % denominator
        result += str(numerator // denominator)
        if not remain:
            return result
        result += '.'
        d = {}
        while remain:
            if remain in d:
                result = result[:d[remain]] + '(' + result[d[remain]:] + ')'
                break
            d[remain] = len(result)
            remain *= 10
            result += str(remain // denominator)
            remain %= denominator
        return result
```
