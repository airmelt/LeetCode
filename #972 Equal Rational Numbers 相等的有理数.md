# 972 Equal Rational Numbers 相等的有理数

__Description__:
Given two strings s and t, each of which represents a non-negative rational number, return true if and only if they represent the same number. The strings may use parentheses to denote the repeating part of the rational number.

A rational number can be represented using up to three parts: \<IntegerPart>, \<NonRepeatingPart>, and a \<RepeatingPart>. The number will be represented in one of the following three ways:

\<IntegerPart>
For example, 12, 0, and 123.
\<IntegerPart>\<.>\<NonRepeatingPart>
For example, 0.5, 1., 2.12, and 123.0001.
\<IntegerPart>\<.>\<NonRepeatingPart>\<(>\<RepeatingPart><)>
For example, 0.1(6), 1.(9), 123.00(1212).
The repeating portion of a decimal expansion is conventionally denoted within a pair of round brackets. For example:

1/6 = 0.16666666... = 0.1(6) = 0.1666(6) = 0.166(66).

__Example:__

Example 1:

Input: s = "0.(52)", t = "0.5(25)"
Output: true
Explanation: Because "0.(52)" represents 0.52525252..., and "0.5(25)" represents 0.52525252525..... , the strings represent the same number.

Example 2:

Input: s = "0.1666(6)", t = "0.166(66)"
Output: true

Example 3:

Input: s = "0.9(9)", t = "1."
Output: true
Explanation: "0.9(9)" represents 0.999999999... repeated forever, which equals 1.  [See this link for an explanation.]
"1." represents the number 1, which is formed correctly: (IntegerPart) = "1" and (NonRepeatingPart) = "".

__Constraints:__

Each part consists only of digits.
The \<IntegerPart> does not have leading zeros (except for the zero itself).
1 <= \<IntegerPart>.length <= 4
0 <= \<NonRepeatingPart>.length <= 4
1 <= \<RepeatingPart>.length <= 4

__题目描述__:
给定两个字符串 S 和 T，每个字符串代表一个非负有理数，只有当它们表示相同的数字时才返回 true；否则，返回 false。字符串中可以使用括号来表示有理数的重复部分。

通常，有理数最多可以用三个部分来表示：整数部分 \<IntegerPart>、小数非重复部分 \<NonRepeatingPart> 和小数重复部分 <(>\<RepeatingPart><)>。数字可以用以下三种方法之一来表示：

\<IntegerPart>（例：0，12，123）
\<IntegerPart><.>\<NonRepeatingPart> （例：0.5，2.12，2.0001）
\<IntegerPart><.>\<NonRepeatingPart><(>\<RepeatingPart><)>（例：0.1(6)，0.9(9)，0.00(1212)）
十进制展开的重复部分通常在一对圆括号内表示。例如：

1 / 6 = 0.16666666... = 0.1(6) = 0.1666(6) = 0.166(66)

0.1(6) 或 0.1666(6) 或 0.166(66) 都是 1 / 6 的正确表示形式。

__示例 :__

示例 1：

输入：S = "0.(52)", T = "0.5(25)"
输出：true
解释：因为 "0.(52)" 代表 0.52525252...，而 "0.5(25)" 代表 0.52525252525.....，则这两个字符串表示相同的数字。

示例 2：

输入：S = "0.1666(6)", T = "0.166(66)"
输出：true

示例 3：

输入：S = "0.9(9)", T = "1."
输出：true
解释：
"0.9(9)" 代表 0.999999999... 永远重复，等于 1 。[有关说明，请参阅此链接]
"1." 表示数字 1，其格式正确：(IntegerPart) = "1" 且 (NonRepeatingPart) = "" 。

__提示:__

每个部分仅由数字组成。
整数部分 \<IntegerPart> 不会以 2 个或更多的零开头。（对每个部分的数字没有其他限制）。
1 <= \<IntegerPart>.length <= 4
0 <= \<NonRepeatingPart>.length <= 4
1 <= \<RepeatingPart>.length <= 4

__思路__:

模拟
按照小数点和括号将这个数字分为 3 个部分
分别是整数部分, 不循环小数部分和循环小数部分
分别转化为 double 类型
再做差比较大小
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isRationalEqual(string s, string t) 
    {
        return fabs(calculate(s) - calculate(t)) < 1e-8;
    }
private:
    double calculate(string& s) 
    {
        int pos = s.find("("), n = s.size();
        if (pos == -1) return stof(s.c_str());
        string result(s.substr(0, pos)), repeat_part(s.substr(pos + 1, n - pos - 2));
        while (result.size() <= 12) result += repeat_part;
        return stod(result);
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isRationalEqual(String s, String t) {
        return Math.abs(calculate(s) - calculate(t)) < 0.000_000_01;
    }
    
    private double calculate(String s) {
        int pos = s.indexOf('('), n = s.length();
        if (pos == -1) return Double.valueOf(s);
        StringBuilder result = new StringBuilder(s.substring(0, pos)), repeatPart = new StringBuilder(s.substring(pos + 1, n - 1));
        while (result.length() <= 12) result.append(repeatPart);
        return Double.valueOf(result.toString());
    }
}
```

__Python__:

```Python
class Solution:
    def isRationalEqual(self, s: str, t: str) -> bool:
        return abs(float(s[:pos1] + s[pos1 + 1:-1] * 3 if (pos1 := s.find('(')) != -1 else s) - float(t[:pos2] + t[pos2 + 1:-1] * 3 if (pos2 := t.find('(')) != -1 else t)) < 10 ** -8
```
