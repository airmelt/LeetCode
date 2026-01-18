# 2851 String Transformation 字符串转换

__Description:__

You are given two strings `s` and `t` of equal length `n`. You can perform the following operation on the string `s`:

- Remove a __suffix__ of `s` of length `l` where `0 < l < n` and append it at the start of `s`.

  - For example, let `s = 'abcd'` then in one operation you can remove the suffix `'cd'` and append it in front of `s` making `s = 'cdab'`.

You are also given an integer `k`. Return _the number of ways in which_ `s` _can be transformed into_ `t` _in __exactly___ `k` _operations._

Since the answer can be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "abcd", t = "cdab", k = 2
Output: 2
Explanation: 
First way:
In first operation, choose suffix from index = 3, so resulting s = "dabc".
In second operation, choose suffix from index = 3, so resulting s = "cdab".

Second way:
In first operation, choose suffix from index = 1, so resulting s = "bcda".
In second operation, choose suffix from index = 1, so resulting s = "cdab".
```

Example 2:

```text
Input: s = "ababab", t = "ababab", k = 1
Output: 2
Explanation: 
First way:
Choose suffix from index = 2, so resulting s = "ababab".

Second way:
Choose suffix from index = 4, so resulting s = "ababab".
```

__Constraints:__

- `2 <= s.length <= 5 * 10 ^ 5`
- `1 <= k <= 10 ^ 15`
- `s.length == t.length`
- `s` and `t` consist of only lowercase English alphabets.

__题目描述:__

给你两个长度都为 `n` 的字符串 `s` 和 `t` 。你可以对字符串 `s` 执行以下操作:

- 将 `s` 长度为 `l` （ `0 < l < n`）的 __后缀字符串__ 删除，并将它添加在 `s` 的开头。

  - 比方说， `s = 'abcd'` ，那么一次操作中，你可以删除后缀 `'cd'` ，并将它添加到 `s` 的开头，得到 `s = 'cdab'` 。

给你一个整数 `k` ，请你返回 __恰好__ `k` 次操作将 `s` 变为 `t` 的方案数。

由于答案可能很大，返回答案对 `10 ^ 9 + 7` __取余__ 后的结果。

__示例:__

示例 1：

```text
输入：s = "abcd", t = "cdab", k = 2
输出：2
解释：
第一种方案：
第一次操作，选择 index = 3 开始的后缀，得到 s = "dabc" 。
第二次操作，选择 index = 3 开始的后缀，得到 s = "cdab" 。

第二种方案：
第一次操作，选择 index = 1 开始的后缀，得到 s = "bcda" 。
第二次操作，选择 index = 1 开始的后缀，得到 s = "cdab" 。
```

示例 2：

```text
输入：s = "ababab", t = "ababab", k = 1
输出：2
解释：
第一种方案：
选择 index = 2 开始的后缀，得到 s = "ababab" 。

第二种方案：
选择 index = 4 开始的后缀，得到 s = "ababab" 。
```

__提示：__

- `2 <= s.length <= 5 * 10 ^ 5`
- `1 <= k <= 10 ^ 15`
- `s.length == t.length`
- `s` 和 `t` 都只包含小写英文字母。

__思路:__

```text
动态规划 ➕ KMP ➕ 矩阵快速幂
设 dp[i][0] 表示 i 次操作后等于 t 的方案数
dp[i][1] 表示 i 次操作不等于 t 的方案数
初始化 dp[0][0] = 1, dp[0][1] = 0, if s == t
dp[0][0] = 0, dp[0][1] = 1
先求出 t 在 s + s[:-1] 里出现的次数 c
这里可以使用 KMP 加快查找速度
如果操作后等于 t
如果从 t 转移过来, 有 c - 1 种方式
如果不是从 t 转移过来, 有 c 种方式
如果操作后不等于 t
如果从 t 转移过来, 有 n - c 种方式
如果不是从 t 转移过来, 有 n - 1 - c 种方式
所以
dp[i][0] = dp[i - 1][0] * (c - 1) + dp[i - 1][1] * c
dp[i][1] = dp[i - 1][0] * (n - c) + dp[i - 1][1] * (n - 1 - c)
可以用矩阵快速幂优化计算 dp
时间复杂度为 O(N + logK), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfWays(string s, string t, long long k) 
    {
        int n = s.length(), c = 0, MOD = 1e9 + 7;
        auto helper_next = [&](const auto& s) -> vector<int> 
        {
            vector<int> next(n);
            for (int i = 1, j = 0; i < n; i++) 
            {
                while (j and s[i] != s[j]) j = next[j - 1];
                j += s[i] == s[j];
                next[i] = j;
            }
            return next;
        };
        auto kmp = [&](const auto& text, const auto& pattern) -> int 
        {
            vector<int> next = helper_next(pattern);
            int m = text.size(), result = 0;
            for (int i = 0, j = 0; i < m; i++) 
            {
                while (j and pattern[j] != text[i]) j = next[j - 1];
                if (pattern[j] == text[i]) ++j;
                if (j == n) 
                {
                    ++result;
                    j = next[j - 1];
                }
            }
            return result;
        };
        auto multiply = [&](const auto& a, const auto& b) -> vector<vector<long long>>
        {
            return vector<vector<long long>>{ { (a.front().front() * b.front().front() + a.front().back() * b.back().front()) % MOD, (a.front().front() * b.front().back() + a.front().back() * b.back().back()) % MOD }, { (a.back().front() * b.front().front() + a.back().back() * b.back().front()) % MOD, (a.back().front() * b.front().back() + a.back().back() * b.back().back()) % MOD } };
        };
        auto quick_pow = [&](auto& a, auto n) -> vector<vector<long long>> 
        {
            vector<vector<long long>> result = { { 1, 0 }, { 0, 1 } };
            for (; n; n >>= 1) 
            {
                if (n & 1) result = multiply(result, a);
                a = multiply(a, a);
            }
            return result;
        };
        vector<vector<long long>> dp{ { (c = kmp(s + s.substr(0, n - 1), t)) - 1, c }, { n - c, n - 1 - c } };
        return quick_pow(dp, k).front()[s != t];
    }
};
```

__Java__:

```Java
class Solution {
    private static final long MOD = 1_000_000_007L;

    public int numberOfWays(String s, String t, long k) {
        int n = s.length(), c = kmp(s + s.substring(0, n - 1), t);
        return (int)quickPow(new long[][]{ { c - 1, c }, { n - c, n - 1 - c} }, k)[0][s.equals(t) ? 0 : 1];
    }

    private int[] helperNext(String s) {
        int n = s.length(), next[] = new int[n];
        for (int i = 1, j = 0; i < n; i++) {
            while (j > 0 && s.charAt(j) != s.charAt(i)) j = next[j - 1];
            if (s.charAt(j) == s.charAt(i)) ++j;
            next[i] = j;
        }
        return next;
    }

    private int kmp(String text, String pattern) {
        int m = text.length(), n = pattern.length(), next[] = helperNext(pattern), result = 0;
        for (int i = 0, j = 0; i < m; i++) {
            while (j > 0 && pattern.charAt(j) != text.charAt(i)) j = next[j - 1];
            if (pattern.charAt(j) == text.charAt(i)) ++j;
            if (j == n) {
                ++result;
                j = next[j - 1];
            }
        }
        return result;
    }

    private long[][] multiply(long[][] a, long[][] b) {
        return new long[][]{ { (a[0][0] * b[0][0] + a[0][1] * b[1][0]) % MOD, (a[0][0] * b[0][1] + a[0][1] * b[1][1]) % MOD }, { (a[1][0] * b[0][0] + a[1][1] * b[1][0]) % MOD, (a[1][0] * b[0][1] + a[1][1] * b[1][1]) % MOD } };
    }

    private long[][] quickPow(long[][] a, long n) {
        long[][] result = { { 1, 0 }, { 0, 1 } };
        for (; n > 0; n >>= 1) {
            if ((n & 1) > 0) result = multiply(result, a);
            a = multiply(a, a);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWays(self, s, t, k):
        n, MOD = len(s), 10 ** 9 + 7

        def multiply(a: List[List[int]], b: List[List[int]]) -> List[List[int]]:
            return [[(a[0][0] * b[0][0] + a[0][1] * b[1][0]) % MOD, (a[0][0] * b[0][1] + a[0][1] * b[1][1]) % MOD], [(a[1][0] * b[0][0] + a[1][1] * b[1][0]) % MOD, (a[1][0] * b[0][1] + a[1][1] * b[1][1]) % MOD]]

        def quick_pow(a: List[List[int]], n: int) -> List[List[int]]:
            base = [[1, 0], [0, 1]]
            while n:
                if n & 1:
                    base = multiply(base, a)
                a = multiply(a, a)
                n >>= 1
            return base

        def helper_next(s: str) -> List[int]:
            next, j = [0] * n, 0
            for i, c in enumerate(s[1:], 1):
                while j and s[j] != c:
                    j = next[j - 1]
                if s[j] == c:
                    j += 1
                next[i] = j
            return next

        def kmp(text: str, pattern: str) -> int:
            next, result, j = helper_next(pattern), 0, 0
            for i, c in enumerate(text):
                while j and pattern[j] != c:
                    j = next[j - 1]
                if pattern[j] == c:
                    j += 1
                if j == n:
                    result += 1
                    j = next[j - 1]
            return result
        return quick_pow([[(c := kmp(s + s[:-1], t)) - 1, c], [n - c, n - 1 - c]], k)[0][s != t]
```
