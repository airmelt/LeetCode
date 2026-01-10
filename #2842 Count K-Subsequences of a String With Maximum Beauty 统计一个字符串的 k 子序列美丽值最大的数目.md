# 2842 Count K-Subsequences of a String With Maximum Beauty 统计一个字符串的 k 子序列美丽值最大的数目

__Description:__

You are given a string `s` and an integer `k`.

A __k-subsequence__ is a __subsequence__ of `s`, having length `k`, and all its characters are __unique__, __i.e__., every character occurs once.

Let `f(c)` denote the number of times the character `c` occurs in `s`.

The __beauty__ of a __k-subsequence__ is the __sum__ of `f(c)` for every character `c` in the k-subsequence.

For example, consider `s = "abbbdd"` and `k = 2`:

- `f('a') = 1`, `f('b') = 3`, `f('d') = 2`
- Some k-subsequences of `s` are:
  - "__ab__ bbdd" -> "ab" having a beauty of `f('a') + f('b') = 4`
  - "__a__ bbb __d__ d" -> "ad" having a beauty of `f('a') + f('d') = 3`
  - "a __b__ bb __d__ d" -> "bd" having a beauty of `f('b') + f('d') = 5`

Return _an integer denoting the number of k-subsequences_ _whose __beauty__ is the __maximum__ among all __k-subsequences___. Since the answer may be too large, return it modulo `10 ^ 9 + 7`.

A subsequence of a string is a new string formed from the original string by deleting some (possibly none) of the characters without disturbing the relative positions of the remaining characters.

Notes

- `f(c)` is the number of times a character `c` occurs in `s`, not a k-subsequence.
- Two k-subsequences are considered different if one is formed by an index that is not present in the other. So, two k-subsequences may form the same string.

__Example:__

Example 1:

```text
Input: s = "bcca", k = 2
Output: 4
Explanation: From s we have f('a') = 1, f('b') = 1, and f('c') = 2.
The k-subsequences of s are: 
bcca having a beauty of f('b') + f('c') = 3 
bcca having a beauty of f('b') + f('c') = 3 
bcca having a beauty of f('b') + f('a') = 2 
bcca having a beauty of f('c') + f('a') = 3
bcca having a beauty of f('c') + f('a') = 3 
There are 4 k-subsequences that have the maximum beauty, 3. 
Hence, the answer is 4.
```

Example 2:

```text
Input: s = "abbcd", k = 4
Output: 2
Explanation: From s we have f('a') = 1, f('b') = 2, f('c') = 1, and f('d') = 1. 
The k-subsequences of s are: 
abbcd having a beauty of f('a') + f('b') + f('c') + f('d') = 5
abbcd having a beauty of f('a') + f('b') + f('c') + f('d') = 5 
There are 2 k-subsequences that have the maximum beauty, 5. 
Hence, the answer is 2.
```

__Constraints:__

- `1 <= s.length <= 2 * 10 ^ 5`
- `1 <= k <= s.length`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个整数 `k` 。

__k 子序列__ 指的是 `s` 的一个长度为 `k` 的 __子序列__ ，且所有字符都是 __唯一__ 的，也就是说每个字符在子序列里只出现过一次。

定义 `f(c)` 为字符 `c` 在 `s` 中出现的次数。

k 子序列的 __美丽值__ 定义为这个子序列中每一个字符 `c` 的 `f(c)` 之 __和__ 。

比方说， `s = "abbbdd"` 和 `k = 2` ，我们有:

- `f('a') = 1`, `f('b') = 3`, `f('d') = 2`
- `s` 的部分 k 子序列为:
  - "___ab___ bbdd" -> "ab" ，美丽值为 `f('a') + f('b') = 4`
  - "___a___ bbb ___d___ d" -> "ad" ，美丽值为 `f('a') + f('d') = 3`
  - "a ___b___ bb ___d___ d" -> "bd" ，美丽值为 `f('b') + f('d') = 5`

请你返回一个整数，表示所有 __k 子序列__ 里面 __美丽值__ 是 __最大值__ 的子序列数目。由于答案可能很大，将结果对 `10 ^ 9 + 7` 取余后返回。

一个字符串的子序列指的是从原字符串里面删除一些字符（也可能一个字符也不删除），不改变剩下字符顺序连接得到的新字符串。

注意：

- `f(c)` 指的是字符 `c` 在字符串 `s` 的出现次数，不是在 k 子序列里的出现次数。
- 两个 k 子序列如果有任何一个字符在原字符串中的下标不同，则它们是两个不同的子序列。所以两个不同的 k 子序列可能产生相同的字符串。

__示例:__

示例 1：

```text
输入：s = "bcca", k = 2
输出：4
解释：s 中我们有 f('a') = 1 ，f('b') = 1 和 f('c') = 2 。
s 的 k 子序列为：
bcca ，美丽值为 f('b') + f('c') = 3
bcca ，美丽值为 f('b') + f('c') = 3
bcca ，美丽值为 f('b') + f('a') = 2
bcca ，美丽值为 f('c') + f('a') = 3
bcca ，美丽值为 f('c') + f('a') = 3
总共有 4 个 k 子序列美丽值为最大值 3 。
所以答案为 4 。
```

示例 2：

```text
输入：s = "abbcd", k = 4
输出：2
解释：s 中我们有 f('a') = 1 ，f('b') = 2 ，f('c') = 1 和 f('d') = 1 。
s 的 k 子序列为：
abbcd ，美丽值为 f('a') + f('b') + f('c') + f('d') = 5
abbcd ，美丽值为 f('a') + f('b') + f('c') + f('d') = 5 
总共有 2 个 k 子序列美丽值为最大值 5 。
所以答案为 2 。
```

__提示：__

- `1 <= s.length <= 2 * 10 ^ 5`
- `1 <= k <= s.length`
- `s` 只包含小写英文字母。

__思路:__

```text
数学
如果 k = 1, 只需要选出现次数最大的字符
如果 k = 2, 那么要从 t 个出现次数最多的字符中选择 2 个每种都可以有 v 种选发, v 为出现次数, 即 v ^ t * C(t, k)
所以计算每个字符出现的次数, 按照出现次数从大到小排列出现的次数
如果当前的出现次数不小于 k, 那么取 v ^ t * C(t, k)
否则取 v ^ t, 再用 k 减去 t
如果 k 比所有字符出现次数之和还大, 返回 0
这里计算 pow 可以用快速幂
计算组合数, 由于最大只会到 26, 可以用定义直接计算, 如果过大可以使用逆元
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countKSubsequencesWithMaxBeauty(string s, int k) 
    {
        int c[26]{};
        for (const auto& cur : s) ++c[cur - 'a'];
        map<int, int> m;
        for (const auto& v : c) if (v) ++m[-v];
        long long result = 1LL;
        for (const auto& [v, t]: m) 
        {
            if (t >= k) return result * pow(-v, k) % MOD * comb(t, k) % MOD;
            result = result * pow(-v, t) % MOD;
            k -= t;
        }
        return 0;
    }
private:
    static constexpr long long MOD = 1e9 + 7;

long long pow(long long a, int n) 
{
        long long result = 1LL;
        for (; n; n >>= 1) 
        {
            if (n & 1) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }

    long long comb(long long n, int k) 
    {
        auto result = n;
        for (int i = 2; i <= k; i++) result = result * --n / i;
        return result % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    private static final long MOD = 1_000_000_007L;

    public int countKSubsequencesWithMaxBeauty(String s, int k) {
        var c = new int[26];
        for (var cur : s.toCharArray()) ++c[cur - 'a'];
        var map = new TreeMap<Integer, Integer>();
        for (int v : c) if (v > 0) map.merge(v, 1, Integer::sum);
        long result = 1L;
        for (var entry : map.descendingMap().entrySet()) {
            if (entry.getValue() >= k) return (int)(result * quickPow(entry.getKey(), k) % MOD * comb(entry.getValue(), k) % MOD);
            result = result * quickPow(entry.getKey(), entry.getValue()) % MOD;
            k -= entry.getValue();
        }
        return 0;
    }

    private long quickPow(long a, int n) {
        long result = 1L;
        for (; n > 0; n >>= 1) {
            if ((n & 1) > 0) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }

    private long comb(long n, int k) {
        long result = n;
        for (int i = 2; i <= k; i++) result = result * --n / i;
        return result % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def countKSubsequencesWithMaxBeauty(self, s: str, k: int) -> int:
        result, MOD, c = 1, 10 ** 9 + 7, Counter(Counter(s).values())
        for v, t in sorted(c.items(), reverse=True):
            if t >= k:
                return result * pow(v, k, MOD) * comb(t, k) % MOD
            result *= pow(v, t, MOD)
            k -= t
        return 0
```
