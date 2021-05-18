# 647 Palindromic Substrings 回文子串

__Description__:
Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

__Example:__

Example 1:

Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".

Example 2:

Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

__Constraints:__

1 <= s.length <= 1000
s consists of lowercase English letters.

__题目描述__:
给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

__示例 :__

示例 1：

输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"

示例 2：

输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"

__提示:__

输入的字符串长度不会超过 1000 。

__思路__:

中心扩展法
以 i 或者 [i, i + 1] 为中心向两端扩展直到不是回文串
用一个全局变量记录回文串的数量
时间复杂度 O(n ^ 2), 空间复杂度 O(1)
用马拉车算法可以将时间复杂度降至 O(n), 此时空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countSubstrings(string s) 
    {
        int m = 0, r = 0, result = 0;
        string t = "$#";
        for (const char &c: s) 
        {
            t += c;
            t += '#';
        }
        int n = t.size();
        t += '!';
        vector<int> dp(n);
        for (int i = 1; i < n; i++) 
        {
            dp[i] = (i <= r) ? min(r - i + 1, dp[2 * m - i]) : 1;
            while (t[i + dp[i]] == t[i - dp[i]]) ++dp[i];
            if (i + dp[i] - 1 > r) 
            {
                m = i;
                r = i + dp[i] - 1;
            }
            result += (dp[i] >> 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int result = 0;
    
    public int countSubstrings(String s) {
        for (int i = 0; i < s.length(); i++) {
            count(s, i, i);
            count(s, i, i + 1);
        }
        return result;
    }
    
    private void count(String s, int start, int end) {
        while (start > -1 && end < s.length() && s.charAt(start--) == s.charAt(end++)) ++result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubstrings(self, s: str) -> int:
        result, n = 0, len(s)
        
        def count(start: int, end: int):
            nonlocal result
            while start > -1 and end < n and s[start] == s[end]:
                start -= 1
                end += 1
                result += 1
                
        for i in range(n):
            count(i, i)
            count(i, i + 1)
        return result
```
