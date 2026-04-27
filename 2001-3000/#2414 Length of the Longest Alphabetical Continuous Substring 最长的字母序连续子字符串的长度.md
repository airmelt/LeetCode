# 2414 Length of the Longest Alphabetical Continuous Substring 最长的字母序连续子字符串的长度

__Description:__

An __alphabetical continuous string__ is a string consisting of consecutive letters in the alphabet. In other words, it is any substring of the string `"abcdefghijklmnopqrstuvwxyz"`.

- For example, `"abc"` is an alphabetical continuous string, while `"acb"` and `"za"` are not.

Given a string `s` consisting of lowercase letters only, return the _length of the __longest__ alphabetical continuous substring._

__Example:__

Example 1:

```text
Input: s = "abacaba"
Output: 2
Explanation: There are 4 distinct continuous substrings: "a", "b", "c" and "ab".
"ab" is the longest continuous substring.
```

Example 2:

```text
Input: s = "abcde"
Output: 5
Explanation: "abcde" is the longest continuous substring.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only English lowercase letters.

__题目描述:__

__字母序连续字符串__ 是由字母表中连续字母组成的字符串。换句话说，字符串 `"abcdefghijklmnopqrstuvwxyz"` 的任意子字符串都是 __字母序连续字符串__ 。

- 例如， `"abc"` 是一个字母序连续字符串，而 `"acb"` 和 `"za"` 不是。

给你一个仅由小写英文字母组成的字符串 `s` ，返回其 __最长__ 的 字母序连续子字符串 的长度。

__示例:__

示例 1：

```text
输入：s = "abacaba"
输出：2
解释：共有 4 个不同的字母序连续子字符串 "a"、"b"、"c" 和 "ab" 。
"ab" 是最长的字母序连续子字符串。
```

示例 2：

```text
输入：s = "abcde"
输出：5
解释："abcde" 是最长的字母序连续子字符串。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写英文字母组成

__思路:__

```text
1. 前缀和
记录当前连续子字符串的长度
每一次和之前的连续子字符串进行比较
如果当前字符和前一个字符连续, 那么当前连续子字符串长度加一
否则, 重置当前连续子字符串长度为 1
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 子串枚举
从 "abcdefghijklmnopqrstuvwxyz" 中枚举所有可能的连续子字符串
返回其中 s 最长的子字符串的长度
时间复杂度为 O(26 * 26 * N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestContinuousSubstring(string s) 
    {
        int result = 1, cur = 1, n = s.size();
        for (int i = 1; i < n; i++) result = max(result, cur = s[i - 1] + 1 == s[i] ? cur + 1 : 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestContinuousSubstring(String s) {
        int result = 1, cur = 1, n = s.length();
        for (int i = 1; i < n; i++) result = Math.max(result, cur = s.charAt(i - 1) + 1 == s.charAt(i) ? cur + 1 : 1);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestContinuousSubstring(self, s: str) -> int:
        return max(j - i + 1 for i in range(26) for j in range(i, 26) if "abcdefghijklmnopqrstuvwxyz"[i:j + 1] in s)
```
