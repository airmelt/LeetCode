# 91 Decode Ways 解码方法

__Description__:
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a non-empty string containing only digits, determine the total number of ways to decode it.

__Example:__

Example 1:

Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).

Example 2:

Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

__题目描述__:
一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。

__示例 :__

示例 1:

输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。

示例 2:

输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。

__思路__:

动态规划
dp[i + 1] = s[i] == '0' ? 0 : dp[i]
如果前一位和该位在 1-26之间:
dp[i + 1] = dp[i] + dp[i - 1]
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numDecodings(string s) 
    {
        int result = s.size() == 0 ? (s[0] == '0' ? 0 : 1) : 1;
        int one = result, two = 0;
        for (int i = 0; i < s.size(); i++)
        {
            result = s[i] == '0' ? 0 : one;
            if (i > 0 and (s[i - 1] == '1' or s[i - 1] == '2' and s[i] < '7')) result += two;
            two = one;
            one = result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numDecodings(String s) {
        int result = s.length() == 0 ? (s.charAt(0) == '0' ? 0 : 1) : 1;
        int one = result, two = 0;
        for (int i = 0; i < s.length(); i++)
        {
            result = s.charAt(i) == '0' ? 0 : one;
            if (i > 0 && (s.charAt(i - 1) == '1' || s.charAt(i - 1) == '2' && s.charAt(i) < '7')) result += two;
            two = one;
            one = result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numDecodings(self, s: str) -> int:
        last, result = 1, int(s[0] != '0')
        for i in range(1, len(s)):
            last, result = result, last * (9 < int(s[i - 1:i + 1]) < 27) + result * (int(s[i]) > 0)
        return result
```
