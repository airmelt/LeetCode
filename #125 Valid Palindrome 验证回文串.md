__Description__:
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

Note: For the purpose of this problem, we define empty string as valid palindrome.

**Example:**
Example 1:
Input: "A man, a plan, a canal: Panama"
Output: true

Example 2:
Input: "race a car"
Output: false

__题目描述__:
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

**示例:**
示例 1:
输入: "A man, a plan, a canal: Panama"
输出: true

示例 2:
输入: "race a car"
输出: false

__思路__:
利用ASCII值比较, 留下小写字母和数字, 大写字母转换为小写字母
双指针法, 设置一对指针分别指向字符串开始和结束位置, 分别移动两个指针比较
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0, j = s.size() - 1;
        while (i < j) {
            // isalnum(char c) 用来判断一个字符是否为英文字母或数字，相当于 isalpha(c) || isdigit(c)
            // isalpha(char c) 用来判断一个字符是否是英文字母，相当于 isupper(c) || islower(c)
            while (i < j && !isalnum(s[i])) i++;
            while (i < j && !isalnum(s[j])) j--;
            // toupper(char c) 如果c为小写英文字母，则返回对应的大写字母；否则返回原来的值。
            // tolower(char c) 如果c为大写英文字母，则返回对应的小写字母；否则返回原来的值。
            if (tolower(s[i++]) != tolower(s[j--])) return false;
        }
        return true;
    }
};
```

__Java__:
```
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null) return true;
        s = s.toLowerCase();
        int i = 0, j = s.length() - 1;
        while (i < j) {
            while (i < j && !isLegal(s.charAt(i))) i++;
            while (i < j && !isLegal(s.charAt(j))) j--;
            if (s.charAt(i++) != s.charAt(j--)) return false;
        }
        return true;
    }

    private boolean isLegal(char c) {
        return (c >= 'a' && c <= 'z') || (c >= '0' && c <= '9');
    }
}
```

__Python__:
```
class Solution:
    def isPalindrome(self, s: str) -> bool:
        return ''.join(filter(str.isalnum, s)).lower() == ''.join(filter(str.isalnum, s)).lower()[::-1]
```
