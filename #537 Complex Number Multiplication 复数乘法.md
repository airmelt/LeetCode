# 535 Encode and Decode TinyURL TinyURL 的加密与解密

__Description__:
Given two strings representing two complex numbers.

You need to return a string representing their multiplication. Note i2 = -1 according to the definition.

__Example:__

Example 1:
Input: "1+1i", "1+1i"
Output: "0+2i"
Explanation: (1 + i) \* (1 + i) = 1 + i2 + 2 \* i = 2i, and you need convert it to the form of 0+2i.

Example 2:
Input: "1+-1i", "1+-1i"
Output: "0+-2i"
Explanation: (1 - i) \* (1 - i) = 1 + i2 - 2 \* i = -2i, and you need convert it to the form of 0+-2i.

__Note:__

The input strings will not have extra blank.
The input strings will be given in the form of a+bi, where the integer a and b will both belong to the range of [-100, 100]. And the output should be also in this form.

__题目描述__:
给定两个表示复数的字符串。

返回表示它们乘积的字符串。注意，根据定义 i2 = -1 。

__示例 :__

示例 1:

输入: "1+1i", "1+1i"
输出: "0+2i"
解释: (1 + i) \* (1 + i) = 1 + i2 + 2 \* i = 2i ，你需要将它转换为 0+2i 的形式。

示例 2:

输入: "1+-1i", "1+-1i"
输出: "0+-2i"
解释: (1 - i) \* (1 - i) = 1 + i2 - 2 \* i = -2i ，你需要将它转换为 0+-2i 的形式。

__注意:__

输入字符串不包含额外的空格。
输入字符串将以 a+bi 的形式给出，其中整数 a 和 b 的范围均在 [-100, 100] 之间。输出也应当符合这种形式。

__思路__:

利用 split() 转换为两个数字 a + bi 和 c + di
A = a \* c - b \* d, B = a \* d + b \* c
输出 A + Bi即可
时间复杂度 O(1), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string complexNumberMultiply(string a, string b) 
    {
        int A, B, C, D;
        sscanf(a.c_str(), "%d+%di", &A, &B);
        sscanf(b.c_str(), "%d+%di", &C, &D);
        return to_string(A * C - B * D) + "+" + to_string(A * D + C * B) + "i";
    }
};
```

__Java__:

```Java
class Solution {
    public String complexNumberMultiply(String a, String b) {
        return (Integer.parseInt(a.split("\\+")[0]) * Integer.parseInt(b.split("\\+")[0]) - Integer.parseInt(a.split("\\+")[1].split("i")[0]) * Integer.parseInt(b.split("\\+")[1].split("i")[0])) + "+" + (Integer.parseInt(a.split("\\+")[0]) * Integer.parseInt(b.split("\\+")[1].split("i")[0]) + Integer.parseInt(a.split("\\+")[1].split("i")[0]) * Integer.parseInt(b.split("\\+")[0])) + "i";
    }
}
```

__Python__:

```Python
class Solution:
    def complexNumberMultiply(self, a: str, b: str) -> str:
        return str(int((c := a.split('+'))[0]) * int((d := b.split('+'))[0]) - int(c[1][:-1]) * int(d[1][:-1])) + '+' + str((int(c[0]) * int(d[1][:-1])) + (int(d[0]) * int(c[1][:-1]))) + 'i'
```
