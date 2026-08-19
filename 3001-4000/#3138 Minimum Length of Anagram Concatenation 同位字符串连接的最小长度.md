# 3138 Minimum Length of Anagram Concatenation 同位字符串连接的最小长度

__Description:__

You are given a string `s`, which is known to be a concatenation of anagrams of some string `t`.

Return the minimum possible length of the string `t`.

An __anagram__ is formed by rearranging the letters of a string. For example, "aab", "aba", and, "baa" are anagrams of "aab".

__Example:__

Example 1:

```text
Input: s = "abba"

Output: 2

Explanation:

One possible string t could be "ba".
```

Example 2:

```text
Input: s = "cdef"

Output: 4

Explanation:

One possible string t could be "cdef", notice that t can be equal to s.
```

Example 3:

```text
Input: s = "abcbcacabbaccba"

Output: 3
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consist only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，它由某个字符串 `t` 和若干 `t` 的 __同位字符串__ 连接而成。

请你返回字符串 `t` 的 __最小__ 可能长度。

__同位字符串__ 指的是重新排列一个字符串的字母得到的另外一个字符串。例如，"aab"，"aba" 和 "baa" 是 "aab" 的同位字符串。

__示例:__

示例 1：

```text
输入：s = "abba"

输出：2

解释：

一个可能的字符串 t 为 "ba" 。
```

示例 2：

```text
输入：s = "cdef"

输出：4

解释：

一个可能的字符串 t 为 "cdef" ，注意 t 可能等于 s 。
```

示例 3：

```text
输入：s = "abcbcacabbaccba"

输出：3
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
模拟
每 k 个统计子字符串的字符数目, 其中 k 为字符串总长度的因子
由于字符串中只有小写字符
这里 k 一定不超过各字符的 gcd, 否则无法构成同位字符串
n 个字符可以分成 n // k 段
检查这一段的字符数是否相等
找到第一个完全相等的就直接返回 k
时间复杂度为 O(CN), 空间复杂度为 O(1), 实际上需要 26 个小写字符的空间, 其中 C 表示 N 的因子数, 可以通过分解质因子得到
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minAnagramLength(string s) 
    {
        int n = s.size(), chars[26]{}, g = 0;
        for (const auto& c : s) ++chars[c - 'a'];
        for (const auto& c : chars) g = gcd(g, c);
        for (int times = g; times > 1; times--) 
        {
            bool flag = true;
            if (!(g % times)) 
            {
                int k = n / times, cur[26]{};
                for (int i = 0; i < k; i++) ++cur[s[i] - 'a'];
                for (int i = k << 1; i <= n and flag; i += k) 
                {
                    int t[26]{};
                    for (int j = i - k; j < i; j++) ++t[s[j] - 'a'];
                    for (int j = 0; j < 26 and flag; j++) if (t[j] != cur[j]) flag = false;
                }
                if (flag) return k;
            }
        }
        return n;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAnagramLength(String s) {
        int n = s.length(), chars[] = new int[26], g = 0;
        for (var c : s.toCharArray()) ++chars[c - 'a'];
        for (int c : chars) g = gcd(g, c);
        for (int times = g; times > 1; times--) {
            boolean flag = true;
            if (g % times == 0) {
                int k = n / times, cur[] = new int[26];
                for (int i = 0; i < k; i++) ++cur[s.charAt(i) - 'a'];
                for (int i = k << 1; i <= n && flag; i += k) {
                    var t = new int[26];
                    for (int j = i - k; j < i; j++) ++t[s.charAt(j) - 'a'];
                    if (!Arrays.equals(t, cur)) flag = false;
                }
                if (flag) return k;
            }
        }
        return n;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def minAnagramLength(self, s: str) -> int:
        return min((k for k in range(1, (len(s) >> 1) + 1) if not len(s) % k and all(sorted(s[i - k: i]) == sorted(s[:k]) for i in range(k << 1, len(s) + 1, k))), default=len(s))
```
