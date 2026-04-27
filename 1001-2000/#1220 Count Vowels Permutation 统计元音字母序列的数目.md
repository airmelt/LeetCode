# 1220 Count Vowels Permutation 统计元音字母序列的数目

__Description:__

Given an integer n, your task is to count how many strings of length n can be formed under the following rules:

Each character is a lower case vowel ('a', 'e', 'i', 'o', 'u')
Each vowel 'a' may only be followed by an 'e'.
Each vowel 'e' may only be followed by an 'a' or an 'i'.
Each vowel 'i' may not be followed by another 'i'.
Each vowel 'o' may only be followed by an 'i' or a 'u'.
Each vowel 'u' may only be followed by an 'a'.
Since the answer may be too large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 1
Output: 5
Explanation: All possible strings are: "a", "e", "i" , "o" and "u".

Example 2:

Input: n = 2
Output: 10
Explanation: All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".

Example 3:

Input: n = 5
Output: 68

__Constraints:__

1 <= n <= 2 * 10^4

__题目描述：__

给你一个整数 n，请你帮忙统计一下我们可以按下述规则形成多少个长度为 n 的字符串：

字符串中的每个字符都应当是小写元音字母（'a', 'e', 'i', 'o', 'u'）
每个元音 'a' 后面都只能跟着 'e'
每个元音 'e' 后面只能跟着 'a' 或者是 'i'
每个元音 'i' 后面 不能 再跟着另一个 'i'
每个元音 'o' 后面只能跟着 'i' 或者是 'u'
每个元音 'u' 后面只能跟着 'a'
由于答案可能会很大，所以请你返回 模 10^9 + 7 之后的结果。

__示例：__

示例 1：

输入：n = 1
输出：5
解释：所有可能的字符串分别是："a", "e", "i" , "o" 和 "u"。

示例 2：

输入：n = 2
输出：10
解释：所有可能的字符串分别是："ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" 和 "ua"。

示例 3：

输入：n = 5
输出：68

__提示：__

1 <= n <= 2 * 10^4

__思路：__

动态规划
应该是最简单的困难动态规划
设 a, e, i, o, u 分别表示对应字母结尾的排列数
根据题意, 以 'a' 结尾的只能是 'e', 'i',  'o', a = (e + i + o) % mod
其他的以此类推
最后返回所有的排列数的和并取余
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int countVowelPermutation(int n) 
    {
        long a = 1, e = 1, i = 1, o = 1, u = 1, mod = 1e9 + 7;
        while (--n) 
        {
            long aa = (e + i + u) % mod, ee = (a + i) % mod, ii = (e + o) % mod, oo = i % mod, uu = (o + i) % mod;
            a = aa;
            e = ee;
            i = ii;
            o = oo;
            u = uu;
        }
        return (a + e + i + o + u) % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int countVowelPermutation(int n) {
        long a = 1, e = 1, i = 1, o = 1, u = 1, mod = 1_000_000_007;
        while (--n > 0) {
            long aa = (e + i + u) % mod, ee = (a + i) % mod, ii = (e + o) % mod, oo = i % mod, uu = (o + i) % mod;
            a = aa;
            e = ee;
            i = ii;
            o = oo;
            u = uu;
        }
        return (int)((a + e + i + o + u) % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def countVowelPermutation(self, n: int) -> int:
        a, e, i, o, u, mod = 1, 1, 1, 1, 1, 10 ** 9 + 7
        for _ in range(n - 1):
            a, e, i, o, u = (e + i + u) % mod, (a + i) % mod, (e + o) % mod, i % mod, (o + i) % mod
        return sum([a, e, i, o, u]) % mod
```
