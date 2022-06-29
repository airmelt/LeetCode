# 1163 Last Substring in Lexicographical Order 按字典序排在最后的子串

__Description__:
Given a string s, return the last substring of s in lexicographical order.

__Example:__

Example 1:

Input: s = "abab"
Output: "bab"
Explanation: The substrings are ["a", "ab", "aba", "abab", "b", "ba", "bab"]. The lexicographically maximum substring is "bab".

Example 2:

Input: s = "leetcode"
Output: "tcode"

__Constraints:__

1 <= s.length <= 4 * 10^5
s contains only lowercase English letters.

__题目描述__:
给你一个字符串 s ，找出它的所有子串并按字典序排列，返回排在最后的那个子串。

__示例 :__

示例 1：

输入：s = "abab"
输出："bab"
解释：我们可以找出 7 个子串 ["a", "ab", "aba", "abab", "b", "ba", "bab"]。按字典序排在最后的子串是 "bab"。

示例 2：

输入：s = "leetcode"
输出："tcode"

__提示:__

1 <= s.length <= 4 * 10^5
s 仅含有小写英文字符。

__思路__:

后缀数组
最大子字符串一定是以最大字符开头的直到字符串结尾的子串
先从后往前找到最大字符第一次出现的位置
然后找到每一个最大字符判断是否需要更新
更新可以直接比较两个待选子串的大小
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string lastSubstring(string s) 
    {
        int n = s.size(), pos = n - 1, max_value = s.back();
        for (int i = n - 2; i > -1; i--) if (s[i] >= max_value) pos = i, max_value = s[i];
        int start = pos + 1, left, right;
        while (start < n) 
        {
            if (s[start] != max_value) ++start;
            else 
            {
                left = pos + 1;
                right = start + 1;
                while (left < start and right < n) 
                {
                    if (s[left] == s[right])
                    {
                        ++left;
                        ++right;
                    }
                    else 
                    {
                        if (s[left] < s[right]) pos = start;
                        start = right;
                        break;
                    } 
                }
                if (left == start or right == n) start = right;
            }
        }
        return s.substr(pos);
    }
};
```

__Java__:

```Java
class Solution {
    public String lastSubstring(String s) {
        int n = s.length(), pos = n - 1, max = s.charAt(pos);
        for (int i = n - 2; i > -1; i--) {
            if (s.charAt(i) >= max) {
                pos = i;
                max = s.charAt(i);
            }
        }
        int start = pos + 1, left = 0, right = 0;
        while (start < n) {
            if (s.charAt(start) != max) ++start;
            else {
                left = pos + 1;
                right = start + 1;
                while (left < start && right < n) {
                    if (s.charAt(left) == s.charAt(right)) {
                        ++left; 
                        ++right;
                    } else {
                        if (s.charAt(left) < s.charAt(right)) pos = start;
                        start = right;
                        break;
                    } 
                }
                if (left == start || right == n) start = right;
            }
        }
        return s.substring(pos);
    }
}
```

__Python__:

```Python
class Solution:
    def lastSubstring(self, s: str) -> str:
        n, result = len(s), ""
        for i in range(n):
            result = max(result, s[i:])
        return result
```
