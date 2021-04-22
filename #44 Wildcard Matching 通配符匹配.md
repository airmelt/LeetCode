# 44 Wildcard Matching 通配符匹配

__Description__:
Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '\*'.

'?' Matches any single character.
'\*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

__Note:__

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like ? or \*.

__Example:__

Example 1:

Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

Example 2:

Input:
s = "aa"
p = "\*"
Output: true
Explanation: '\*' matches any sequence.

Example 3:

Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.

Example 4:

Input:
s = "adceb"
p = "\*a\*b"
Output: true
Explanation: The first '\*' matches the empty sequence, while the second '\*' matches the substring "dce".

Example 5:

Input:
s = "acdcb"
p = "a\*c?b"
Output: false

__题目描述__:
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '\*' 的通配符匹配。

'?' 可以匹配任何单个字符。
'\*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。

__说明:__

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 \*。

__示例 :__

示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

示例 2:

输入:
s = "aa"
p = "\*"
输出: true
解释: '\*' 可以匹配任意字符串。

示例 3:

输入:
s = "cb"
p = "?a"
输出: false
解释: '?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。

示例 4:

输入:
s = "adceb"
p = "\*a\*b"
输出: true
解释: 第一个 '\*' 可以匹配空字符串, 第二个 '\*' 可以匹配字符串 "dce".

示例 5:

输入:
s = "acdcb"
p = "a\*c?b"
输入: false

__思路__:

1. 动态规划
dp[i][j]表示 p[:i]和 s[:j]匹配, 初始化表示空字符串匹配, 即 dp[0][0] = true
分为三种情况:
1.1. p[i - 1] == '?', 则该字符一定匹配, 此时 dp[i][j] = dp[i - 1][j - 1]
1.2. p[i - 1] == '\*', 则该字符和 s的前面所有字符(0到 j - 1)都可以匹配, 先初始化 dp[i][0] = dp[i - 1][0], 再循环用或连接
1.3. p[i - 1]为其他字符, 则观察是否有p[i - 1] == s[j - 1]和 dp[i - 1][j - 1]
时间复杂度O(mn), 空间复杂度O(mn), 其中 m, n分别为两个字符串的长度
2. 贪心 & 回溯
注意到这里的题目与正则表达式不同, 这里的 '\*'可以匹配任意字符
用双指针 i, j标记表示 s[:i]和 p[:j]匹配, ipoint, jpoint记录 '\*'的位置

- 2.1 如果字符相等或者 p[j] == '?', 说明匹配, 两个指针后移
- 2.2 如果 p[j] == '\*', 匹配但是不知道匹配多少个字符, 记录下此时指针的位置, 只后移 j指针
- 2.3 如果不匹配, 但是 ipoint > -1, 说明之前有匹配过 '\*', 回溯到上一个 '\*'的位置, 重新标记 i, j指针的位置
- 2.4 否则匹配失败返回 false
时间复杂度O(nlgm), 空间复杂度O(1), 其中 m, n分别为两个字符串的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isMatch(string s, string p) 
    {
        vector<vector<bool>> dp(p.size() + 1, vector<bool>(s.size() + 1, false));
        dp[0][0] = true;
        for (int i = 1; i <= p.size(); i++) 
        {
            if (p[i - 1] == '*') 
            {
                dp[i][0] = dp[i - 1][0];
                for (int j = 1; j <= s.size(); j++) dp[i][j] = dp[i - 1][j] or dp[i][j - 1];
            }
            else if (p[i - 1] == '?') for (int j = 1; j <= s.size(); j++) dp[i][j] = dp[i - 1][j - 1];
            else for (int j = 1; j <= s.size(); j++) dp[i][j] = (s[j - 1] == p[i - 1]) and dp[i - 1][j - 1];
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isMatch(String s, String p) {
        if (p == null || p.isEmpty()) return s == null || s.isEmpty();
        int i = 0, j = 0, ipoint = -1, jpoint = -1, slen = s.length(), plen = p.length();
        while (i < slen) {
            if (j < plen && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?')) {
                ++i;
                ++j;
            } else if (j < plen && p.charAt(j) == '*') {
                ipoint = i;
                jpoint = j++;
            } else if (ipoint > -1) {
                i = ++ipoint;
                j = jpoint + 1;
            }else return false;
        }
        while (j < plen && p.charAt(j) == '*') ++j;
        return j == plen;
    }
}
```

__Python__:

```Python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        dp = [[False for _ in range(len(s) + 1)] for _ in range(len(p) + 1)]
        dp[0][0] = True
        for i in range(1, len(p) + 1):
            if p[i - 1] == '?':
                for j in range(1, len(s) + 1):
                    dp[i][j] = dp[i - 1][j - 1]
            elif p[i - 1] == '*':
                dp[i][0] = dp[i - 1][0]
                for j in range(1, len(s) + 1):
                    dp[i][j] = dp[i][j - 1] or dp[i - 1][j]
            else:
                for j in range(1, len(s) + 1):
                    dp[i][j] = (s[j - 1] == p[i - 1]) and dp[i - 1][j - 1]
        return dp[-1][-1]
```
