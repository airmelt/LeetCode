# 516 Longest Palindromic Subsequence 最长回文子序列

__Description__:
Given a string s, find the longest palindromic subsequence's length in s.

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".

Example 2:

Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".

__Constraints:__

1 <= s.length <= 1000
s consists only of lowercase English letters.

__题目描述__:
给定一个字符串 s ，找到其中最长的回文子序列，并返回该序列的长度。可以假设 s 的最大长度为 1000 。

__示例 :__

示例 1:
输入:

"bbbab"
输出:

4
一个可能的最长回文子序列为 "bbbb"。

示例 2:
输入:

"cbbd"
输出:

2
一个可能的最长回文子序列为 "bb"。

__提示:__

1 <= s.length <= 1000
s 只包含小写英文字母

__思路__:

动态规划
dp[i][j]表示 s[i:j + 1]中的最长回文子序列
当 s[i] == s[j], dp[i][j] = dp[i + 1][j - 1] + 2
否则 dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
初始状态 dp[i][i] = 1
由于需要获得 dp[i + 1][j - 1], 需要从后往前遍历
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestPalindromeSubseq(string s) 
    {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int i = n - 1; i > -1; i--) for (int j = i + 1; j < n; j++) dp[i][j] = s[i] == s[j] ? dp[i + 1][j - 1] + 2 : max(dp[i + 1][j], dp[i][j - 1]);
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length(), dp[][] = new int[s.length()][s.length()];
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int i = n - 1; i > -1; i--) for (int j = i + 1; j < n; j++) dp[i][j] = s.charAt(i) == s.charAt(j) ? dp[i + 1][j - 1] + 2 : Math.max(dp[i + 1][j], dp[i][j - 1]);
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0] * (len(s)) for _ in range(len(s))]
        for i in range((n := len(s))):
            dp[i][i] = 1
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                dp[i][j] = dp[i + 1][j - 1] + 2 if s[i] == s[j] else max(dp[i + 1][j], dp[i][j - 1])
        return dp[0][-1]
```
