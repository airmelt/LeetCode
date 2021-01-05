# 242 Valid Anagram 有效的字母异位词

__Description__:
Given two strings s and t , write a function to determine if t is an anagram of s.

**Example :**

Example 1:
Input: s = "anagram", t = "nagaram"
Output: true

Example 2:
Input: s = "rat", t = "car"
Output: false

**Note:**
You may assume the string contains only lowercase alphabets.

__Follow up:__
What if the inputs contain unicode characters? How would you adapt your solution to such case?

__题目描述__:
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

**示例 :**

示例 1:
输入: s = "anagram", t = "nagaram"
输出: true

示例 2:
输入: s = "rat", t = "car"
输出: false

**说明:**
你可以假设字符串只包含小写字母。

**进阶:**
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

__思路__:

维护一个长度为 26的数组, 将 s和 t的值存入数组
如果包含 unicode, 可以维护一个 map, 存入出现次数, 此时空间复杂度为O(n)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isAnagram(string s, string t) 
    {
        int *a = new int[26]();
        if (s.size() != t.size()) return false;
        for (int i = 0; i < s.size(); i++) 
        {
            a[s[i] - 'a']++;
            a[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) if (a[i]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isAnagram(String s, String t) {
        int a[] = new int[26];
        if (s.length() != t.length()) return false;
        for (int i = 0; i < s.length(); i++) {
            a[s.charAt(i) - 'a']++;
            a[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < 26; i++) if (a[i] != 0) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return sorted(s) == sorted(t)
```
