# 10 Regular Expression Matching 正则表达式匹配

__Description__:
Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '\*'.

'.' Matches any single character.
'\*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

__Note:__

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like . or \*.

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
p = "a\*"
Output: true
Explanation: '\*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

Example 3:

Input:
s = "ab"
p = ".\*"
Output: true
Explanation: ".\*" means "zero or more (\*) of any character (.)".

Example 4:

Input:
s = "aab"
p = "c\*a\*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".

Example 5:

Input:
s = "mississippi"
p = "mis\*is\*p\*."
Output: false

__题目描述__:
给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '\*' 的正则表达式匹配。

'.' 匹配任意单个字符
'\*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

__说明：__

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 \*。

__示例 :__
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

示例 2:

输入:
s = "aa"
p = "a\*"
输出: true
解释: 因为 '\*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。

示例 3:

输入:
s = "ab"
p = ".\*"
输出: true
解释: ".\*" 表示可匹配零个或多个（'\*'）任意字符（'.'）。

示例 4:

输入:
s = "aab"
p = "c\*a\*b"
输出: true
解释: 因为 '\*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。

示例 5:

输入:
s = "mississippi"
p = "mis\*is\*p\*."
输出: false

__思路__:

1. 双字符串匹配, 会产生大量子问题, 可以用动态规划解决
2. 首先定义 dp[i][j]表示 s[:i]和p[:j]匹配
3. 从 dp[i - 1][j - 1]出发, 观察 s[i]和 p[j]
  3.1 最简单的情况, s[i] == p[j], 则 dp[i][j] = dp[i - 1][j - 1]
  3.2 若 p[j] == ".", 也可以直接匹配, 则dp[i][j] = dp[i - 1][j - 1]
  3.3 若 p[j] == "*", 要匹配之前的字符 0个或多个, 这里又可以分两种情况, 一种是 p[j - 1] == s[i](p[j - 1] == "."也是归类到这一种), 那么可以匹配, 有dp[i][j] = dp[i - 1][j - 1], 另一种是 p[j - 1] != s[i], 这里可以看是否是 0个匹配, 要看 dp[i][j - 2], 所以有dp[i][j] = dp[i][j - 2]

时间复杂度O(mn), 空间复杂度O(mn), n, m分别为 s和 p的长度
注意到这里只使用了 dp两行相邻的, 可以将空间复杂度减小到 O(m)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isMatch(string s, string p) 
    {
        int n = s.size(), m = p.size();
        bool dp[n + 1][m + 1];
        for (int i = 0; i <= n; i++) for(int j = 0; j <= m; j++) dp[i][j] = false;
        dp[0][0] = true;
        for (int i = 2; i <= m; i++) if (p[i - 1] == '*') dp[0][i] = dp[0][i - 2];
        for (int i = 1; i <= n; i++)
        {
            for (int j = 1; j <= m; j++)
            {
                if (s[i - 1] == p[j - 1] or p[j - 1] == '.') dp[i][j] = dp[i - 1][j - 1];
                if (p[j - 1] == '*') if (j > 1) dp[i][j] = ((p[j - 2] == s[i - 1] or p[j - 2] == '.') and dp[i - 1][j]) or dp[i][j - 2];
            }
        }   
        return dp[n][m];
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isMatch(String s, String p) {
        return s.matches(p);
    }
}
```

__Python__:

```Python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        return re.match(p + '$', s)
```
