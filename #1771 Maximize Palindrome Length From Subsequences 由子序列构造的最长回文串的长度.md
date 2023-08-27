# 1771 Maximize Palindrome Length From Subsequences 由子序列构造的最长回文串的长度

__Description:__

You are given two strings, `word1` and `word2`. You want to construct a string in the following manner:

- Choose some __non-empty__ subsequence `subsequence1` from `word1`.
- Choose some __non-empty__ subsequence `subsequence2` from `word2`.
- Concatenate the subsequences: `subsequence1 + subsequence2`, to make the string.

Return _the __length__ of the longest __palindrome__ that can be constructed in the described manner._ If no palindromes can be constructed, return `0`.

A __subsequence__ of a string `s` is a string that can be made by deleting some (possibly none) characters from `s` without changing the order of the remaining characters.

A palindrome is a string that reads the same forward as well as backward.

__Example:__

Example 1:

```text
Input: word1 = "cacb", word2 = "cbba"
Output: 5
Explanation: Choose "ab" from word1 and "cba" from word2 to make "abcba", which is a palindrome.
```

Example 2:

```text
Input: word1 = "ab", word2 = "ab"
Output: 3
Explanation: Choose "ab" from word1 and "a" from word2 to make "aba", which is a palindrome.
```

Example 3:

```text
Input: word1 = "aa", word2 = "bb"
Output: 0
Explanation: You cannot construct a palindrome from the described method, so return 0.
```

__Constraints:__

- `1 <= word1.length, word2.length <= 1000`
- `word1` and `word2` consist of lowercase English letters.

__题目描述:__

给你两个字符串 `word1` 和 `word2` ，请你按下述方法构造一个字符串:

- 从 `word1` 中选出某个 __非空__ 子序列 `subsequence1` 。
- 从 `word2` 中选出某个 __非空__ 子序列 `subsequence2` 。
- 连接两个子序列 `subsequence1 + subsequence2` ，得到字符串。

返回可按上述方法构造的最长 __回文串__ 的 __长度__ 。如果无法构造回文串，返回 `0` 。

字符串 `s` 的一个 __子序列__ 是通过从 `s` 中删除一些（也可能不删除）字符而不更改其余字符的顺序生成的字符串。

回文串 是正着读和反着读结果一致的字符串。

__示例:__

示例 1：

```text
输入：word1 = "cacb", word2 = "cbba"
输出：5
解释：从 word1 中选出 "ab" ，从 word2 中选出 "cba" ，得到回文串 "abcba" 。
```

示例 2：

```text
输入：word1 = "ab", word2 = "ab"
输出：3
解释：从 word1 中选出 "ab" ，从 word2 中选出 "a" ，得到回文串 "aba" 。
```

示例 3：

```text
输入：word1 = "aa", word2 = "bb"
输出：0
解释：无法按题面所述方法构造回文串，所以返回 0 。
```

__提示：__

- `1 <= word1.length, word2.length <= 1000`
- `word1` 和 `word2` 由小写英文字母组成

__思路:__

```text
动态规划
dp[i][j] 表示 word1[:i] 和 word2[j:] 的最长回文串的长度, i, j 均不为 0
不妨令 word1 += word2, 那么问题就转化为求 word1 的最长回文串的长度
初始化 dp[i][i] = 1, 表示单字符的最长回文串的长度为 1
if word1[i] == word2[j], dp[i][j] = dp[i + 1][j - 1] + 2
此时 if i < m && j >= m, result = max(result, dp[i][j]), 因为这里需要保证两个子序列非空
else dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
从后往前遍历, 因为需要用到 dp[i + 1]
注意到 dp[i] 只与 dp[i + 1] 有关, 因此可以使用滚动数组优化空间复杂度
时间复杂度为 O((M + N) ^ 2), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestPalindrome(string word1, string word2)
     {
        int result = 0, m = word1.length(), n = word2.length(), t = m + n;
        vector<int> dp(t);
        dp.back() = 1;
        word1 += word2;
        for (int i = t - 1; i > -1; i--) 
        {
            vector<int> cur(t);
            cur[i] = 1;
            for (int j = i + 1; j < t; j++) 
            {
                if (word1[i] == word1[j]) 
                {
                    cur[j] = dp[j - 1] + 2;
                    if (i < m && j >= m) result = max(result, cur[j]);
                } 
                else cur[j] = max(dp[j], cur[j - 1]);
            }
            dp = move(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestPalindrome(String word1, String word2) {
        int result = 0, m = word1.length(), n = word2.length(), t = m + n, dp[][] = new int[t][t];
        word1 += word2;
        for (int i = 0; i < t; i++) dp[i][i] = 1;
        for (int i = t - 1; i > -1; i--) {
            for (int j = i + 1; j < t; j++) {
                if (word1.charAt(i) == word1.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                    if (i < m && j >= m) result = Math.max(result, dp[i][j]);
                } else dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution {
    public int longestPalindrome(String word1, String word2) {
        int result = 0, m = word1.length(), n = word2.length(), t = m + n, dp[][] = new int[t][t];
        word1 += word2;
        for (int i = 0; i < t; i++) dp[i][i] = 1;
        for (int i = t - 1; i > -1; i--) {
            for (int j = i + 1; j < t; j++) {
                if (word1.charAt(i) == word1.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                    if (i < m && j >= m) result = Math.max(result, dp[i][j]);
                } else dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
        return result;
    }
}
```
