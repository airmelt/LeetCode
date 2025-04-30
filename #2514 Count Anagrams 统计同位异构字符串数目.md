# 2514 Count Anagrams 统计同位异构字符串数目

__Description:__

You are given a string `s` containing one or more words. Every consecutive pair of words is separated by a single space `' '`.

A string `t` is an __anagram__ of string `s` if the `i ^ th` word of `t` is a __permutation__ of the `i ^ th` word of `s`.

- For example, `"acb dfe"` is an anagram of `"abc def"`, but `"def cab"` and `"adc bef"` are not.

Return _the number of __distinct anagrams__ of_ `s`. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "too hot"
Output: 18
Explanation: Some of the anagrams of the given string are "too hot", "oot hot", "oto toh", "too toh", and "too oht".
```

Example 2:

```text
Input: s = "aa"
Output: 1
Explanation: There is only one anagram possible for the given string.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters and spaces `' '`.
- There is single space between consecutive words.

__题目描述:__

给你一个字符串 `s` ，它包含一个或者多个单词。单词之间用单个空格 `' '` 隔开。

如果字符串 `t` 中第 `i` 个单词是 `s` 中第 `i` 个单词的一个 __排列__ ，那么我们称字符串 `t` 是字符串 `s` 的同位异构字符串。

- 比方说， `"acb dfe"` 是 `"abc def"` 的同位异构字符串，但是 `"def cab"` 和 `"adc bef"` 不是。

请你返回 `s` 的同位异构字符串的数目，由于答案可能很大，请你将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：s = "too hot"
输出：18
解释：输入字符串的一些同位异构字符串为 "too hot" ，"oot hot" ，"oto toh" ，"too toh" 以及 "too oht" 。
```

示例 2：

```text
输入：s = "aa"
输出：1
解释：输入字符串只有一个同位异构字符串。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母和空格 `' '` 。
- 相邻单词之间由单个空格隔开。

__思路:__

```text
数学
将字符串分割成单词
统计每个单词的字符频率
计算每个单词的排列数
使用组合数学公式计算排列数
每个单词的排列数公式为 A(n, k), n 为单词长度, k 为字符频率
将每个单词的排列数相乘
这里需要使用逆元加快计算速度
最后对结果取模
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int countAnagrams(string s) {
        
    }
};
```

__Java__:

```Java
class Solution {
    public int countAnagrams(String s) {
        
    }
}
```

__Python__:

```Python
class Solution:
    def countAnagrams(self, s: str) -> int:
        d, result, MOD = defaultdict(int), 1, 10 ** 9 + 7
        for i in s.split():
            d[''.join(sorted(list(i)))] += 1
        for k, v in d.items():
            cur = defaultdict(int)
            c = 1
            for j in k:
                cur[j] += 1
            for v2 in cur.values():
                c *= factorial(v2)
            p = factorial(len(k))
            result *= p // c
            result %= MOD
        return result
```
