# 1208 Get Equal Substrings Within Budget 尽可能使字符串相等

__Description:__

You are given two strings s and t of the same length and an integer maxCost.

You want to change s to t. Changing the ith character of s to ith character of t costs |s[i] - t[i]| (i.e., the absolute difference between the ASCII values of the characters).

Return the maximum length of a substring of s that can be changed to be the same as the corresponding substring of t with a cost less than or equal to maxCost. If there is no substring from s that can be changed to its corresponding substring from t, return 0.

__Example:__

Example 1:

Input: s = "abcd", t = "bcdf", maxCost = 3
Output: 3
Explanation: "abc" of s can change to "bcd".
That costs 3, so the maximum length is 3.

Example 2:

Input: s = "abcd", t = "cdef", maxCost = 3
Output: 1
Explanation: Each character in s costs 2 to change to character in t,  so the maximum length is 1.

Example 3:

Input: s = "abcd", t = "acde", maxCost = 0
Output: 1
Explanation: You cannot make any change, so the maximum length is 1.

__Constraints:__

1 <= s.length <= 10^5
t.length == s.length
0 <= maxCost <= 10^6
s and t consist of only lowercase English letters.

__题目描述：__

给你两个长度相同的字符串，s 和 t。

将 s 中的第 i 个字符变到t中的第 i 个字符需要 |s[i] - t[i]| 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。

用于变更字符串的最大预算是 maxCost。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。

如果你可以将 s 的子字符串转化为它在 t 中对应的子字符串，则返回可以转化的最大长度。

如果 s 中没有子字符串可以转化成 t 中对应的子字符串，则返回 0。

__示例：__

示例 1：

输入：s = "abcd", t = "bcdf", maxCost = 3
输出：3
解释：s 中的 "abc" 可以变为 "bcd"。开销为 3，所以最大长度为 3。

示例 2：

输入：s = "abcd", t = "cdef", maxCost = 3
输出：1
解释：s 中的任一字符要想变成 t 中对应的字符，其开销都是 2。因此，最大长度为 1。

示例 3：

输入：s = "abcd", t = "acde", maxCost = 0
输出：1
解释：a -> a, cost = 0，字符串未发生变化，所以最大长度为 1。

__提示：__

1 <= s.length, t.length <= 10^5
0 <= maxCost <= 10^6
s 和t都只含小写英文字母。

__思路：__

滑动窗口
一直向右滑动
因为需要滑动窗口的最大值, 所以只有在当前所用的值大于给定值时, 才收缩窗口
最后返回 n - left
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution
{
public:
    int equalSubstring(string s, string t, int maxCost)
    {
        int left = 0, right = 0, used = 0, n = s.length();
        while (right < n)
        {
            used += abs(t[right] - s[right++]);
            if (used > maxCost) used -= abs(t[left] - s[left++]);
        }
        return n - left;
    }
};
```

__Java__:

```Java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int left = 0, right = 0, used = 0, n = s.length();
        while (right < n) {
            used += Math.abs(t.charAt(right) - s.charAt(right++));
            if (used > maxCost) used -= Math.abs(t.charAt(left) - s.charAt(left++));
        }
        return n - left;
    }
}
```

__Python__:

```Python
class Solution:
    def equalSubstring(self, s: str, t: str, maxCost: int) -> int:
        l, r, used = 0, 0, 0
        while r < (n := len(s)):
            used += abs(ord(t[r]) - ord(s[r]))
            r += 1
            if used > maxCost:
                used -= abs(ord(t[l]) - ord(s[l]))
                l += 1
        return n - l
```
