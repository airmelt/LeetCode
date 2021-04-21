# 395 Longest Substring with At Least K Repeating Characters 至少有K个重复字符的最长子串

__Description__:
Given a string s and an integer k, return the length of the longest substring of s such that the frequency of each character in this substring is greater than or equal to k.

__Example:__

Example 1:

Input: s = "aaabb", k = 3
Output: 3
Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.

Example 2:

Input: s = "ababbc", k = 2
Output: 5
Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

__Constraints:__

1 <= s.length <= 10^4
s consists of only lowercase English letters.
1 <= k <= 10^5

__题目描述__:
找到给定字符串（由小写字符组成）中的最长子串 T ， 要求 T 中的每一字符出现次数都不少于 k 。输出 T 的长度。

__示例 :__

示例 1:

输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。

示例 2:

输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。

__思路__:

分治
递归的终点为字符串为空或者字符串长度小于 k立即返回 0
递归的检查两边的字符串是否满足大于等于 k这个条件
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestSubstring(string s, int k) 
    {
        if (k <= 1) return s.size();
        if (s.empty() or s.size() < k) return 0;
        vector<int> hash(128, 0);
        for (char c : s) ++hash[c];
        int i = 0;
        while (i < s.size() and hash[s[i]] >= k) ++i;
        if (i == s.size()) return s.size();
        int l = longestSubstring(s.substr(0, i), k);
        while (i < s.size() and hash[s[i]] < k) ++i;
        int r = longestSubstring(s.substr(i), k);
        return max(l, r);
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubstring(String s, int k) {
        if (s == null || s.isEmpty() || s.length() < k) return 0;
        if (k <= 1) return s.length();
        int count[] = new int[128], i = 0;
        for (char c : s.toCharArray()) ++count[c];
        while (i < s.length() && count[s.charAt(i)] >= k) ++i;
        if (i == s.length()) return s.length();
        int l = longestSubstring(s.substring(0, i), k);
        while (i < s.length() && count[s.charAt(i)] < k) ++i;
        int r = longestSubstring(s.substring(i), k);
        return Math.max(l, r);
    }
}
```

__Python__:

```Python
class Solution(object):
    def longestSubstring(self, s, k):
        if not s:
            return 0
        for c in set(s):
            if s.count(c) < k:
                return max(self.longestSubstring(t, k) for t in s.split(c))
        return len(s)
```
