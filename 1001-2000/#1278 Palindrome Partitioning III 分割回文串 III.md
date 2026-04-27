# 1278 Palindrome Partitioning III 分割回文串 III

__Description:__

You are given a string s containing lowercase letters and an integer k. You need to :

First, change some characters of s to other lowercase English letters.
Then divide s into k non-empty disjoint substrings such that each substring is a palindrome.
Return the minimal number of characters that you need to change to divide the string.

__Example:__

Example 1:

Input: s = "abc", k = 2
Output: 1
Explanation: You can split the string into "ab" and "c", and change 1 character in "ab" to make it palindrome.

Example 2:

Input: s = "aabbc", k = 3
Output: 0
Explanation: You can split the string into "aa", "bb" and "c", all of them are palindrome.

Example 3:

Input: s = "leetcode", k = 8
Output: 0

__Constraints:__

1 <= k <= s.length <= 100.
s only contains lowercase English letters.

__题目描述：__

给你一个由小写字母组成的字符串 s，和一个整数 k。

请你按下面的要求分割字符串：

首先，你可以将 s 中的部分字符修改为其他的小写英文字母。
接着，你需要把 s 分割成 k 个非空且不相交的子串，并且每个子串都是回文串。
请返回以这种方式分割字符串所需修改的最少字符数。

__示例：__

示例 1：

输入：s = "abc", k = 2
输出：1
解释：你可以把字符串分割成 "ab" 和 "c"，并修改 "ab" 中的 1 个字符，将它变成回文串。

示例 2：

输入：s = "aabbc", k = 3
输出：0
解释：你可以把字符串分割成 "aa"、"bb" 和 "c"，它们都是回文串。

示例 3：

输入：s = "leetcode", k = 8
输出：0

__提示：__

1 <= k <= s.length <= 100
s 中只含有小写英文字母。

__思路：__

动态规划
令 dp[i][j] 表示将 s[:i] 分割成 j 部分需要修改的次数
dp[i][j] = min(dp[k][j - 1] + cost(s, k + 1, i)), 枚举第 j 个字符串的起始位置
cost(s, k + 1, i) 表示将 s[k + 1, i] 变成回文串需要的次数
预处理 cost[i][j] 可以压缩时间复杂度
时间复杂度为 O(kn ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int palindromePartition(string s, int k) 
    {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(k)), cost(n, vector<int>(n));
        for (int i = n - 1; i > 0; i--) for (int j = i - 1; j < n - 1; j++) cost[i - 1][j + 1] = cost[i][j] + (s[i - 1] != s[j + 1]);
        for (int i = 1; i < n; i++) dp[i].front() = cost.front()[i];
        for (int i = 1; i < n; i++)
        {
            for (int j = 1; j < k; j++) 
            {
                dp[i][j] = INT_MAX;
                for (int l = i; l >= j; l--) dp[i][j] = min(dp[i][j], dp[l - 1][j - 1] + cost[l][i]);
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int palindromePartition(String s, int k) {
        int n = s.length(), dp[][] = new int[n][k], cost[][] = new int[n][n];
        for (int i = n - 1; i > 0; i--) for (int j = i - 1; j < n - 1; j++) cost[i - 1][j + 1] = cost[i][j] + (s.charAt(i - 1) == s.charAt(j + 1) ? 0 : 1);
        for (int i = 1; i < n; i++) dp[i][0] = cost[0][i];
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < k; j++) {
                dp[i][j] = Integer.MAX_VALUE;
                for (int l = i; l >= j; l--) dp[i][j] = Math.min(dp[i][j], dp[l - 1][j - 1] + cost[l][i]);
            }
        }
        return dp[n - 1][k - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def palindromePartition(self, s: str, k: int) -> int:
        @lru_cache(None)
        def count(start: int, end: int) -> int:
            return 0 if start >= end else (s[start] != s[end]) + count(start + 1, end - 1)
        @lru_cache(None)
        def partition(i: int, k: int) -> int:
            return count(0, i) if k == 1 else min(partition(j, k - 1) + count(j + 1, i) for j in range(k - 2, i))
        return partition(len(s) - 1, k)
```
