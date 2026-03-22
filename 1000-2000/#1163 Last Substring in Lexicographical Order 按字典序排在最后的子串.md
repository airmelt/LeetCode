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

后缀数组 ➕ 双指针
最大子字符串一定是以最大字符开头的直到字符串结尾的子串
设 left 表示最大字符位置, right 为工作指针, step 为步长
当 s[left] < s[right + step] 时, 包括 step 为 0 的情况, 说明 s[left] 不是最大字符, left 移动到 right + step 处, right 移动到下一个位置继续遍历
当 s[left + step] == s[right + step] 时, step 自增, 直到找到不相等的位置
当 s[left + step] < s[right + step] 时, 注意到 step 为 0 时应该有 s[left] == s[right], 所以 s[right] 应该也是最大字符, left 移动到 right, right 移动到下一个位置
否则 right 自增遍历字符串
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string lastSubstring(string s) 
    {
        int n = s.size(), left = 0, right = 1, step = 0;
        while (right + step < n) 
        {
            if (s[left + step] == s[right + step]) ++step;
            else if (s[left] < s[right + step]) 
            {
                left = right + step;
                right = left + 1;
                step = 0;
            }
            else if (s[left + step] < s[right + step]) 
            {
                left = right++;
                step = 0;
            }
            else
            {
                ++right;
                step = 0;
            }
        }
       return s.substr(left);
    }
};
```

__Java__:

```Java
class Solution {
    public String lastSubstring(String s) {
        int n = s.length(), left = 0, right = left + 1, step = 0;
        while (right + step < n) {
            if (s.charAt(left + step) == s.charAt(right + step)) ++step;
            else if (s.charAt(left) < s.charAt(right + step)) {
                left = right + step;
                right = left + 1;
                step = 0;
            } else if (s.charAt(left + step) < s.charAt(right + step)) {
                left = right++;
                step = 0;
            } else {
                ++right;
                step = 0;
            }
        }
        return s.substring(left);
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
