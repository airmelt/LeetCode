# 730 Count Different Palindromic Subsequences 统计不同回文子序列

__Description__:
Given a string s, return the number of different non-empty palindromic subsequences in s. Since the answer may be very large, return it modulo 10^9 + 7.

A subsequence of a string is obtained by deleting zero or more characters from the string.

A sequence is palindromic if it is equal to the sequence reversed.

Two sequences a1, a2, ... and b1, b2, ... are different if there is some i for which ai != bi.

__Example:__

Example 1:

Input: s = "bccb"
Output: 6
Explanation: The 6 different non-empty palindromic subsequences are 'b', 'c', 'bb', 'cc', 'bcb', 'bccb'.
Note that 'bcb' is counted only once, even though it occurs twice.

Example 2:

Input: s = "abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba"
Output: 104860361
Explanation: There are 3104860382 different non-empty palindromic subsequences, which is 104860361 modulo 10^9 + 7.

__Constraints:__

1 <= s.length <= 1000
s[i] is either 'a', 'b', 'c', or 'd'.

__题目描述__:
给定一个字符串 S，找出 S 中不同的非空回文子序列个数，并返回该数字与 10^9 + 7 的模。

通过从 S 中删除 0 个或多个字符来获得子序列。

如果一个字符序列与它反转后的字符序列一致，那么它是回文字符序列。

如果对于某个  i，A_i != B_i，那么 A_1, A_2, ... 和 B_1, B_2, ... 这两个字符序列是不同的。

__示例 :__

示例 1：

输入：
S = 'bccb'
输出：6
解释：
6 个不同的非空回文子字符序列分别为：'b', 'c', 'bb', 'cc', 'bcb', 'bccb'。
注意：'bcb' 虽然出现两次但仅计数一次。

示例 2：

输入：
S = 'abcdabcdabcdabcdabcdabcdabcdabcddcbadcbadcbadcbadcbadcbadcbadcba'
输出：104860361
解释：
共有 3104860382 个不同的非空回文子序列，对 10^9 + 7 取模为 104860361。

__提示:__

字符串 S 的长度将在[1, 1000]范围内。
每个字符 S[i] 将会是集合 {'a', 'b', 'c', 'd'} 中的某一个。

__思路__:

1. 二维数组 DP
dp[i][j] 表示 s[i:j + 1] 中的回文子序列个数
初始化 dp[i][i] = 1, 每一个单独的字符本身就是回文串
如果 s[i] != s[j], dp[i][j] = dp[i + 1][j] + dp[i][j - 1] - dp[i + 1][j - 1], 中间重叠的部分算了两次需要减掉
如果 s[i] == s[j], 记录 left 为 i + 1 开始的第一个与 s[i] 相同的下标, right 为 j - 1 开始的第一个与 s[j] 相同的下标
如果 left > right, 说明 s[i:j + 1] 中间没有一个字符与两端的字母相同, 比如 'aba', 这时 dp[i][j] = (dp[i + 1][j - 1]  + 1) \* 2, 要么是内部的子回文串, 要么是内部的子回文串再加上两头的字符组成的新的回文串, 这里 + 1 是因为可以是空串
如果 left == right, 说明 s[i:j + 1] 中间有且仅有一个字符与两端的字母相同, 比如 'aaa', 这时 dp[i][j] = dp[i + 1][j - 1] \* 2 + 1, 因为单个的字符 'a' 已经统计过了比上一种情况少 1
其他情况就是 left != right, 说明 s[i:j + 1] 中间至少有两个字符与两端的字母相同, 比如 'aabaa', 这时 dp[i][j] = dp[i + 1][j - 1] \* 2 - dp[left + 1][right - 1], 需要把中间重复计算的去掉
最后返回 dp[0][n - 1]
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)
2. 一维数组 DP
类似扑克牌开火车的玩法, 出现两个相同的就回收
用一个数组记录下标 i 之后出现的回文串
用另一个数组记录已经使用过的回文串
对每一个遍历到的字符, 初始值为 1, 表示长度为 1 的自身一定是一个回文串
每当出现一次 s[i] == s[j], 则 i ~ j 中间的字符串可以和两头组成新的回文串, 更新 count[i] 和 used[i], 再更新 cur 将空串也要算进去, 即 cur = cur_count - cur_used + 1
否则更新 count[i] += cur
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countPalindromicSubsequences(string s) 
    {
        int n = s.size(), m = 1e9 + 7;
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int i = n - 2; i > -1; i--)
        {
            for (int j = i + 1; j < n; j++)
            {
                if (s[i] == s[j])
                {
                    int left = i + 1, right = j - 1;
                    while (left <= right and s[left] != s[i]) ++left;
                    while (left <= right and s[right] != s[j]) --right;
                    if (left > right) dp[i][j] = (dp[i + 1][j - 1] + 1) << 1;
                    else if (left == right) dp[i][j] = (dp[i + 1][j - 1] << 1) + 1;
                    else dp[i][j] = (dp[i + 1][j - 1] << 1) - dp[left + 1][right - 1];
                }
                else dp[i][j] = dp[i + 1][j] + dp[i][j - 1] - dp[i + 1][j - 1];
                dp[i][j] = (dp[i][j] < 0) ? dp[i][j] + m : dp[i][j] % m;
            }
        }
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int countPalindromicSubsequences(String s) {
        int n = s.length(), m = 1000000007;
        int dp[][] = new int[n][n];
        for (int i = 0; i < n; i++) dp[i][i] = 1;
        for (int i = n - 2; i > -1; --i) {
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    int left = i + 1, right = j - 1;
                    while (left <= right && s.charAt(left) != s.charAt(i)) ++left;
                    while (left <= right && s.charAt(right) != s.charAt(i)) --right;
                    if (left > right) dp[i][j] = dp[i + 1][j - 1] * 2 + 2;
                    else if (left == right) dp[i][j] = dp[i + 1][j - 1] * 2 + 1;
                    else dp[i][j] = dp[i + 1][j - 1] * 2 - dp[left + 1][right - 1];
                } 
                else dp[i][j] = dp[i][j - 1] + dp[i + 1][j] - dp[i + 1][j - 1];
                dp[i][j] = (dp[i][j] < 0) ? dp[i][j] + m : dp[i][j] % m;
            }
        }
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def countPalindromicSubsequences(self, s: str) -> int:
        # 以下均以 bccb 为例
        # count 数组记录 s[i:j + 1] 中间有多少个回文串, i = 0, j = 2, count[0] = 2, 'c' 和 'cc'
        # used 数组记录已经使用过的回文串, i = 0, j = 3, used[0] = 3, 'c', 'cc', '' 均已经被使用
        m, count, used, result = 10 ** 9 + 7, [0] * (n := len(s)), [0] * n, 0
        for j in range(n):
            # 初始状态下, 自身一定是一个回文串
            cur = 1
            for i in range(j - 1, -1, -1):
                if s[i] == s[j]:
                    cur, used[i], count[i] = count[i] - used[i] + 1, count[i] + 1, count[i] + cur
                else:
                    count[i] += cur
            result += cur
        return result % m
```
