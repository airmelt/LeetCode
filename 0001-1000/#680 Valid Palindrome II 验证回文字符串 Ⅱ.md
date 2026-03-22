# 680 Valid Palindrome II 验证回文字符串 Ⅱ

__Description__:
Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

__Example:__

Example 1:

Input: "aba"
Output: True

Example 2:

Input: "abca"
Output: True
Explanation: You could delete the character 'c'.

__Note:__

The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

__题目描述__:
给定一个非空字符串 s，最多删除一个字符。判断是否能成为回文字符串。

__示例 :__

示例 1:

输入: "aba"
输出: True
示例 2:

输入: "abca"
输出: True
解释: 你可以删除c字符。

__注意:__

字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。

__思路__:

判断回文用双指针, 如果找到第一次不一样的字符, 可以左指针加一个字符或者右指针减一个字符再判断一次
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool validPalindrome(string s) 
    {
        for (int i = 0, j = s.size() - 1; i < j; i++, j--) if (s[i] != s[j]) return isPalindrome(s, i + 1, j) or isPalindrome(s, i, j - 1);
        return true;
    }
private:
    bool isPalindrome(string s, int i, int j) 
    {
        while (i < j) 
        {
            if (s[i] != s[j]) return false;
            i++;
            j--;
        }  
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validPalindrome(String s) {
        for (int i = 0, j = s.length() - 1; i < j; i++, j--) if (s.charAt(i) != s.charAt(j)) return isPalindrome(s, i + 1, j) || isPalindrome(s, i, j - 1);
        return true;
    }
    
    private boolean isPalindrome(String s, int i, int j) {
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) return false;
            i++;
            j--;
        }  
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        if s == s[::-1]:
            return True
        l, r = 0, len(s) - 1
        while l < r:
            if s[l] == s[r]:
                l, r = l + 1, r - 1
            else:
                a, b = s[l + 1:r + 1], s[l:r]
                return a == a[::-1] or b == b[::-1]
```
