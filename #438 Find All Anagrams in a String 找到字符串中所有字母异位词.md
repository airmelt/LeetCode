# 438 Find All Anagrams in a String 找到字符串中所有字母异位词

__Description__:
Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

__Example:__

Example 1:

Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".

Example 2:

Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

__题目描述__:
给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

__说明：__

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。

__示例：__

示例 1:

输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。

示例 2:

输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。

__思路__:

滑动窗口

1. 固定窗口大小为 p的长度, 每次移动窗口比较大小, 满足要求就加入结果
2. 不固定窗口长度, 每次移动窗口比较 p是是否在窗口中, 满足则比较长度, 长度也符合的加入结果
时间复杂度O(n * m), 空间复杂度O(m), n为字符串 s的长度, m为字符串 p的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findAnagrams(string s, string p) 
    {
        vector<int> result;
        int left = 0, right = 0, match = 0;
        unordered_map<char, int> count, window;
        for (auto c : p) ++count[c];
        while (right < s.size()) 
        {
            char c1 = s[right];
            if (count.count(c1)) 
            {
                window[c1]++;
                if (window[c1] == count[c1]) match++;
            }
            right++;
            while (match == count.size()) 
            {
                if (right - left == p.size()) result.emplace_back(left);
                char c2 = s[left];
                if (count.count(c2)) 
                {
                    window[c2]--;
                    if (window[c2] < count[c2]) match--;
                }
                left++;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();
        if (s.length() < p.length()) return result;
        int[] array_s = new int[26], array_p = new int[26];
        for (int i = 0; i < p.length(); i++) {
            array_p[p.charAt(i) - 'a']++;
            array_s[s.charAt(i) - 'a']++;
        }
        array_s[s.charAt(p.length() - 1) - 'a']--;
        for (int i = p.length() - 1; i < s.length(); i++) {
            array_s[s.charAt(i) - 'a']++;
            if (isSame(array_p, array_s)) result.add(i - p.length() + 1);
            array_s[s.charAt(i - p.length() + 1) - 'a']--;
        }
        return result;
    }

    private boolean isSame(int[] a, int[] b) {
        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i]) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        result = []
        if len(p) > len(s):
            return result
        array_s, array_p = [0] * 26, [0] * 26
        left, right = 0, len(p) - 2
        for i in range(len(p)):
            array_p[ord(p[i]) - ord('a')] += 1
            array_s[ord(s[i]) - ord('a')] += 1
        array_s[ord(s[len(p) - 1]) - ord('a')] -= 1
        while right < len(s) - 1:
            right += 1
            array_s[ord(s[right]) - ord('a')] += 1
            if array_p == array_s:
                result.append(left)
            array_s[ord(s[left]) - ord('a')] -= 1
            left += 1
        return result
```
