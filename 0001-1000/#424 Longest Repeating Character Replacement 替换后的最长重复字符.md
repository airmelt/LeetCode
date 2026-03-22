# 424 Longest Repeating Character Replacement 替换后的最长重复字符

__Description__:
Given a string s that consists of only uppercase English letters, you can perform at most k operations on that string.

In one operation, you can choose any character of the string and change it to any other uppercase English character.

Find the length of the longest sub-string containing all repeating letters you can get after performing the above operations.

__Note:__
Both the string's length and k will not exceed 104.

__Example:__

Example 1:

Input:
s = "ABAB", k = 2

Output:
4

Explanation:
Replace the two 'A's with two 'B's or vice versa.

Example 2:

Input:
s = "AABABBA", k = 1

Output:
4

Explanation:
Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.

__题目描述__:
给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

__注意:__
字符串长度 和 k 不会超过 10^4。

__示例 :__

示例 1:

输入:
s = "ABAB", k = 2

输出:
4

解释:
用两个'A'替换为两个'B',反之亦然。

示例 2:

输入:
s = "AABABBA", k = 1

输出:
4

解释:
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。

__思路__:

滑动窗口
left和 right表示窗口的范围
用一个数组记录当前窗口中的各个字符出现的次数, 记录出现最多的字符的次数 cur
当窗口长度 ➖ cur > k时, 更新窗口, 将 left自增, 并移除该字符
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int characterReplacement(string s, int k) 
    {
        int left = 0, cur = 0, result = 0, count[26]{0};
        for (int right = 0; right < s.size(); right++)
        {
            ++count[s[right] - 'A'];
            cur = max(cur, count[s[right] - 'A']);
            while (right - left + 1 - cur > k) --count[s[left++] - 'A'];
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int characterReplacement(String s, int k) {
        int count[] = new int[26], left = 0, result = 0, cur = 0;
        for (int right = 0; right < s.length(); right++) {
            ++count[s.charAt(right) - 'A'];
            cur = Math.max(cur, count[s.charAt(right) - 'A']);
            while (right - left + 1 - cur > k) --count[s.charAt(left++) - 'A'];
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        left, result, count, cur = 0, 0, [0] * 26, 0
        for right in range(len(s)):
            count[ord(s[right]) - ord('A')] += 1
            cur = max(cur, count[ord(s[right]) - ord('A')])
            while right - left + 1 - cur > k:
                count[ord(s[left]) - ord('A')] -= 1
                left += 1
            result = max(result, right - left + 1)
        return result
```
