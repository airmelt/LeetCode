# 567 Permutation in String 字符串的排列

__Description__:
Given two strings s1 and s2, write a function to return true if s2 contains the permutation of s1. In other words, one of the first string's permutations is the substring of the second string.

__Example:__

Example 1:

Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").

Example 2:

Input:s1= "ab" s2 = "eidboaoo"
Output: False

__Constraints:__

The input strings only contain lower case letters.
The length of both given strings is in range [1, 10,000].

__题目描述__:
给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的 子串 。

__示例 :__

示例 1：

输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").

示例 2：

输入: s1= "ab" s2 = "eidboaoo"
输出: False

__提示:__

输入的字符串只包含小写字母
两个字符串的长度都在 [1, 10,000] 之间

__思路__:

滑动窗口
s1 的长度必须小于或等于 s2 的长度, 否则直接返回 false
初始化记录两个字符串在 [0, len(s1)] 区间的子字符串的字符及个数
然后在 s2[len(s1):len(s2)] 中滑动找到是否有子字符串等于 s1
最后判断一下边界
时间复杂度 O(n), 空间复杂度 O(1), 其中 n 为 s2 的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool checkInclusion(string s1, string s2) 
    {
        if (s2.size() < s1.size()) return false;
        vector<int> c1(26), c2(26);
        for (int i = 0; i < s1.size(); i++)
        {
            ++c1[s1[i] - 'a'];
            ++c2[s2[i] - 'a'];
        }
        for (int i = s1.size(), n = s1.size(); i < s2.size(); i++)
        {
            if (c1 == c2) return true;
            --c2[s2[i - n] - 'a'];
            ++c2[s2[i] - 'a'];
        }
        return c1 == c2;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int c1[] = new int[26], c2[] = new int[26], m = s1.length(), n = s2.length();
        if (n < m) return false;
        for (int i = 0; i < m; i++)
        {
            ++c1[s1.charAt(i) - 'a'];
            ++c2[s2.charAt(i) - 'a'];
        }
        for (int i = m; i < n; i++)
        {
            if (Arrays.equals(c1, c2)) return true;
            --c2[s2.charAt(i - m) - 'a'];
            ++c2[s2.charAt(i) - 'a'];
        }
        return Arrays.equals(c1, c2);
    }
}
```

__Python__:

```Python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        return any(Counter(s2[i:i + len(s1)]) == Counter(s1) for i in range(len(s2) - len(s1) + 1)) if len(s2) >= len(s1) else False
```
