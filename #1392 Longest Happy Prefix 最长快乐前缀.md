# 1392 Longest Happy Prefix 最长快乐前缀

__Description:__

A string is called a happy prefix if is a non-empty prefix which is also a suffix (excluding itself).

Given a string s, return the longest happy prefix of s. Return an empty string "" if no such prefix exists.

__Example:__

Example 1:

Input: s = "level"
Output: "l"
Explanation: s contains 4 prefix excluding itself ("l", "le", "lev", "leve"), and suffix ("l", "el", "vel", "evel"). The largest prefix which is also suffix is given by "l".

Example 2:

Input: s = "ababab"
Output: "abab"
Explanation: "abab" is the largest prefix which is also suffix. They can overlap in the original string.

__Constraints:__

1 <= s.length <= 10^5
s contains only lowercase English letters.

__题目描述:__

「快乐前缀」 是在原字符串中既是 非空 前缀也是后缀（不包括原字符串自身）的字符串。

给你一个字符串 s，请你返回它的 最长快乐前缀。如果不存在满足题意的前缀，则返回一个空字符串 "" 。

__示例:__

示例 1：

输入：s = "level"
输出："l"
解释：不包括 s 自己，一共有 4 个前缀（"l", "le", "lev", "leve"）和 4 个后缀（"l", "el", "vel", "evel"）。最长的既是前缀也是后缀的字符串是 "l" 。

示例 2：

输入：s = "ababab"
输出："abab"
解释："abab" 是最长的既是前缀也是后缀的字符串。题目允许前后缀在原字符串中重叠。

__提示：__

1 <= s.length <= 10^5
s 只含有小写英文字母

__思路:__

```text
KMP 算法
题意需要求取的最长快乐前缀就是既为前缀又为后缀但步包括 s 自身的最长子串
可以初始化 next 数组为 -1 或 0
步骤稍有不同
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string longestPrefix(string s) 
    {
        int n = s.size();
        vector<int> next(n);
        for (int i = 1, j = 0; i < n; i++) 
        {
            while (j and s[i] != s[j]) j = next[j - 1];
            j += s[i] == s[j];
            next[i] = j;
        }
        return s.substr(0, next.back());
    }
};
```

__Java__:

```Java
class Solution {
    public String longestPrefix(String s) {
        int n = s.length(), next[] = new int[n];
        for (int i = 1, j = 0; i < n; i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) j = next[j - 1];
            if (s.charAt(i) == s.charAt(j)) ++j;
            next[i] = j;
        }
        return s.substring(0, next[n - 1]);
    }
}
```

__Python__:

```Python
class Solution:
    def longestPrefix(self, s: str) -> str:
        nxt, j = [0] * (n := len(s)), 0
        for i in range(1, n):
            while j and s[i] != s[j]:
                j = nxt[j - 1]
            j += s[i] == s[j]
            nxt[i] = j
        return s[:nxt[-1]]
```
