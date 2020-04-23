__Description__:
Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

__Example:__
Example 1:

Input: num1 = "2", num2 = "3"
Output: "6"

Example 2:

Input: num1 = "123", num2 = "456"
Output: "56088"

__Note:__

1. The length of both num1 and num2 is < 110.
2. Both num1 and num2 contain only digits 0-9.
3. Both num1 and num2 do not contain any leading zero, except the number 0 itself.
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.

__题目描述__:
给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

__示例 :__
示例 1:

输入: num1 = "2", num2 = "3"
输出: "6"

示例 2:

输入: num1 = "123", num2 = "456"
输出: "56088"

__说明:__

1. num1 和 num2 的长度小于110。
2. num1 和 num2 只包含数字 0-9。
3. num1 和 num2 均不以零开头，除非是数字 0 本身。
4. 不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

__思路__:
参考[LeetCode #415 Add Strings 字符串相加](https://www.jianshu.com/p/39cc4837d2f3)
两个数相乘结果的位数最多不会超过两个数的位数之和
如 99 * 99 = 9801
每一次在第 i + j + 1位记录两个数的乘积
然后处理进位
最后将不必要的 0删除即可
时间复杂度O(mn), 空间复杂度O(m + n), 其中 m, n分别为两个字符串的长度

__代码__:
__C++__:
```C++
class Solution {
public:
    string multiply(string num1, string num2) {
        int m = num1.size() - 1, n = num2.size() - 1;
        vector<int> value(m + n + 2, 0);
        for (int i = m; i > -1; i--) for (int j = n; j > -1; j--) {
            int temp = (num1[i] - '0') * (num2[j] - '0');
            temp += value[i + j + 1];
            value[i + j] += temp / 10;
            value[i + j + 1] = temp % 10;
        }
        string result = "";
        int i = 0;
        while (i < m + n + 1 && value[i] == 0) ++i;
        for (; i < m + n + 2; i++) result += to_string(value[i]);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String multiply(String num1, String num2) {
        int m = num1.length() - 1, n = num2.length() - 1, value[] = new int[m + n + 2];
        for (int i = m; i > -1; i--) for (int j = n; j > -1; j--) {
            int temp = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
            temp += value[i + j + 1];
            value[i + j] += temp / 10;
            value[i + j + 1] = temp % 10;
        }
        int i = 0;
        StringBuilder sb = new StringBuilder();
        while (i < m + n + 1 && value[i] == 0) ++i;
        for (; i < m + n + 2; i++) sb.append(Integer.toString(value[i]));
        return sb.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        return str(int(num1) * int(num2))
```