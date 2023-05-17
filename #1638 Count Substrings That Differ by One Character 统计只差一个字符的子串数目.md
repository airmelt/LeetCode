# 1638 Count Substrings That Differ by One Character 统计只差一个字符的子串数目

__Description:__

Given two strings `s` and `t`, find the number of ways you can choose a non-empty substring of `s` and replace a __single character__ by a different character such that the resulting substring is a substring of `t`. In other words, find the number of substrings in `s` that differ from some substring in `t` by __exactly__ one character.

For example, the underlined substrings in `"computer"` and `"computation"` only differ by the `'e'`/ `'a'`, so this is a valid way.

Return the number of substrings that satisfy the condition above.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "aba", t = "baba"
Output: 6
Explanation: The following are the pairs of substrings from s and t that differ by exactly 1 character:
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
The underlined portions are the substrings that are chosen from s and t.
```

Input: s = "ab", t = "bb"
Output: 3
Explanation: The following are the pairs of substrings from s and t that differ by 1 character:
("ab", "bb")
("ab", "bb")
("ab", "bb")
The underlined portions are the substrings that are chosen from s and t.

__Constraints:__

- `1 <= s.length, t.length <= 100`
- `s` and `t` consist of lowercase English letters only.

__题目描述:__

给你两个字符串 `s` 和 `t` ，请你找出 `s` 中的非空子串的数目，这些子串满足替换 __一个不同字符__ 以后，是 `t` 串的子串。换言之，请你找到 `s` 和 `t` 串中 __恰好__ 只有一个字符不同的子字符串对的数目。

比方说， `"computer"` and `"computation"` 只有一个字符不同: `'e'`/ `'a'` ，所以这一对子字符串会给答案加 1 。

请你返回满足上述条件的不同子字符串对数目。

一个 子字符串 是一个字符串中连续的字符。

__示例:__

示例 1：

```text
输入：s = "aba", t = "baba"
输出：6
解释：以下为只相差 1 个字符的 s 和 t 串的子字符串对：
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
("aba", "baba")
加粗部分分别表示 s 和 t 串选出来的子字符串。
```

输入：s = "ab", t = "bb"
输出：3
解释：以下为只相差 1 个字符的 s 和 t 串的子字符串对：
("ab", "bb")
("ab", "bb")
("ab", "bb")
加粗部分分别表示 s 和 t 串选出来的子字符串。

输入：s = "a", t = "a"
输出：0

示例 4：

```text
输入：s = "abe", t = "bbc"
输出：10
```

__提示：__

- `1 <= s.length, t.length <= 100`
- `s` 和 `t` 都只包含小写英文字母。

__思路:__

```text
1. 枚举
遍历 s 和 t 串
如果 s[i + k] != t[j + k] 此时 diff 自增 1
如果 diff == 1, 则 s[i:i + k] 和 t[j:j + k] 刚好有一个字符不想等
如果 diff > 1, 后面都不需要考虑, 直接跳出循环
时间复杂度为 O(MN * min(M, N)), 空间复杂度为 O(1)
2. 动态规划
字符串相等的可以考虑使用最长公共子串的动态规划算法
设 dpl[i][j] 表示以 s[i] 和 t[j] 结尾的最长相同子串的长度
dpr[i][j] 表示以 s[i] 和 t[j] 开头的最长相同子串的长度
则 s[i] == t[j], dpl[i][j] = dpl[i - 1][j - 1] + 1, dpr[i][j] = dpr[i + 1][j + 1] + 1
否则 dpl[i][j] = dpr[i][j] = 0
遍历 s 和 t, 如果 s[i] != t[j], 则两边根据乘法原理有 (1 + dpl[i][j]) * (1 + dpr[i + 1][j + 1]) 个子串
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSubstrings(string s, string t) 
    {
        int m = s.size(), n = t.size(), result = 0;
        vector<vector<int>> dpl(m + 1, vector<int>(n + 1)), dpr(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) dpl[i + 1][j + 1] = s[i] == t[j] ? (dpl[i][j] + 1) : 0;
        for (int i = m - 1; i >= 0; i--) for (int j = n - 1; j >= 0; j--) dpr[i][j] = s[i] == t[j] ? (dpr[i + 1][j + 1] + 1) : 0;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (s[i] != t[j]) result += (dpl[i][j] + 1) * (dpr[i + 1][j + 1] + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countSubstrings(String s, String t) {
        int result = 0, m = s.length(), n = t.length();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int diff = 0, k = 0;
                while (i + k < m && j + k < n && diff < 2) {
                    diff += s.charAt(i + k) != t.charAt(j + k++) ? 1 : 0;
                    result += diff == 1 ? 1 : 0;
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubstrings(self, s: str, t: str) -> int:
        return sum(sum(sum(s[i + p] != t[j + p] for p in range(k)) == 1 for k in range(min(len(s) - i, len(t) - j) + 1)) for i in range(len(s)) for j in range(len(t)))
```
