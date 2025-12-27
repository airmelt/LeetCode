# 2825 Make String a Subsequence Using Cyclic Increments 循环增长使字符串子序列等于另一个字符串

__Description:__

You are given two __0-indexed__ strings `str1` and `str2`.

In an operation, you select a __set__ of indices in `str1`, and for each index `i` in the set, increment `str1[i]` to the next character __cyclically__. That is `'a'` becomes `'b'`, `'b'` becomes `'c'`, and so on, and `'z'` becomes `'a'`.

Return `true` _if it is possible to make_ `str2` _a subsequence of_ `str1` _by performing the operation __at most once___, _and_ `false` _otherwise_.

Note: A subsequence of a string is a new string that is formed from the original string by deleting some (possibly none) of the characters without disturbing the relative positions of the remaining characters.

__Example:__

Example 1:

```text
Input: str1 = "abc", str2 = "ad"
Output: true
Explanation: Select index 2 in str1.
Increment str1[2] to become 'd'. 
Hence, str1 becomes "abd" and str2 is now a subsequence. Therefore, true is returned.
```

Example 2:

```text
Input: str1 = "zc", str2 = "ad"
Output: true
Explanation: Select indices 0 and 1 in str1. 
Increment str1[0] to become 'a'. 
Increment str1[1] to become 'd'. 
Hence, str1 becomes "ad" and str2 is now a subsequence. Therefore, true is returned.
```

Example 3:

```text
Input: str1 = "ab", str2 = "d"
Output: false
Explanation: In this example, it can be shown that it is impossible to make str2 a subsequence of str1 using the operation at most once. 
Therefore, false is returned.
```

__Constraints:__

- `1 <= str1.length <= 10 ^ 5`
- `1 <= str2.length <= 10 ^ 5`
- `str1` and `str2` consist of only lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `str1` 和 `str2` 。

一次操作中，你选择 `str1` 中的若干下标。对于选中的每一个下标 `i` ，你将 `str1[i]` __循环__ 递增，变成下一个字符。也就是说 `'a'` 变成 `'b'` ， `'b'` 变成 `'c'` ，以此类推， `'z'` 变成 `'a'` 。

如果执行以上操作 __至多一次__ ，可以让 `str2` 成为 `str1` 的子序列，请你返回 `true` ，否则返回 `false` 。

注意：一个字符串的子序列指的是从原字符串中删除一些（可以一个字符也不删）字符后，剩下字符按照原本先后顺序组成的新字符串。

__示例:__

示例 1：

```text
输入：str1 = "abc", str2 = "ad"
输出：true
解释：选择 str1 中的下标 2 。
将 str1[2] 循环递增，得到 'd' 。
因此，str1 变成 "abd" 且 str2 现在是一个子序列。所以返回 true 。
```

示例 2：

```text
输入：str1 = "zc", str2 = "ad"
输出：true
解释：选择 str1 中的下标 0 和 1 。
将 str1[0] 循环递增得到 'a' 。
将 str1[1] 循环递增得到 'd' 。
因此，str1 变成 "ad" 且 str2 现在是一个子序列。所以返回 true 。
```

示例 3：

```text
输入：str1 = "ab", str2 = "d"
输出：false
解释：这个例子中，没法在执行一次操作的前提下，将 str2 变为 str1 的子序列。
所以返回 false 。
```

__提示：__

- `1 <= str1.length <= 10 ^ 5`
- `1 <= str2.length <= 10 ^ 5`
- `str1` 和 `str2` 只包含小写英文字母。

__思路:__

```text
双指针
遍历 s 字符串
如果当前字符或者当前字符的下一个字符等于 t 的第 i 个字符
那么 i 向后移动直到遍历完 t
时间复杂度为 O(M + N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canMakeSubsequence(string str1, string str2) 
    {
        int m = str1.size(), n = str2.size(), i = 0, c = 0;
        for (const auto& b : str1) if (b == str2[i] or (b == 'z' ? 'a' : (b + 1)) == str2[i]) if (++i == n) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canMakeSubsequence(String str1, String str2) {
        int m = str1.length(), n = str2.length(), i = 0, c = 0;
        for (char b : str1.toCharArray()) if (b == str2.charAt(i) || (b == 'z' ? 'a' : (char)(b + 1)) == str2.charAt(i)) if (++i == n) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def canMakeSubsequence(self, s: str, t: str) -> bool:
        m, n, i = len(s), len(t), 0
        for b in s:
            if b == t[i] or (c := chr(ord(b) + 1) if b != 'z' else 'a') == t[i]:
                i += 1
                if i == len(t):
                    return True
        return False
```
