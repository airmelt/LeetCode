# 97 Interleaving String 交错字符串

__Description__:
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

__Example:__

Example 1:

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true

Example 2:

Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false

__题目描述__:
给定三个字符串 s1, s2, s3, 验证 s3 是否是由 s1 和 s2 交错组成的。

__示例 :__

示例 1:

输入: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
输出: true

示例 2:

输入: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
输出: false

__思路__:

交错字符串应该是指的, s1和 s2中的字符按原字符串顺序交错排列是否可以组成 s3
参考[LeetCode #10 Regular Expression Matching 正则表达式匹配](https://www.jianshu.com/p/43a16636dc1a)
动态规划
dp[i][j]表示 s1[:i]和 s2[:j]能交错组成 s3[:i + j - 1]
dp[0][0] = true
dp[i][0] = dp[i - 1][0] && s1[i - 1] == s3[i - 1]
dp[0][j] = dp[0][j - 1] && s2[j - 1] == s3[j - 1]
dp[i][j] = (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1)
时间复杂度O(mn), 空间复杂度O(mn), 其中 m, n分别为字符串 s1, s2的长度
这里可以看到, 每一行遍历都只用到了一行 dp数组, 可以将空间复杂度优化到 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isInterleave(string s1, string s2, string s3) 
    {
        if (s1.size() + s2.size() != s3.size()) return false;
        vector<bool> dp(s2.size() + 1, false);
        for (int i = 0; i <= s1.size(); i++) for (int j = 0; j <= s2.size(); j++) 
        {
               if (i == 0 and j == 0) dp[j] = true;
               else if (i == 0) dp[j] = dp[j - 1] and s2[j - 1] == s3[i + j - 1];
               else if (j == 0) dp[j] = dp[j] and s1[i - 1] == s3[i + j - 1];
               else dp[j] = (dp[j] and s1[i - 1] == s3[i + j - 1]) or (dp[j - 1] and s2[j - 1] == s3[i + j - 1]);
        }
       return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int m = s1.length(), n = s2.length();
        if ((m + n) != s3.length()) return false;
        boolean dp[][] = new boolean[m + 1][n + 1];
        dp[0][0] = true;
        for (int i = 1; i <= m; i++) dp[i][0] = dp[i - 1][0] && s1.charAt(i - 1) == s3.charAt(i - 1);
        for (int j = 1; j <= n; j++) dp[0][j] = dp[0][j - 1] && s2.charAt(j - 1) == s3.charAt(j - 1);
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = (dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1)) || (dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1));
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if len(s1) + len(s2) != len(s3):
            return False
        dp = [[True] * (len(s2) + 1) for _ in range(len(s1) + 1)]
        dp[0][0] = True
        for i in range(1, len(s1) + 1):
            dp[i][0] = dp[i - 1][0] and s1[i - 1] == s3[i - 1]
        for i in range(1, len(s2) + 1):
            dp[0][i] = dp[0][i - 1] and s2[i - 1] == s3[i - 1]
        for i in range(1, len(s1) + 1):
            for j in range(1, len(s2) + 1):
                dp[i][j] = (s1[i - 1] == s3[i + j - 1] and dp[i - 1][j]) or (s2[j - 1] == s3[i + j - 1] and dp[i][j - 1])
        return dp[-1][-1]
```
