# 2565 Subsequence With the Minimum Score 最少得分子序列

__Description:__

You are given two strings `s` and `t`.

You are allowed to remove any number of characters from the string `t`.

The score of the string is `0` if no characters are removed from the string `t`, otherwise:

- Let `left` be the minimum index among all removed characters.
- Let `right` be the maximum index among all removed characters.

Then the score of the string is `right - left + 1`.

Return _the minimum possible score to make_ `t` _a subsequence of_ `s`_._

A __subsequence__ of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

__Example:__

Example 1:

```text
Input: s = "abacaba", t = "bzaa"
Output: 1
Explanation: In this example, we remove the character "z" at index 1 (0-indexed).
The string t becomes "baa" which is a subsequence of the string "abacaba" and the score is 1 - 1 + 1 = 1.
It can be proven that 1 is the minimum score that we can achieve.
```

Example 2:

```text
Input: s = "cde", t = "xyz"
Output: 3
Explanation: In this example, we remove characters "x", "y" and "z" at indices 0, 1, and 2 (0-indexed).
The string t becomes "" which is a subsequence of the string "cde" and the score is 2 - 0 + 1 = 3.
It can be proven that 3 is the minimum score that we can achieve.
```

__Constraints:__

- `1 <= s.length, t.length <= 10 ^ 5`
- `s` and `t` consist of only lowercase English letters.

__题目描述:__

给你两个字符串 `s` 和 `t` 。

你可以从字符串 `t` 中删除任意数目的字符。

如果没有从字符串 `t` 中删除字符，那么得分为 `0` ，否则:

- 令 `left` 为删除字符中的最小下标。
- 令 `right` 为删除字符中的最大下标。

字符串的得分为 `right - left + 1` 。

请你返回使 `t` 成为 `s` 子序列的最小得分。

一个字符串的 __子序列__ 是从原字符串中删除一些字符后（也可以一个也不删除），剩余字符不改变顺序得到的字符串。（比方说 `"ace"` 是 `"abcde"` 的子序列，但是 `"aec"` 不是）。

__示例:__

示例 1：

```text
输入：s = "abacaba", t = "bzaa"
输出：1
解释：这个例子中，我们删除下标 1 处的字符 "z" （下标从 0 开始）。
字符串 t 变为 "baa" ，它是字符串 "abacaba" 的子序列，得分为 1 - 1 + 1 = 1 。
1 是能得到的最小得分。
```

示例 2：

```text
输入：s = "cde", t = "xyz"
输出：3
解释：这个例子中，我们将下标为 0， 1 和 2 处的字符 "x" ，"y" 和 "z" 删除（下标从 0 开始）。
字符串变成 "" ，它是字符串 "cde" 的子序列，得分为 2 - 0 + 1 = 3 。
3 是能得到的最小得分。
```

__提示：__

- `1 <= s.length, t.length <= 10 ^ 5`
- `s` 和 `t` 都只包含小写英文字母。

__思路:__

```text
前后缀分解
如果字符 j 位于 [left, right] 之间, 删除不会影响结果
所以应该删除一个 t 的子字符串
记 suf[i] 为 s[i:] 中 t 的后缀子序列的起始位置
从后向前遍历 s, 若 s[i] == t[j] 则 j--
如果 j < 0, 说明 t 已经是 s 的子序列, 返回 0
否则, 记录下 j + 1 的位置, 即为 suf[i] 的值
然后从前向后遍历 s, 若 s[i] == t[j] 则 j++
此时 j 即为 s[:i] 中 t 的前缀子序列的长度
将结果更新为 min(suf[i + 1] - j)
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumScore(string s, string t) 
    {
        int m = s.size(), n = t.size();
        vector<int> suf(m + 1, n);
        for (int i = m - 1, j = n - 1; i > -1; i--) 
        {
            if ((j -= s[i] == t[j]) < 0) return 0;
            suf[i] = j + 1;
        }
        int result = suf.front();
        for (int i = 0, j = 0; i < m; i++) if (s[i] == t[j]) result = min(result, suf[i + 1] - (++j));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumScore(String s, String t) {
        int m = s.length(), n = t.length(), suf[] = new int[m + 1];
        suf[m] = n;
        for (int i = m - 1, j = n - 1; i > -1; i--) {
            if (s.charAt(i) == t.charAt(j)) --j;
            if (j < 0) return 0;
            suf[i] = j + 1;
        }
        int result = suf[0];
        for (int i = 0, j = 0; i < m; i++) if (s.charAt(i) == t.charAt(j)) result = Math.min(result, suf[i + 1] - (++j));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumScore(self, s: str, t: str) -> int:
        suf, j = [n := len(t)] * ((m := len(s)) + 1), n - 1
        for i in range(m - 1, -1, -1):
            if (j := j - (s[i] == t[j])) < 0:
                return 0
            suf[i] = j + 1
        result, j = suf[0], 0
        for i, c in enumerate(s):
            if c == t[j]:
                result = min(result, suf[i + 1] - (j := j + 1))
        return result
```
