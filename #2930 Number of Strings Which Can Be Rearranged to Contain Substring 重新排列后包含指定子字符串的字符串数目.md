# 2930 Number of Strings Which Can Be Rearranged to Contain Substring 重新排列后包含指定子字符串的字符串数目

__Description:__

You are given an integer `n`.

A string `s` is called __good__ if it contains only lowercase English characters __and__ it is possible to rearrange the characters of `s` such that the new string contains `"leet"` as a __substring__.

For example:

- The string `"lteer"` is good because we can rearrange it to form `"leetr"` .
- `"letl"` is not good because we cannot rearrange it to contain `"leet"` as a substring.

Return _the __total__ number of good strings of length_ `n`.

Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: n = 4
Output: 12
Explanation: The 12 strings which can be rearranged to have "leet" as a substring are: "eelt", "eetl", "elet", "elte", "etel", "etle", "leet", "lete", "ltee", "teel", "tele", and "tlee".
```

Example 2:

```text
Input: n = 10
Output: 83943898
Explanation: The number of strings with length 10 which can be rearranged to have "leet" as a substring is 526083947580. Hence the answer is 526083947580 % (109 + 7) = 83943898.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`

__题目描述:__

给你一个整数 `n` 。

如果一个字符串 `s` 只包含小写英文字母，__且__ 将 `s` 的字符重新排列后，新字符串包含 __子字符串__ `"leet"` ，那么我们称字符串 `s` 是一个 __好__ 字符串。

比方说：

- 字符串 `"lteer"` 是好字符串，因为重新排列后可以得到 `"leetr"` 。
- `"letl"` 不是好字符串，因为无法重新排列并得到子字符串 `"leet"` 。

请你返回长度为 `n` 的好字符串 __总__ 数目。

由于答案可能很大，将答案对 `10 ^ 9 + 7` __取余__ 后返回。

子字符串 是一个字符串中一段连续的字符序列。

__示例:__

示例 1：

```text
输入：n = 4
输出：12
解释：总共有 12 个字符串重新排列后包含子字符串 "leet" ："eelt" ，"eetl" ，"elet" ，"elte" ，"etel" ，"etle" ，"leet" ，"lete" ，"ltee" ，"teel" ，"tele" 和 "tlee" 。
```

示例 2：

```text
输入：n = 10
输出：83943898
解释：长度为 10 的字符串重新排列后包含子字符串 "leet" 的方案数为 526083947580 。所以答案为 526083947580 % (109 + 7) = 83943898 。
```

__提示：__

- `1 <= n <= 10 ^ 5`

__思路:__

```text
数学
正难则反
考虑在全集中减去不符合要求的字符串
全集为 26 ^ n, 每个位置都可以选 26 个字母
这里面分成 4 种情况
1. 不包括 l
2. 不包括 t
3. 不包括 e
4. 只包括 1 个 e
不包括单字母即 25 ^ n, 每个位置只允许填 25 种字母
只包括 1 个 e 的情况下, e 可以选 n 个位置, 剩下位置只能选其他 25 个字母, 即 25 ^ (n - 1) * n
减去的情况为 3 * 25 ^ n + 25 ^ (n - 1) * n, 即 25 ^ (n - 1) * (n + 25 * 3) = 25 ^ (n - 1) * (75 + n)
减去的时候是包括 2 种同时成立或 3 种同时成立的
需要再加回 2 种同时成立的, 24 ^ (n - 1) * (72 + n * 2)
这里又多加了一次 3 种同时成立的, 23 ^ (n - 1) * (23 + n)
计算的时候可以用快速幂加快计算
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int stringCount(int n) 
    {
        return ((quick_pow(26LL, n) - quick_pow(25LL, n - 1) * (75 + n) + quick_pow(24LL, n - 1) * (72 + (n << 1)) - quick_pow(23LL, n - 1) * (23 + n)) % MOD + MOD) % MOD;
    }
private:
    static constexpr long long MOD = 1e9 + 7LL;

    long long quick_pow(long long a, int n) 
    {
        long long result = 1LL;
        for (; n; n >>= 1) 
        {
            if (n & 1) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private static final long MOD = 1_000_000_007L;
    
    public int stringCount(int n) {
        return (int)(((quickPow(26L, n) - quickPow(25L, n - 1) * (75 + n) + quickPow(24L, n - 1) * (72 + (n << 1)) - quickPow(23L, n - 1) * (23 + n)) % MOD + MOD) % MOD);
    }

    private long quickPow(long a, int n) {
        long result = 1L;
        for (; n > 0; n >>= 1) {
            if ((n & 1) > 0) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def stringCount(self, n: int) -> int:
        return (pow(26, n, 10 ** 9 + 7) - pow(25, n - 1, 10 ** 9 + 7) * (75 + n) + pow(24, n - 1, 10 ** 9 + 7) * (72 + (n << 1)) - pow(23, n - 1, 10 ** 9 + 7) * (23 + n)) % (10 ** 9 + 7)
```
