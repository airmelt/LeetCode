# 2730 Find the Longest Semi-Repetitive Substring 找到最长的半重复子字符串

__Description:__

You are given a digit string `s` that consists of digits from 0 to 9.

A string is called __semi-repetitive__ if there is __at most__ one adjacent pair of the same digit. For example, `"0010"`, `"002020"`, `"0123"`, `"2002"`, and `"54944"` are semi-repetitive while the following are not: `"00101022"` (adjacent same digit pairs are 00 and 22), and `"1101234883"` (adjacent same digit pairs are 11 and 88).

Return the length of the __longest semi-repetitive substring__ of `s`.

__Example:__

Example 1:

```text
Input: s = "52233"

Output: 4

Explanation:

The longest semi-repetitive substring is "5223". Picking the whole string "52233" has two adjacent same digit pairs 22 and 33, but at most one is allowed.
```

Example 2:

```text
Input: s = "5494"

Output: 4

Explanation:

`s` is a semi-repetitive string.
```

Example 3:

```text
Input: s = "1111111"

Output: 2

Explanation:

The longest semi-repetitive substring is "11". Picking the substring "111" has two adjacent same digit pairs, but at most one is allowed.
```

__Constraints:__

- `1 <= s.length <= 50`
- `'0' <= s[i] <= '9'`

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，这个字符串只包含 `0` 到 `9` 的数字字符。

如果一个字符串 `t` 中至多有一对相邻字符是相等的，那么称这个字符串 `t` 是 __半重复的__ 。例如， `"0010"` 、 `"002020"` 、 `"0123"` 、 `"2002"` 和 `"54944"` 是半重复字符串，而 `"00101022"` （相邻的相同数字对是 00 和 22）和 `"1101234883"` （相邻的相同数字对是 11 和 88）不是半重复字符串。

请你返回 `s` 中最长 __半重复__ 子字符串 的长度。

__示例:__

示例 1：

```text
输入：s = "52233"

输出：4

解释：

最长的半重复子字符串是 "5223"。整个字符串 "52233" 有两个相邻的相同数字对 22 和 33，但最多只能选取一个。
```

示例 2：

```text
输入：s = "5494"

输出：4

解释：

`s` 是一个半重复字符串。
```

示例 3：

```text
输入：s = "1111111"

输出：2

解释：

最长的半重复子字符串是 "11"。子字符串 "111" 有两个相邻的相同数字对，但最多允许选取一个。
```

__提示：__

- `1 <= s.length <= 50`
- `'0' <= s[i] <= '9'`

__思路:__

```text
滑动窗口
'0' <= s[i] <= '9' 实际上没有什么特殊作用
如果当前元素和前一个元素相等就计数
如果计数超过 1 则说明窗口中已经有 2 对相邻相同数字
那么移动左边的窗口直到把相邻数字移除
重置计数
保留窗口的最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSemiRepetitiveSubstring(string s) 
    {
        int result = 1, cur = 0, left = 0, n = s.size();
        for (int right = 1; right < n; right++) 
        {
            if (s[right] == s[right - 1]) ++cur;
            if (cur > 1) 
            {
                ++left;
                while (s[left] != s[left - 1]) ++left;
                cur = 1;
            }
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSemiRepetitiveSubstring(String s) {
        int result = 1, cur = 0, left = 0, n = s.length();
        for (int right = 1; right < n; right++) {
            if (s.charAt(right) == s.charAt(right - 1)) ++cur;
            if (cur > 1) {
                ++left;
                while (s.charAt(left) != s.charAt(left - 1)) ++left;
                cur = 1;
            }
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSemiRepetitiveSubstring(self, s: str) -> int:
        return max(len(s[i:j]) for i in range(len(s)) for j in range(i + 1, len(s) + 1) if sum(s[i + k] == s[i + k + 1] for k in range(len(s[i:j]) - 1)) < 2)
```
