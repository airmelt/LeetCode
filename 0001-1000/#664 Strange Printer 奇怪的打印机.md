# 664 Strange Printer 奇怪的打印机

__Description__:
There is a strange printer with the following two special properties:

The printer can only print a sequence of the same character each time.
At each turn, the printer can print new characters starting from and ending at any place and will cover the original existing characters.
Given a string s, return the minimum number of turns the printer needed to print it.

__Example:__

Example 1:

Input: s = "aaabbb"
Output: 2
Explanation: Print "aaa" first and then print "bbb".

Example 2:

Input: s = "aba"
Output: 2
Explanation: Print "aaa" first and then print "b" from the second place of the string, which will cover the existing character 'a'.

__Constraints:__
1 <= s.length <= 100
s consists of lowercase English letters.

__题目描述__:
有台奇怪的打印机有以下两个特殊要求：

打印机每次只能打印由 同一个字符 组成的序列。
每次可以在任意起始和结束位置打印新字符，并且会覆盖掉原来已有的字符。
给你一个字符串 s ，你的任务是计算这个打印机打印它需要的最少打印次数。

__示例 :__

示例 1：

输入：s = "aaabbb"
输出：2
解释：首先打印 "aaa" 然后打印 "bbb"。

示例 2：

输入：s = "aba"
输出：2
解释：首先打印 "aaa" 然后在第二个位置打印 "b" 覆盖掉原来的字符 'a'。

__提示:__

1 <= s.length <= 100
s 由小写英文字母组成

__思路__:

动态规划
打印一个字符需要 1 次操作, 打印 n 个字符最多需要 n 次操作
设 dp[i][j] 表示打印 s[i:j] 需要 dp[i][j] 次操作
如果 s[i] == s[j] 说明 s[i:j - 1] 打印的同时可以直接打印出 s[i:j], 此时有 dp[i][j] = dp[i][j - 1]
否则在 [i:j] 区间中查找最小的打印次数 dp[i][j] = min(dp[i][k] + dp[k + 1][j]), 其中 i <= k < j
边界条件为 dp[i][i] = 1
由于计算需要用到 dp[i][k] 和 dp[k + 1][j], 计算时需要 i 逆序遍历, j 正序遍历
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int strangePrinter(string s) 
    {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = n - 1; i > -1; i--) 
        {
            dp[i][i] = 1;
            for (int j = i + 1; j < n; j++) 
            {
                if (s[i] == s[j]) dp[i][j] = dp[i][j - 1];
                else 
                {
                    int cur = INT_MAX;
                    for (int k = i; k < j; k++) cur = min(dp[i][k] + dp[k + 1][j], cur);
                    dp[i][j] = cur;
                }
            }
        }
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int strangePrinter(String s) {
        int n = s.length();
        int dp[][] = new int[n][n];
        for (int i = n - 1; i > -1; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) dp[i][j] = dp[i][j - 1];
                else {
                    int cur = Integer.MAX_VALUE;
                    for (int k = i; k < j; k++) cur = Math.min(dp[i][k] + dp[k + 1][j], cur);
                    dp[i][j] = cur;
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def strangePrinter(self, s: str) -> int:
        n = len(s)
        dp = [[0] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = 1
        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                dp[i][j] = dp[i][j - 1] if s[i] == s[j] else min(dp[i][k] + dp[k + 1][j] for k in range(i, j))
        return dp[0][-1]
```
