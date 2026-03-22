# 583 Delete Operation for Two Strings 两个字符串的删除操作

__Description__:

Given two strings word1 and word2, return the minimum number of steps required to make word1 and word2 the same.

In one step, you can delete exactly one character in either string.

__Example:__

Example 1:

Input: word1 = "sea", word2 = "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".

Example 2:

Input: word1 = "leetcode", word2 = "etco"
Output: 4

__Constraints:__

1 <= word1.length, word2.length <= 500
word1 and word2 consist of only lowercase English letters.

__题目描述__:
给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

__示例 :__

输入: "sea", "eat"
输出: 2
解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"

__提示:__

给定单词的长度不超过500。
给定单词中的字符只含有小写字母。

__思路__:

动态规划
找到两个字符串的最长相同子序列
dp[i][j] 表示 s1[:i] 和 s2[:j] 的最长相同子序列
动态转移方程: 当 s1[i] == s2[j], dp[i + 1][j + 1] = dp[i][j] + 1, 否则 dp[i + 1][j + 1] = max(dp[i][j + 1], dp[i + 1][j])
注意到每次更新只与前一行有关, 可以用滚动数组将空间复杂度优化到 O(n)
时间复杂度 O(mn), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minDistance(string word1, string word2) 
    {
        int m = word1.size(), n = word2.size();
        vector<int> dp(n + 1);
        for (int i = 0; i <= m; i++) 
        {
            vector<int> cur(n + 1);
            for (int j = 0; j <= n; j++) 
            {
                if (!i or !j) cur[j] = i + j;
                else if (word1[i - 1] == word2[j - 1]) cur[j] = dp[j - 1];
                else cur[j] = 1 + min(dp[j], cur[j - 1]);
            }
            dp = cur;
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length(), dp[][] = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) dp[i + 1][j + 1] = word1.charAt(i) == word2.charAt(j) ? dp[i][j] + 1 : Math.max(dp[i][j + 1], dp[i + 1][j]);
        return m + n - (dp[m][n] << 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(m):
            for j in range(n):
                dp[i + 1][j + 1] = dp[i][j] + 1 if word1[i] == word2[j] else max(dp[i][j + 1], dp[i + 1][j])
        return m + n - (dp[m][n] << 1)
```
