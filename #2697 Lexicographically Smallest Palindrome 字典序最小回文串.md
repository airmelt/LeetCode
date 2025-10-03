# 2697 Lexicographically Smallest Palindrome 字典序最小回文串

__Description:__

You are given a string `s` consisting of __lowercase English letters__, and you are allowed to perform operations on it. In one operation, you can __replace__ a character in `s` with another lowercase English letter.

Your task is to make `s` a __palindrome__ with the __minimum__ __number__ __of operations__ possible. If there are __multiple palindromes__ that can be made using the __minimum__ number of operations, make the __lexicographically smallest__ one.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`.

Return the resulting palindrome string.

__Example:__

Example 1:

```text
Input: s = "egcfe"
Output: "efcfe"
Explanation: The minimum number of operations to make "egcfe" a palindrome is 1, and the lexicographically smallest palindrome string we can get by modifying one character is "efcfe", by changing 'g'.
```

Example 2:

```text
Input: s = "abcd"
Output: "abba"
Explanation: The minimum number of operations to make "abcd" a palindrome is 2, and the lexicographically smallest palindrome string we can get by modifying two characters is "abba".
```

Example 3:

```text
Input: s = "seven"
Output: "neven"
Explanation: The minimum number of operations to make "seven" a palindrome is 1, and the lexicographically smallest palindrome string we can get by modifying one character is "neven".
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s` consists of only lowercase English letters _._

__题目描述:__

给你一个由 __小写英文字母__ 组成的字符串 `s` ，你可以对其执行一些操作。在一步操作中，你可以用其他小写英文字母 __替换__  `s` 中的一个字符。

请你执行 __尽可能少的操作__ ，使 `s` 变成一个 __回文串__ 。如果执行 __最少__ 操作次数的方案不止一种，则只需选取 __字典序最小__ 的方案。

对于两个长度相同的字符串 `a` 和 `b` ，在 `a` 和 `b` 出现不同的第一个位置，如果该位置上 `a` 中对应字母比 `b` 中对应字母在字母表中出现顺序更早，则认为 `a` 的字典序比 `b` 的字典序要小。

返回最终的回文字符串。

__示例:__

示例 1：

```text
输入：s = "egcfe"
输出："efcfe"
解释：将 "egcfe" 变成回文字符串的最小操作次数为 1 ，修改 1 次得到的字典序最小回文字符串是 "efcfe"，只需将 'g' 改为 'f' 。
```

示例 2：

```text
输入：s = "abcd"
输出："abba"
解释：将 "abcd" 变成回文字符串的最小操作次数为 2 ，修改 2 次得到的字典序最小回文字符串是 "abba" 。
```

示例 3：

```text
输入：s = "seven"
输出："neven"
解释：将 "seven" 变成回文字符串的最小操作次数为 1 ，修改 1 次得到的字典序最小回文字符串是 "neven" 。
```

__提示：__

- `1 <= s.length <= 1000`
- `s` 仅由小写英文字母组成

__思路:__

```text
贪心
对称的比较 s 逆序对应位置的字符
选择较小的字符即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string makeSmallestPalindrome(string s) 
    {
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            if (s[i] > s[n - i - 1]) s[i] = s[n - i - 1];
            else s[n - i - 1] = s[i];
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String makeSmallestPalindrome(String s) {
        char[] chars = s.toCharArray();
        for (int i = 0, n = chars.length; i < n; i++) {
            if (chars[i] > chars[n - i - 1]) chars[i] = chars[n - i - 1];
            else chars[n - i - 1] = chars[i];
        }
        return new String(chars);
    }
}
```

__Python__:

```Python
class Solution:
    def makeSmallestPalindrome(self, s: str) -> str:
        return ''.join(min(s[i], s[-i - 1]) for i in range(len(s)))
```
