# 2663 Lexicographically Smallest Beautiful String 字典序最小的美丽字符串

__Description:__

A string is beautiful if:

- It consists of the first `k` letters of the English lowercase alphabet.
- It does not contain any substring of length `2` or more which is a palindrome.

You are given a beautiful string `s` of length `n` and a positive integer `k`.

Return _the lexicographically smallest string of length_ `n`_, which is larger than_ `s` _and is __beautiful___. If there is no such string, return an empty string.

A string `a` is lexicographically larger than a string `b` (of the same length) if in the first position where `a` and `b` differ, `a` has a character strictly larger than the corresponding character in `b`.

- For example, `"abcd"` is lexicographically larger than `"abcc"` because the first position they differ is at the fourth character, and `d` is greater than `c`.

__Example:__

Example 1:

```text
Input: s = "abcz", k = 26
Output: "abda"
Explanation: The string "abda" is beautiful and lexicographically larger than the string "abcz".
It can be proven that there is no string that is lexicographically larger than the string "abcz", beautiful, and lexicographically smaller than the string "abda".
```

Example 2:

```text
Input: s = "dc", k = 4
Output: ""
Explanation: It can be proven that there is no string that is lexicographically larger than the string "dc" and is beautiful.
```

__Constraints:__

- `1 <= n == s.length <= 10 ^ 5`
- `4 <= k <= 26`
- `s` is a beautiful string.

__题目描述:__

如果一个字符串满足以下条件，则称其为 美丽字符串 ：

- 它由英语小写字母表的前 `k` 个字母组成。
- 它不包含任何长度为 `2` 或更长的回文子字符串。

给你一个长度为 `n` 的美丽字符串 `s` 和一个正整数 `k` 。

请你找出并返回一个长度为 `n` 的美丽字符串，该字符串还满足:在字典序大于 `s` 的所有美丽字符串中字典序最小。如果不存在这样的字符串，则返回一个空字符串。

对于长度相同的两个字符串 `a` 和 `b` ，如果字符串 `a` 在与字符串 `b` 不同的第一个位置上的字符字典序更大，则字符串 `a` 的字典序大于字符串 `b` 。

- 例如， `"abcd"` 的字典序比 `"abcc"` 更大，因为在不同的第一个位置（第四个字符）上 `d` 的字典序大于 `c` 。

__示例:__

示例 1：

```text
输入：s = "abcz", k = 26
输出："abda"
解释：字符串 "abda" 既是美丽字符串，又满足字典序大于 "abcz" 。
可以证明不存在字符串同时满足字典序大于 "abcz"、美丽字符串、字典序小于 "abda" 这三个条件。
```

示例 2：

```text
输入：s = "dc", k = 4
输出：""
解释：可以证明，不存在既是美丽字符串，又字典序大于 "dc" 的字符串。
```

__提示：__

- `1 <= n == s.length <= 10 ^ 5`
- `4 <= k <= 26`
- `s` 是一个美丽字符串

__思路:__

```text
贪心
如果不能包含长度大于等于 2 的回文子串
那么 s[i] != s[i - 1] 且 s[i] != s[i - 2]
s[i + 1] 以及 s[i + 2] 可以保证 s[i] 之后的字符合法
以 dcdd, k = 4 为例
首先将 s[n - 1] 加 1, 这样就保证一定比 s 大, 即 ddaa, 注意这里需要进位
此时 "aa" 为回文, 那就继续加 1, 变成 ddab
此时 "dd" 为回文, 那就继续加 1, 变成 eabc, i 已经移动到首位, 无法操作
返回 "", 空字符串
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string smallestBeautifulString(string s, int k) 
    {
        k += 'a';
        int n = s.size(), i = n - 1;
        ++s[i];
        while (i < n) 
        {
            if (s[i] == k) 
            {
                if (i == 0) return "";
                s[i] = 'a';
                ++s[--i];
            } 
            else if (i and s[i] == s[i - 1] or i > 1 and s[i] == s[i - 2]) ++s[i];
            else ++i;
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestBeautifulString(String s, int k) {
        k += 'a';
        char[] result = s.toCharArray();
        int n = s.length(), i = n - 1;
        ++result[i];
        while (i < n) {
            if (result[i] == k) {
                if (i == 0) return "";
                result[i] = 'a';
                ++result[--i];
            } else if (i > 0 && result[i] == result[i - 1] || i > 1 && result[i] == result[i - 2]) ++result[i];
            else ++i;
        }
        return new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def smallestBeautifulString(self, s: str, k: int) -> str:
        k, result, i = k + (a := ord('a')), list(map(ord, s)), (n := len(s)) - 1
        result[i] += 1
        while i < n:
            if result[i] == k:
                if not i:
                    return ""
                result[i] = a
                i -= 1
                result[i] += 1
            elif i and result[i] == result[i - 1] or i > 1 and result[i] == result[i - 2]:
                result[i] += 1  
            else:
                i += 1 
        return ''.join(map(chr, result))
```
