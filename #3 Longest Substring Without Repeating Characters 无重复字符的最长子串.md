# 3 Longest Substring Without Repeating Characters 无重复字符的最长子串

__Description__:
Given a string, find the length of the longest substring without repeating characters.

__Example:__

Example 1:

Input: "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Example 2:

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Example 3:

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

__题目描述__:
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

__示例 :__

示例 1:

输入: "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

__思路__:

滑动窗口法, 设置左指针 l = 0, 右指针 r = -1, 移动右指针保证右指针不移出字符串 s, 用一个长度为128(长度为26也可)记录出现过的字符, 如果这个字符串出现过(数组记录不为 0), 则移动左指针减少滑动窗口的长度, 否则移动右指针增加滑动窗口长度, 记录最大的滑动窗口长度即可
时间复杂度O(n), 空间复杂度O(1), ASCII码最多128个需要记录

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lengthOfLongestSubstring(string s) 
    {
        int l = 0, r = -1, result = 0, count[128]{0};
        while (l < s.size()) 
        {
            if (r + 1 < s.size() and !count[s[r + 1]]) ++count[s[++r]];
            else --count[s[l++]];
            result = max(result, r - l + 1);
        }
        return result;
    }
};  
```

__Java__:

```Java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int l = 0, r = -1, result = 0, count[] = new int[128];
        while (l < s.length()) {
            if (r + 1 < s.length() && count[s.charAt(r + 1)] == 0) ++count[s.charAt(++r)];
            else --count[s.charAt(l++)];
            result = Math.max(result, r - l + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        l, r, result, count = 0, -1, 0, [False] * 128
        while l < len(s):
            if r + 1 < len(s) and not count[ord(s[r + 1])]:
                r += 1
                count[ord(s[r])] = True
            else:
                count[ord(s[l])] = False
                l += 1
            result = max(result, r - l + 1)
        return result
```
