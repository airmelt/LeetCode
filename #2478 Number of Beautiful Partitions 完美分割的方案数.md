# 2478 Number of Beautiful Partitions 完美分割的方案数

__Description:__

You are given a string `s` that consists of the digits `'1'` to `'9'` and two integers `k` and `minLength`.

A partition of `s` is called __beautiful__ if:

- `s` is partitioned into `k` non-intersecting substrings.
- Each substring has a length of __at least__ `minLength`.
- Each substring starts with a __prime__ digit and ends with a __non-prime__ digit. Prime digits are `'2'`, `'3'`, `'5'`, and `'7'`, and the rest of the digits are non-prime.

Return _the number of __beautiful__ partitions of_ `s`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "23542185131", k = 3, minLength = 2
Output: 3
Explanation: There exists three ways to create a beautiful partition:
"2354 | 218 | 5131"
"2354 | 21851 | 31"
"2354218 | 51 | 31"
```

Example 2:

```text
Input: s = "23542185131", k = 3, minLength = 3
Output: 1
Explanation: There exists one way to create a beautiful partition: "2354 | 218 | 5131".
```

Example 3:

```text
Input: s = "3312958", k = 3, minLength = 1
Output: 1
Explanation: There exists one way to create a beautiful partition: "331 | 29 | 58".
```

__Constraints:__

- `1 <= k, minLength <= s.length <= 1000`
- `s` consists of the digits `'1'` to `'9'`.

__题目描述:__

给你一个字符串 `s` ，每个字符是数字 `'1'` 到 `'9'` ，再给你两个整数 `k` 和 `minLength` 。

如果对 `s` 的分割满足以下条件，那么我们认为它是一个 __完美__ 分割:

- `s` 被分成 `k` 段互不相交的子字符串。
- 每个子字符串长度都 __至少__ 为 `minLength` 。
- 每个子字符串的第一个字符都是一个 _质数_ 数字，最后一个字符都是一个 __非质数__ 数字。质数数字为 `'2'` ， `'3'` ， `'5'` 和 `'7'` ，剩下的都是非质数数字。

请你返回 `s` 的 __完美__ 分割数目。由于答案可能很大，请返回答案对 `10 ^ 9 + 7` __取余__ 后的结果。

一个 子字符串 是字符串中一段连续字符串序列。

__示例:__

示例 1：

```text
输入：s = "23542185131", k = 3, minLength = 2
输出：3
解释：存在 3 种完美分割方案：
"2354 | 218 | 5131"
"2354 | 21851 | 31"
"2354218 | 51 | 31"
```

示例 2：

```text
输入：s = "23542185131", k = 3, minLength = 3
输出：1
解释：存在一种完美分割方案："2354 | 218 | 5131" 。
```

示例 3：

```text
输入：s = "3312958", k = 3, minLength = 1
输出：1
解释：存在一种完美分割方案："331 | 29 | 58" 。
```

__提示：__

- `1 <= k, minLength <= s.length <= 1000`
- `s` 每个字符都为数字 `'1'` 到 `'9'` 之一。

__思路:__

```text
动态规划
dp[i][j] 表示使用 s[:j] 分割成 i 段的方案数
空串的方案数为 1, 即 dp[0][0] = 1
如果 k * minLength > n, 或者 s 的第一个字符不是质数, 或者最后一个字符是质数, 那么返回 0
枚举 i 从 1 到 k
累计所有 dp[i - 1][j - minLength] 的值
如果 j == minLength 或者 j == n, 或者 s[j - minLength - 1] 不是质数, s[j - minLength] 是质数, 那么 cur += dp[i - 1][j - minLength]
如果 j 处可以分割, 那么 dp[i][j] = cur, 这里 s[j - 1] 是非质数, s[j] 是质数
最后返回 dp[k][n] % (10 ^ 9 + 7)
可以用滚动数组优化空间
时间复杂度为 O(KN + KNM), 空间复杂度为 O(N), 其中 M 为 minLength
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int beautifulPartitions(string s, int k, int minLength) 
    {
        int n = s.size(), MOD = 1e9 + 7;
        vector<vector<int>> dp(k + 1, vector<int>(n + 1));
        auto is_prime = [](char c) -> bool { return c == '2' or c == '3' or c == '5' or c == '7'; };
        if (k * minLength > n or !is_prime(s.front()) or is_prime(s.back())) return dp.back().back();
        dp.front().front() = 1;
        for (int i = 1; i <= k; i++) 
        {
            int cur = 0;
            for (int j = i * minLength; j <= n - (k - i) * minLength; j++) 
            {
                if (j == minLength or j == n or !is_prime(s[j - minLength - 1]) and is_prime(s[j - minLength])) cur = (cur + dp[i - 1][j - minLength]) % MOD;
                if (!j or j == n or !is_prime(s[j - 1]) and is_prime(s[j])) dp[i][j] = cur;
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int beautifulPartitions(String s, int k, int minLength) {
        int n = s.length(), dp[][] = new int[k + 1][n + 1], MOD = 1_000_000_007;
        if (k * minLength > n || !isPrime(s.charAt(0)) || isPrime(s.charAt(n - 1))) return dp[k][n];
        dp[0][0] = 1;
        for (int i = 1; i <= k; i++) {
            int cur = 0;
            for (int j = i * minLength; j <= n - (k - i) * minLength; j++) {
                if (j == minLength || j == n || !isPrime(s.charAt(j - minLength - 1)) && isPrime(s.charAt(j - minLength))) cur = (cur + dp[i - 1][j - minLength]) % MOD;
                if (j == 0 || j == n || !isPrime(s.charAt(j - 1)) && isPrime(s.charAt(j))) dp[i][j] = cur;
            }
        }
        return dp[k][n];
    }

    private boolean isPrime(char c) {
        return c == '2' || c == '3' || c == '5' || c == '7';
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulPartitions(self, s: str, k: int, minLength: int) -> int:
        if k * minLength > (n := len(s)) or not s[0] in '2357' or s[-1] in '2357':
            return 0
        dp = [1] + [0] * n
        for i in range(1, k + 1):
            total, cur = 0, dp[:]
            for j in range(i * minLength, n - (k - i) * minLength + 1):
                if j == minLength or j == n or not s[j - minLength - 1] in '2357' and s[j - minLength] in '2357':
                    total += cur[j - minLength]
                if j == n or not s[j - 1] in '2357' and s[j] in '2357':
                    dp[j] = total
        return dp[-1] % (10 ** 9 + 7)
```
