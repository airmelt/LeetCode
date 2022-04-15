# 1092 Shortest Common Supersequence 最短公共超序列

__Description__:
Given two strings str1 and str2, return the shortest string that has both str1 and str2 as subsequences. If there are multiple valid strings, return any of them.

A string s is a subsequence of string t if deleting some number of characters from t (possibly 0) results in the string s.

__Example:__

Example 1:

Input: str1 = "abac", str2 = "cab"
Output: "cabac"
Explanation:
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.

Example 2:

Input: str1 = "aaaaaaaa", str2 = "aaaaaaaa"
Output: "aaaaaaaa"

__Constraints:__

1 <= str1.length, str2.length <= 1000
str1 and str2 consist of lowercase English letters.

__题目描述__:
给出两个字符串 str1 和 str2，返回同时以 str1 和 str2 作为子序列的最短字符串。如果答案不止一个，则可以返回满足条件的任意一个答案。

（如果从字符串 T 中删除一些字符（也可能不删除，并且选出的这些字符可以位于 T 中的 任意位置），可以得到字符串 S，那么 S 就是 T 的子序列）

__示例 :__

输入：str1 = "abac", str2 = "cab"
输出："cabac"
解释：
str1 = "abac" 是 "cabac" 的一个子串，因为我们可以删去 "cabac" 的第一个 "c"得到 "abac"。
str2 = "cab" 是 "cabac" 的一个子串，因为我们可以删去 "cabac" 末尾的 "ac" 得到 "cab"。
最终我们给出的答案是满足上述属性的最短字符串。

__提示:__

1 <= str1.length, str2.length <= 1000
str1 和 str2 都由小写英文字母组成。

__思路__:

动态规划
令 dp[i][j] 表示 str1[:i] 和 str2[:j] 的最长相同子序列的长度
dp[i][j] = dp[i - 1][j - 1] + 1, if str1[i - 1] == str2[j - 1]
dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
然后按照 dp 中的路径将最长相同子序列还原
最后将不属于最长相同子序列中的字符拼接即可
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution
{
public:
    string shortestCommonSupersequence(string str1, string str2) 
    {
        int m = str1.size(), n = str2.size(), a = m, b = n, p = 0, q = 0;
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = str1[i - 1] == str2[j - 1] ? dp[i - 1][j - 1] + 1 : max(dp[i][j - 1], dp[i - 1][j]);
        string lcs = "", result = "";
        while (a and b) 
        {
            if (str1[a - 1] == str2[b - 1]) 
            {
                lcs += str1[a-- - 1];
                --b;
            } 
            else if (dp[a - 1][b] > dp[a][b - 1]) --a;
            else --b;
        }
        for (auto c = lcs.rbegin(); c != lcs.rend(); c++) 
        {
            while (p < m and str1[p] != *c) result += str1[p++];
            while (q < n and str2[q] != *c) result += str2[q++];
            result += *c;
            ++p;
            ++q;
        }
        while (p < m) result += str1[p++];
        while (q < n) result += str2[q++];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length(), n = str2.length(), a = m, b = n, p = 0, q = 0, dp[][] = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) dp[i][j] = str1.charAt(i - 1) == str2.charAt(j - 1) ? dp[i - 1][j - 1] + 1 : Math.max(dp[i][j - 1], dp[i - 1][j]);
        StringBuilder lcs = new StringBuilder(), result = new StringBuilder();
        while (a > 0 && b > 0) {
            if (str1.charAt(a - 1) == str2.charAt(b - 1)) {
                lcs.append(str1.charAt(a-- - 1));
                --b;
            } else if (dp[a - 1][b] > dp[a][b - 1]) --a;
            else --b;
        }
        for (char c : lcs.reverse().toString().toCharArray()) {
            while (p < m && str1.charAt(p) != c) result.append(str1.charAt(p++));
            while (q < n && str2.charAt(q) != c) result.append(str2.charAt(q++));
            result.append(c);
            ++p;
            ++q;
        }
        while (p < m) result.append(str1.charAt(p++));
        while (q < n) result.append(str2.charAt(q++));
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def shortestCommonSupersequence(self, str1: str, str2: str) -> str:
        m, n, lcs, result = len(str1), len(str2), '', ''
        dp, a, b, p, q = [[0] * (n + 1) for _ in range(m + 1)], m, n, 0, 0
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                    dp[i][j] = dp[i - 1][j - 1] + 1 if str1[i - 1] == str2[j - 1] else max(dp[i][j - 1], dp[i - 1][j])
        while a and b:
            if str1[a - 1] == str2[b - 1]:
                lcs += str1[a - 1]
                a -= 1
                b -= 1
            elif dp[a - 1][b] > dp[a][b - 1]:
                a -= 1
            else:
                b -= 1
        for c in lcs[::-1]:
            while p < m and str1[p] != c:
                result += str1[p]
                p += 1
            while q < n and str2[q] != c:
                result += str2[q]
                q += 1
            result += c
            p += 1
            q += 1
        return result + str1[p:] + str2[q:]
```
