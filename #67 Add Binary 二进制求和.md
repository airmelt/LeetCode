# 67 Add Binary 二进制求和

__Description__:
Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters 1 or 0.

__Example__:

Example 1:
Input: a = "11", b = "1"
Output: "100"

Example 2:
Input: a = "1010", b = "1011"
Output: "10101"

__题目描述__:
给定两个二进制字符串，返回他们的和（用二进制表示）。

输入为非空字符串且只包含数字 1 和 0。

__示例__:

示例 1:
输入: a = "11", b = "1"
输出: "100"

示例 2:
输入: a = "1010", b = "1011"
输出: "10101"

__思路__:

设置进位符号, 从后往前遍历
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string addBinary(string a, string b) 
    {
        string result = "";
        int i = a.size() - 1, j = b.size() - 1, carry = 0;
        while (i >= 0 or j >= 0) 
        {
            int p = i >= 0 ? a[i--] - '0' : 0, q = j >= 0 ? b[j--] - '0' : 0;
            int temp = p + q + carry;
            result = to_string(temp % 2) + result;
            carry = temp / 2;
        }
        return carry == 1 ? "1" + result : result;
    }
};
```

__Java__:

```Java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder result = new StringBuilder();
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;
        while (i >= 0 || j >= 0) {
            int p = i >= 0 ? a.charAt(i--) - '0' : 0;
            int q = j >= 0 ? b.charAt(j--) - '0' : 0;
            int temp = p + q + carry;
            result.append(temp % 2);
            carry = temp / 2;
        }
        if (carry == 1) result.append(carry);
        return result.reverse().toString();
    }
}
```

__Python__:

```Python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        return bin(int(a, 2) + int(b, 2))[2:]
```
