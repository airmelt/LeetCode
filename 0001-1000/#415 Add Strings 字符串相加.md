# 415 Add Strings 字符串相加

__Description__:
Given two non-negative integers num1 and num2 represented as string, return the sum of num1 and num2.

__Note:__

The length of both num1 and num2 is < 5100.
Both num1 and num2 contains only digits 0-9.
Both num1 and num2 does not contain any leading zero.
You must not use any built-in BigInteger library or convert the inputs to integer directly.

__题目描述__:
给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

__注意：__

num1 和num2 的长度都小于 5100.
num1 和num2 都只包含数字 0-9.
num1 和num2 都不包含任何前导零。
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。

__思路__:

参考[LeetCode #67 Add Binary 二进制求和](https://www.jianshu.com/p/be1b75a32661)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string addStrings(string num1, string num2) 
    {
        string result = "";
        int carry = 0, i = num1.size() - 1, j = num2.size() - 1;
        while (i >= 0 or j >= 0 or carry != 0) 
        {
            if (i >= 0) carry += num1[i--] - '0';
            if (j >= 0) carry += num2[j--] - '0';
            result += to_string(carry % 10);
            carry /= 10;
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder sb = new StringBuilder();
        int carry = 0, i = num1.length() - 1, j = num2.length() - 1;
        while (i >= 0 || j >= 0 || carry != 0) {
            if (i >= 0) carry += num1.charAt(i--) - '0';
            if (j >= 0) carry += num2.charAt(j--) - '0';
            sb.append(carry % 10);
            carry /= 10;
        }
        return sb.reverse().toString();
    }
}
```

__Python__:

```Python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        num1, num2, max_length, result, carry = num1[::-1], num2[::-1], max(len(num1), len(num2)), '', 0
        for i in range(max_length):
            first, second = int(num1[i]) if i < len(num1) else 0, int(num2[i]) if i < len(num2) else 0
            result += str((first + second + carry) % 10)
            carry = int((first + second + carry) / 10)
        if carry > 0:
            result += str(carry)
        return result[::-1]
```
