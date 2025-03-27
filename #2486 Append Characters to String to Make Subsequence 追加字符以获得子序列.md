# 2486 Append Characters to String to Make Subsequence 追加字符以获得子序列

__Description:__

You are given two strings `s` and `t` consisting of only lowercase English letters.

Return _the minimum number of characters that need to be appended to the end of_ `s` _so that_ `t` _becomes a __subsequence__ of_ `s`.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

__Example:__

Example 1:

```text
Input: s = "coaching", t = "coding"
Output: 4
Explanation: Append the characters "ding" to the end of s so that s = "coachingding".
Now, t is a subsequence of s ("coachingding").
It can be shown that appending any 3 characters to the end of s will never make t a subsequence.
```

Example 2:

```text
Input: s = "abcde", t = "a"
Output: 0
Explanation: t is already a subsequence of s ("abcde").
```

Example 3:

```text
Input: s = "z", t = "abcde"
Output: 5
Explanation: Append the characters "abcde" to the end of s so that s = "zabcde".
Now, t is a subsequence of s ("zabcde").
It can be shown that appending any 4 characters to the end of s will never make t a subsequence.
```

__Constraints:__

- `1 <= s.length, t.length <= 10 ^ 5`
- `s` and `t` consist only of lowercase English letters.

__题目描述:__

给你两个仅由小写英文字母组成的字符串 `s` 和 `t` 。

现在需要通过向 `s` 末尾追加字符的方式使 `t` 变成 `s` 的一个 __子序列__ ，返回需要追加的最少字符数。

子序列是一个可以由其他字符串删除部分（或不删除）字符但不改变剩下字符顺序得到的字符串。

__示例:__

示例 1：

```text
输入：s = "coaching", t = "coding"
输出：4
解释：向 s 末尾追加字符串 "ding" ，s = "coachingding" 。
现在，t 是 s ("coachingding") 的一个子序列。
可以证明向 s 末尾追加任何 3 个字符都无法使 t 成为 s 的一个子序列。
```

示例 2：

```text
输入：s = "abcde", t = "a"
输出：0
解释：t 已经是 s ("abcde") 的一个子序列。
```

示例 3：

```text
输入：s = "z", t = "abcde"
输出：5
解释：向 s 末尾追加字符串 "abcde" ，s = "zabcde" 。
现在，t 是 s ("zabcde") 的一个子序列。 
可以证明向 s 末尾追加任何 4 个字符都无法使 t 成为 s 的一个子序列。
```

__提示：__

- `1 <= s.length, t.length <= 10 ^ 5`
- `s` 和 `t` 仅由小写英文字母组成

__思路:__

```text
双指针
遍历 s
如果当前字符和 t 的当前字符相同，指针 i 向后移动
返回 t 的长度 - i
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int appendCharacters(string s, string t) 
    {
        int i = 0, n = t.size();
        for (const auto& c : s) if (i < n and t[i] == c) ++i;
        return n - i;
    }
};
```

__Java__:

```Java
class Solution {
    public int appendCharacters(String s, String t) {
        int i = 0, n = t.length();
        for (char c : s.toCharArray()) if (i < n && t.charAt(i) == c) ++i;
        return n - i;
    }
}
```

__Python__:

```Python
class Solution:
    def appendCharacters(self, s: str, t: str) -> int:
        n, i = len(t), 0
        for c in s:
            if i < n and c == t[i]:
                i += 1
        return n - i
```
