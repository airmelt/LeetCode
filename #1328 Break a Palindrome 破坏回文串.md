# 1328 Break a Palindrome 破坏回文串

__Description:__

Given a palindromic string of lowercase English letters palindrome, replace exactly one character with any lowercase English letter so that the resulting string is not a palindrome and that it is the lexicographically smallest one possible.

Return the resulting string. If there is no way to replace a character to make it not a palindrome, return an empty string.

A string a is lexicographically smaller than a string b (of the same length) if in the first position where a and b differ, a has a character strictly smaller than the corresponding character in b. For example, "abcc" is lexicographically smaller than "abcd" because the first position they differ is at the fourth character, and 'c' is smaller than 'd'.

__Example:__

Example 1:

Input: palindrome = "abccba"
Output: "aaccba"
Explanation: There are many ways to make "abccba" not a palindrome, such as "zbccba", "aaccba", and "abacba".
Of all the ways, "aaccba" is the lexicographically smallest.

Example 2:

Input: palindrome = "a"
Output: ""
Explanation: There is no way to replace a single character to make "a" not a palindrome, so return an empty string.

__Constraints:__

1 <= palindrome.length <= 1000
palindrome consists of only lowercase English letters.

__题目描述：__

给你一个由小写英文字母组成的回文字符串 palindrome ，请你将其中 一个 字符用任意小写英文字母替换，使得结果字符串的 字典序最小 ，且 不是 回文串。

请你返回结果字符串。如果无法做到，则返回一个 空串 。

如果两个字符串长度相同，那么字符串 a 字典序比字符串 b 小可以这样定义：在 a 和 b 出现不同的第一个位置上，字符串 a 中的字符严格小于 b 中的对应字符。例如，"abcc” 字典序比 "abcd" 小，因为不同的第一个位置是在第四个字符，显然 'c' 比 'd' 小。

__示例：__

示例 1：

输入：palindrome = "abccba"
输出："aaccba"
解释：存在多种方法可以使 "abccba" 不是回文，例如 "zbccba", "aaccba", 和 "abacba" 。
在所有方法中，"aaccba" 的字典序最小。

示例 2：

输入：palindrome = "a"
输出：""
解释：不存在替换一个字符使 "a" 变成非回文的方法，所以返回空字符串。

__提示：__

1 <= palindrome.length <= 1000
palindrome 只包含小写英文字母。

__思路：__

贪心
长度不足 2 的直接返回空字符串
把第一个不是字符 'a' 的换成字符 'a'
否则把最后一个字符换成字符 'b'
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    string breakPalindrome(string palindrome) 
    {
        for (int i = 0; i < palindrome.size() >> 1; i++) if (palindrome[i] != 'a' and (palindrome[i] = 'a')) return palindrome;
        return palindrome.size() > 1 and (palindrome.back() = 'b') ? palindrome : "";
    }
};
```

__Java__:

```Java
class Solution {
    public String breakPalindrome(String palindrome) {
        for (int i = 0; i < palindrome.length() >>> 1; i++) if (palindrome.charAt(i) != 'a') return palindrome.substring(0, i) + 'a' + palindrome.substring(i + 1);
        return palindrome.length() > 1 ? palindrome.substring(0, palindrome.length() - 1) + 'b' : "";
    }
}
```

__Python__:

```Python
class Solution:
    def breakPalindrome(self, palindrome: str) -> str:
        for i in range(len(palindrome) >> 1):
            if palindrome[i] != 'a':
                return palindrome[:i] + 'a' + palindrome[i + 1:]
        return palindrome[:-1] + 'b' if len(palindrome) > 1 else ''
```
