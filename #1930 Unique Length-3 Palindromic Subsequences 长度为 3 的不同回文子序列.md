# 1930 Unique Length-3 Palindromic Subsequences 长度为 3 的不同回文子序列

__Description:__

Given a string `s`, return _the number of __unique palindromes of length three__ that are a __subsequence__ of_ `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted once.

A palindrome is a string that reads the same forwards and backwards.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

__Example:__

Example 1:

```text
Input: s = "aabca"
Output: 3
Explanation: The 3 palindromic subsequences of length 3 are:
```

- "aba" (subsequence of "aabca")
- "aaa" (subsequence of "aabca")
- "aca" (subsequence of "aabca")

Example 2:

```text
Input: s = "adc"
Output: 0
Explanation: There are no palindromic subsequences of length 3 in "adc".
```

Example 3:

```text
Input: s = "bbcbaba"
Output: 4
Explanation: The 4 palindromic subsequences of length 3 are:
```

- "bbb" (subsequence of "bbcbaba")
- "bcb" (subsequence of "bbcbaba")
- "bab" (subsequence of "bbcbaba")
- "aba" (subsequence of "bbcbaba")

__Constraints:__

- `3 <= s.length <= 10 ^ 5`
- `s` consists of only lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，返回 `s` 中 __长度为 3__ 的 __不同回文子序列__ 的个数。

即便存在多种方法来构建相同的子序列，但相同的子序列只计数一次。

回文 是正着读和反着读一样的字符串。

子序列 是由原字符串删除其中部分字符（也可以不删除）且不改变剩余字符之间相对顺序形成的一个新字符串。

- 例如， `"ace"` 是 ___abcde___ 的一个子序列。

__示例:__

示例 1：

```text
输入：s = "aabca"
输出：3
解释：长度为 3 的 3 个回文子序列分别是：
```

- "aba" ("aabca" 的子序列)
- "aaa" ("aabca" 的子序列)
- "aca" ("aabca" 的子序列)

示例 2：

```text
输入：s = "adc"
输出：0
解释："adc" 不存在长度为 3 的回文子序列。
```

示例 3：

```text
输入：s = "bbcbaba"
输出：4
解释：长度为 3 的 4 个回文子序列分别是：
```

- "bbb" ("bbcbaba" 的子序列)
- "bcb" ("bbcbaba" 的子序列)
- "bab" ("bbcbaba" 的子序列)
- "aba" ("bbcbaba" 的子序列)

__提示：__

- `3 <= s.length <= 10 ^ 5`
- `s` 仅由小写英文字母组成

__思路:__

```text
状态压缩
用一个 26 位的二进制数表示当前字符是否出现过
枚举每一个中间的字符, 统计左右两边各有哪些字符出现过
用一个前缀和数组 left[i] 表示 [0, i) 中出现过的字符
用一个后缀和数组 right[i] 表示 (i, n] 中出现过的字符
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPalindromicSubsequence(string s) 
    {
        int n = s.size(), result = 0;
        vector<int> left(n + 2), right(n + 2), both(26);
        for (int i = 1; i <= n; i++) left[i] = left[i - 1] | (1 << (s[i - 1] - 'a'));
        for (int i = n; i >= 1; i--) right[i] = right[i + 1] | (1 << (s[i - 1] - 'a'));
        for (int i = 2; i < n; i++) both[s[i - 1] - 'a'] |= left[i - 1] & right[i + 1];
        for (const auto& state : both) result += __builtin_popcount(state);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPalindromicSubsequence(String s) {
        int n = s.length(), result = 0, left[] = new int[n + 2], right[] = new int[n + 2], both[] = new int[26];
        for (int i = 1; i <= n; i++) left[i] = left[i - 1] | (1 << (s.charAt(i - 1) - 'a'));
        for (int i = n; i >= 1; i--) right[i] = right[i + 1] | (1 << (s.charAt(i - 1) - 'a'));
        for (int i = 2; i < n; i++) both[s.charAt(i - 1) - 'a'] |= left[i - 1] & right[i + 1];
        for (int state : both) result += helper(state);
        return result;
    }

    private int helper(int state) {
        int n = 0;
        while (state > 0) {
            state &= state - 1;
            ++n;
        }
        return n;
    }
}
```

__Python__:

```Python
class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        result, left, right, both = 0, [0] * ((n := len(s)) + 2), [0] * (n + 2), [0] * 26
        for i in range(1, n + 1):
            left[i] = left[i - 1] | (1 << (ord(s[i - 1]) - ord('a')))
        for i in range(n, 0, -1):
            right[i] = right[i + 1] | (1 << (ord(s[i - 1]) - ord('a')))
        for i in range(2, n):
            both[(ord(s[i - 1]) - ord('a'))] |= left[i - 1] & right[i + 1]
        for state in both:
            result += bin(state).count('1')
        return result
```
