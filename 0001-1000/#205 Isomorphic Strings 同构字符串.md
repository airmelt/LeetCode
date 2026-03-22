# 205 Isomorphic Strings 同构字符串

__Description__:
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

**Example:**

Example 1:
Input: s = "egg", t = "add"
Output: true

Example 2:
Input: s = "foo", t = "bar"
Output: false

Example 3:
Input: s = "paper", t = "title"
Output: true

__Note__:
You may assume both s and t have the same length.

__题目描述__:
给定两个字符串 s 和 t，判断它们是否是同构的。

如果 s 中的字符可以被替换得到 t ，那么这两个字符串是同构的。

所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身。

**示例：**

示例 1:
输入: s = "egg", t = "add"
输出: true

示例 2:
输入: s = "foo", t = "bar"
输出: false

示例 3:
输入: s = "paper", t = "title"
输出: true

__说明__:
你可以假设 s 和 t 具有相同的长度。

__思路__:

1. 使用hashMap存放对应字符串
时间复杂度O(n), 空间复杂度O(n)
2. 使用 find()(Java为indexOf())
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isIsomorphic(string s, string t) 
    {
        for (int i = 0; i < s.size(); i++) if (s.find(s[i]) != t.find(t[i])) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        for (int i = 0; i < s.length(); i++) {
            if (s.indexOf(s.charAt(i)) != t.indexOf(t.charAt(i))) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        return all(s.find(s[i]) == t.find(t[i]) for i in range(len(s)))
```
