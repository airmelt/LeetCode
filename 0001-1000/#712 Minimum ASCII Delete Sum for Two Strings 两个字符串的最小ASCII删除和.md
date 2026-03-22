# 712 Minimum ASCII Delete Sum for Two Strings 两个字符串的最小ASCII删除和

__Description__:
Given two strings s1 and s2, return the lowest ASCII sum of deleted characters to make two strings equal.

__Example:__

Example 1:

Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.

Example 2:

Input: s1 = "delete", s2 = "leet"
Output: 403
Explanation: Deleting "dee" from "delete" to turn the string into "let",
adds 100[d] + 101[e] + 101[e] to the sum.
Deleting "e" from "leet" adds 101[e] to the sum.
At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.

__Constraints:__

1 <= s1.length, s2.length <= 1000
s1 and s2 consist of lowercase English letters.

__题目描述__:
给定两个字符串s1, s2，找到使两个字符串相等所需删除字符的ASCII值的最小和。

__示例 :__

示例 1:

输入: s1 = "sea", s2 = "eat"
输出: 231
解释: 在 "sea" 中删除 "s" 并将 "s" 的值(115)加入总和。
在 "eat" 中删除 "t" 并将 116 加入总和。
结束时，两个字符串相等，115 + 116 = 231 就是符合条件的最小和。

示例 2:

输入: s1 = "delete", s2 = "leet"
输出: 403
解释: 在 "delete" 中删除 "dee" 字符串变成 "let"，
将 100[d]+101[e]+101[e] 加入总和。在 "leet" 中删除 "e" 将 101[e] 加入总和。
结束时，两个字符串都等于 "let"，结果即为 100+101+101+101 = 403 。
如果改为将两个字符串转换为 "lee" 或 "eet"，我们会得到 433 或 417 的结果，比答案更大。

__注意:__

0 < s1.length, s2.length <= 1000。
所有字符串中的字符ASCII值在[97, 122]之间。

__思路__:

动态规划
设 dp[i][j] 表示 s1[:i] 和 s2[:j] 中删除元素之后相等所累积的 ASCII 码的总和
初始化 dp[i][0] = sum(s1[:i]), 即 s1 的 ASCII 码前缀和, dp[0][j] = sum(s2[:j]), 即 s1 的 ASCII 码前缀和
s1[i] == s2[j], dp[i + 1][j + 1] = dp[i][j], 因为不需要删除元素
dp[i + 1][j + 1] = min(dp[i][j + 1] + s1[i + 1], dp[i + 1][j] + s2[j + 1]), 删除 s1[i] 和 s2[j] 中 ASCII 码较小的元素
可以观察到只需要前一行的值, 用滚动数组可以降低空间复杂度
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minimumDeleteSum(string s1, string s2) 
    {
        int m = s1.size(), n = s2.size(), s = 0;
        vector<int> dp(n + 1);
        for (int j = 1; j <= n; j++) dp[j] = dp[j - 1] + s2[j - 1];
        for (int i = 0; i < m; i++) 
        {
            s += s1[i];
            vector<int> cur(n + 1, s);
            for (int j = 0; j < n; j++) cur[j + 1] = s1[i] == s2[j] ? dp[j] : min(dp[j + 1] + s1[i], cur[j] + s2[j]);
            dp = cur;
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length(), n = s2.length();
        int dp[][] = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
        for (int j = 1; j <= n; j++) dp[0][j] = dp[0][j - 1] + s2.charAt(j - 1);
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) dp[i + 1][j + 1] = s1.charAt(i) == s2.charAt(j) ? dp[i][j] : Math.min(dp[i][j + 1] + s1.charAt(i), dp[i + 1][j] + s2.charAt(j));
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDeleteSum(self, s1: str, s2: str) -> int:
        m, n = len(s1), len(s2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            dp[i][0] = dp[i - 1][0] + ord(s1[i - 1])
        for i in range(1, n + 1):
            dp[0][i] = dp[0][i - 1] + ord(s2[i - 1])
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                dp[i][j] = dp[i - 1][j - 1] if s1[i - 1] == s2[j - 1] else min(dp[i - 1][j] + ord(s1[i - 1]), dp[i][j - 1] + ord(s2[j - 1]))
        return dp[-1][-1]
```
