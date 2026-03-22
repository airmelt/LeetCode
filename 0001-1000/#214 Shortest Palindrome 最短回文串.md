# 214 Shortest Palindrome 最短回文串

__Description__:
Given a string s, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

__Example:__

Example 1:

Input: "aacecaaa"
Output: "aaacecaaa"

Example 2:

Input: "abcd"
Output: "dcbabcd"

__题目描述__:
给定一个字符串 s，你可以通过在字符串前面添加字符将其转换为回文串。找到并返回可以用这种方式转换的最短回文串。

__示例 :__

示例 1:

输入: "aacecaaa"
输出: "aaacecaaa"

示例 2:

输入: "abcd"
输出: "dcbabcd"

__思路__:

由于需要在开头添加字符
如果从开头找到最长回文串, 将剩下的子串反转并加到开头, 就是最短回文串

1. 暴力法
反转 s, 查找最长回文串
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. kmp
查找最长回文子串可以用 kmp加速
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string shortestPalindrome(string s) 
    {
        string r(s);
        reverse(r.begin(), r.end());
        string temp = s + "#" + r;
        vector<int> next(temp.size(), 0);
        for (int i = 1; i < temp.size(); i++) 
        {
            int t = next[i - 1];
            while (t > 0 and temp[i] != temp[t]) t = next[t - 1];
            if (temp[i] == temp[t]) ++t;
            next[i] = t;
        }
        return r.substr(0, s.size() - next[temp.size() - 1]) + s;
    }
};
```

__Java__:

```Java
class Solution {
    public String shortestPalindrome(String s) {
        StringBuilder r = new StringBuilder(s).reverse();
        String result = "";
        for (int i = 0; i < s.length(); i++) if (s.substring(0, s.length() - i).equals(r.toString().substring(i))) {
            result = r.toString().substring(0, i) + s;
            break;
        } 
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def shortestPalindrome(self, s: str) -> str:
        r = s[::-1]
        for i in range(len(s) + 1):
            if s.startswith(r[i:]):
                return r[:i] + s
```
