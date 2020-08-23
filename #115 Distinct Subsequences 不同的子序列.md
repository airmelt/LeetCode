__Description__:
Given a string S and a string T, count the number of distinct subsequences of S which equals T.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

It's guaranteed the answer fits on a 32-bit signed integer.

__Example:__
Example 1:

Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)
```
rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

Example 2:

Input: S = "babgbag", T = "bag"
Output: 5
Explanation:
As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)
```
babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```

__题目描述__:
给定一个字符串 S 和一个字符串 T，计算在 S 的子序列中 T 出现的个数。

一个字符串的一个子序列是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

题目数据保证答案符合 32 位带符号整数范围。

__示例 :__
示例 1：

输入：S = "rabbbit", T = "rabbit"
输出：3
解释：

如下图所示, 有 3 种可以从 S 中得到 "rabbit" 的方案。
(上箭头符号 ^ 表示选取的字母)
```
rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

示例 2：

输入：S = "babgbag", T = "bag"
输出：5
解释：

如下图所示, 有 5 种可以从 S 中得到 "bag" 的方案。 
(上箭头符号 ^ 表示选取的字母)
```
babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
  ^  ^^
babgbag
    ^^^
```

__思路__:
动态规划
dp[i][j]表示 s[:i]中包含 t[:j]的子序列个数
转移方程: 当 s[i] == t[j], dp[i + 1][j + 1] = dp[i][j] + dp[i][j + 1]
否则 dp[i + 1][j + 1] = dp[i + 1][j]
时间复杂度O(mn), 空间复杂度O(max(m, n))

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int numDistinct(string s, string t) 
    {
        vector<long> dp(t.size() + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < s.size(); i++) for (int j = t.size() - 1; j > -1; j--) if (s[i] == t[j]) dp[j + 1] += dp[j];
        return (int)dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public int numDistinct(String s, String t) {
        int dp[][] = new int[s.length() + 1][t.length() + 1], m = s.length(), n = t.length();
        for (int i = 0; i < m + 1; i++) dp[i][0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (s.charAt(i) == t.charAt(j)) dp[i + 1][j + 1] = dp[i][j] + dp[i][j + 1];
                else dp[i + 1][j + 1] = dp[i][j + 1];
            }
        }
        return dp[m][n];
    }
}
```

__Python__:
```Python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0 for _ in range(len(s) + 1)] for _ in range(len(t) + 1)]
        for i in range(len(s)):
            dp[0][i] = 1
        for i in range(len(t)):
            for j in range(len(s)):
                if t[i] == s[j]:
                    dp[i + 1][j + 1] = dp[i + 1][j] + dp[i][j]
                else:
                    dp[i + 1][j + 1] = dp[i + 1][j]
        return dp[-1][-1]
```