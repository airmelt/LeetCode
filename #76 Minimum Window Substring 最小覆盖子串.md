# 76 Minimum Window Substring 最小覆盖子串

__Description__:
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

__Example:__

Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"

__Note:__

If there is no such window in S that covers all characters in T, return the empty string "".
If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

__题目描述__:
给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。

__示例 :__

输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"

__说明：__

如果 S 中不存这样的子串，则返回空字符串 ""。
如果 S 中存在这样的子串，我们保证它是唯一的答案。

__思路__:

滑动窗口法
用两个指针分别指向滑动窗口的两边
当窗口内的字符数不够时移动右指针
当窗口内的字符刚好等于需要的数量时移动左指针且更新最小长度
输出 s的子字符串
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string minWindow(string s, string t) 
    {
        int left = 0, right = 0, start = 0, len = INT_MAX, need[128]{0}, window[128]{0}, match = 0, window_size = 0, n = s.size();
        for (auto &c : t) ++need[c];
        for (auto &i : need) if (i > 0) ++window_size;
        while (right < n)
        {
            char c = s[right++];
            if (need[c] > 0)
            {
                ++window[c];
                if (window[c] == need[c]) ++match;
            }
            while (match == window_size)
            {
                if (right - left < len)
                {
                    start = left;
                    len = right - left;
                }
                char d = s[left++];
                if (need[d] > 0)
                {
                    if (window[d] == need[d]) --match;
                    --window[d];
                }
            }
        }
        return len == INT_MAX ? "" : s.substr(start, len);
    }
};
```

__Java__:

```Java
class Solution {
    public String minWindow(String s, String t) {
        int left = 0, right = 0, start = 0, len = Integer.MAX_VALUE, need[] = new int[128], window[] = new int[128], match = 0, windowSize = 0, n = s.length();
        for (char c : t.toCharArray()) ++need[c];
        for (int i : need) if (i > 0) ++windowSize;
        while (right < n) {
            char c = s.charAt(right++);
            if (need[c] > 0) {
                ++window[c];
                if (window[c] == need[c]) ++match;
            }
            while (match == windowSize) {
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }
                char d = s.charAt(left++);
                if (need[d] > 0) {
                    if (window[d] == need[d]) --match;
                    --window[d];
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
    }
}
```

__Python__:

```Python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        result, length, left, right, window, t = '', float('Inf'), 0, 0, Counter(), Counter(t)
        while right < len(s):
            window[s[right]] += 1
            right += 1
            while all(map(lambda x: window[x] >= t[x], t.keys())):
                if right - left < length:
                    result, length = s[left:right], right - left
                window[s[left]] -= 1
                left += 1
        return result
```
