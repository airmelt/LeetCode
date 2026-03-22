# 5 Longest Palindromic Substring 最长回文子串

__Description__:
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

__Example:__

Example 1:

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Example 2:

Input: "cbbd"
Output: "bb"

__题目描述__:
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

__示例 :__

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。

示例 2：

输入: "cbbd"
输出: "bb"

__思路__:

马拉车算法(Manacher's Algorithm)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string longestPalindrome(string s) 
    {
        if (s.size() < 2) return s;
        string t = "$#";
        for (int i = 0; i < s.size(); i++)
        {
            t += s[i];
            t += '#';
        }
        t += '*';
        int mid = 0, right = 0, result_index = 0, temp[t.size()]{0};
        for (int i = 0; i < t.size(); i++) temp[i] = 1;
        for (int i = 1; i < t.size() - 1; i++)
        {
            if (right > i) temp[i] = min(temp[2 * mid - i], right - i);
            while (t[i + temp[i]] == t[i - temp[i]]) ++temp[i];
            if (right < i + temp[i])
            {
                right = i + temp[i];
                mid = i;
            }
            if (temp[i] > temp[result_index]) result_index = i;
        }
        int result_length = temp[result_index] - 1;
        result_index -= temp[result_index];
        result_index /= 2;
        return s.substr(result_index, result_length);
    }
};
```

__Java__:

```Java
class Solution {
    public String longestPalindrome(String s) {
        if (s.length() < 2) return s;
        StringBuilder sb = new StringBuilder("$#");
        for (int i = 0; i < s.length(); i++) {
            sb.append(s.charAt(i));
            sb.append('#');
        }
        sb.append('*');
        int mid = 0, right = 0, resultIndex = 0, temp[] = new int[sb.length()];
        for (int i = 0; i < sb.length(); i++) temp[i] = 1;
        for (int i = 1; i < sb.length() - 1; i++) {
            if (right > i) temp[i] = Math.min(temp[2 * mid - i], right - i);
            while (sb.charAt(i + temp[i]) == sb.charAt(i - temp[i])) ++temp[i];
            if (right < i + temp[i]) {
                right = i + temp[i];
                mid = i;
            }
            if (temp[i] > temp[resultIndex]) resultIndex = i;
        }
        int resultLength = temp[resultIndex] - 1;
        resultIndex -= temp[resultIndex];
        resultIndex /= 2;
        resultLength += resultIndex;
        return s.substring(resultIndex, resultLength);
    }
}
```

__Python__:

```Python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        t = '$#'
        for i in range(len(s)):
            t += s[i]
            t += '#'
        t += '*'
        temp = [1] * len(t)
        mid = 0
        right = 0
        result_index = 0
        for i in range(1, len(t) - 1):
            if right > i:
                temp[i] = min(temp[2 * mid - i], right - i)
            while t[i + temp[i]] == t[i - temp[i]]:
                temp[i] += 1
            if right < i + temp[i]:
                right = i + temp[i]
                mid = i
            if temp[i] > temp[result_index]:
                result_index = i
        result_length = temp[result_index] - 1
        result_index -= temp[result_index]
        result_index //= 2
        result_length += result_index
        return s[result_index:result_length]
```
