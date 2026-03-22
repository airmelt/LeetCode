# 1143 Longest Common Subsequence 最长公共子序列

__Description__:
Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

__Example:__

Example 1:

Input: text1 = "abcde", text2 = "ace"
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.

Example 2:

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.

Example 3:

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.

__Constraints:__

1 <= text1.length, text2.length <= 1000
text1 and text2 consist of only lowercase English characters.

__题目描述__:
给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。
两个字符串的 公共子序列 是这两个字符串所共同拥有的子序列。

__示例 :__

示例 1：

输入：text1 = "abcde", text2 = "ace"
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。

示例 2：

输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。

示例 3：

输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。

__提示:__

1 <= text1.length, text2.length <= 1000
text1 和 text2 仅由小写英文字符组成。

__思路__:

动态规划
设 dp[i][j] 表示从 i 到 n - 1 的石头中选择 M = j 时可以取得的最大分数
因为爱丽丝是从 1 开始, 从第 0 堆开始取, 最后爱丽丝的得分应当为 dp[0][1]
从后往前遍历, 因为 j 需要从 1 开始遍历
如果 i + 2 * j >= n, 说明可以全部取完, 此时 dp[i][j] = sum(piles[i:])
否则在 [1, 2 * j] 中选择一个 k 使得下个人取得的分数最小
dp[i][j] = sum(piles[i:]) - rival
其中 rival = dp[i + k][max(k, j)]
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestCommonSubsequence(string text1, string text2) 
    {
        int m = text1.size(), n = text2.size();
        vector<int> dp(n + 1);
        for (int i = 1; i <= m; i++)
        {
            vector<int> cur(dp);
            for (int j = 1; j <= n; j++) cur[j] = text1[i - 1] == text2[j - 1] ? dp[j - 1] + 1 : max(cur[j - 1], dp[j]);
            dp = cur;
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length(), dp[][] = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = text1.charAt(i - 1) == text2.charAt(j - 1) ? dp[i - 1][j - 1] + 1 : Math.max(dp[i - 1][j], dp[i][j - 1]);
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                dp[i][j] = dp[i - 1][j - 1] + 1 if text1[i - 1] == text2[j - 1] else max(dp[i - 1][j], dp[i][j - 1])
        return dp[-1][-1]
```
