# 940 Distinct Subsequences II 不同的子序列 II

__Description__:
Given a string s, return the number of distinct non-empty subsequences of s. Since the answer may be very large, return it modulo 10^9 + 7.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not.

__Example:__

Example 1:

Input: s = "abc"
Output: 7
Explanation: The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".

Example 2:

Input: s = "aba"
Output: 6
Explanation: The 6 distinct subsequences are "a", "b", "ab", "aa", "ba", and "aba".

Example 3:

Input: s = "aaa"
Output: 3
Explanation: The 3 distinct subsequences are "a", "aa" and "aaa".

__Constraints:__

1 <= s.length <= 2000
s consists of lowercase English letters.

__题目描述__:
给定一个字符串 s，计算 s 的 不同非空子序列 的个数。因为结果可能很大，所以返回答案需要对 10^9 + 7 取余 。

字符串的 子序列 是经由原字符串删除一些（也可能不删除）字符但不改变剩余字符相对位置的一个新字符串。

例如，"ace" 是 "abcde" 的一个子序列，但 "aec" 不是。

__示例 :__

示例 1：

输入：s = "abc"
输出：7
解释：7 个不同的子序列分别是 "a", "b", "c", "ab", "ac", "bc", 以及 "abc"。

示例 2：

输入：s = "aba"
输出：6
解释：6 个不同的子序列分别是 "a", "b", "ab", "ba", "aa" 以及 "aba"。

示例 3：

输入：s = "aaa"
输出：3
解释：3 个不同的子序列分别是 "a", "aa" 以及 "aaa"。

__提示:__

1 <= s.length <= 2000
s 仅由小写英文字母组成

__思路__:

动态规划
设 dp[i] 表示 s[:i] 中非空子序列的个数
如果不考虑重复 dp[i] = dp[i - 1] + (dp[i - 1] + 1), 对每个字符都可以取或者不取, 取的时候会多出一个本身字符
考虑重复 dp[i] = dp[i - 1] + (dp[i - 1] - dp[pre[s[i]]), 前一部分表示不取当前字符, 那么还是之前的值 dp[i - 1], 如果要取当前字符, 那么前面的重复值需要被减去, 可以用一个 26 位的 count 数组记录当前字符对应的 dp 的个数
因为 dp 只与前一个状态有关, 可以将空间复杂度优化为 O(1)
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int distinctSubseqII(string s) 
    {
        vector<int> count(26);
        int result = 0, mod = 1e9 + 7, n = s.size(), pre = 0;
        for (int i = 0; i < n; i++) 
        {
            pre = result;
            result = ((result << 1) + 1L - count[s[i] - 'a'] + mod) % mod;
            count[s[i] - 'a'] = (pre + 1) % mod;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int distinctSubseqII(String s) {
        long count[] = new long[26], result = 0, mod = 1_000_000_007, n = s.length(), pre = 0;
        for (int i = 0; i < n; i++) {
            pre = result;
            result = ((result << 1) + 1L - count[s.charAt(i) - 'a'] + mod) % mod;
            count[s.charAt(i) - 'a'] = (pre + 1) % mod;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def distinctSubseqII(self, s: str) -> int:
        count, mod, result, n = [0] * 26, 10 ** 9 + 7, 0, len(s)
        for i in range(n):
            result, count[ord(s[i]) - ord('a')] = (result << 1) + 1 - count[ord(s[i]) - ord('a')], result + 1
        return result % mod
```
