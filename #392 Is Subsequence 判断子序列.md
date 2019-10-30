__Description__:
Given a string s and a string t, check if s is subsequence of t.

You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length ~= 500,000) string, and s is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).

__Example:__
Example 1:
s = "abc", t = "ahbgdc"

Return true.

Example 2:
s = "axc", t = "ahbgdc"

Return false.

__Follow up:__
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

__题目描述__:
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

__示例 :__
示例 1:
s = "abc", t = "ahbgdc"

返回 true.

示例 2:
s = "axc", t = "ahbgdc"

返回 false.

__后续挑战 :__

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

__思路__:
1. 双指针法, 每次找到 s在 t中出现, s的指针向后移动, 直到遍历完两个字符串中的一个, 判断 s是否遍历完即可
2. 查找法, 对每一个在 s中出现的字符, 记录其在 t出现的位置, 从 t的出现位置之后的一个字符继续查找
时间复杂度O(n), 空间复杂度O(1), n为两个字符串的最长长度

__代码__:
__C++__:
```C++
class Solution {
public:
    bool isSubsequence(string s, string t) 
    {
        int i = 0, j = 0;
        while (i < s.size() && j < t.size())
        {
            if (s[i] == t[j]) i++;
            j++;
        }
        return i == s.size();
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int index = -1;
        for (char c : s.toCharArray()) if ((index = t.indexOf(c, index + 1)) == -1) return false;
        return true;
    }
}
```

__Python__:
```Python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        t = iter(t)
        return all((i in t for i in s))
```