# 1347 Minimum Number of Steps to Make Two Strings Anagram 制造字母异位词的最小步骤数

__Description:__

You are given two strings of the same length s and t. In one step you can choose any character of t and replace it with another character.

Return the minimum number of steps to make t an anagram of s.

An Anagram of a string is a string that contains the same characters with a different (or the same) ordering.

__Example:__

Example 1:

Input: s = "bab", t = "aba"
Output: 1
Explanation: Replace the first 'a' in t with b, t = "bba" which is anagram of s.

Example 2:

Input: s = "leetcode", t = "practice"
Output: 5
Explanation: Replace 'p', 'r', 'a', 'i' and 'c' from t with proper characters to make t anagram of s.

Example 3:

Input: s = "anagram", t = "mangaar"
Output: 0
Explanation: "anagram" and "mangaar" are anagrams.

__Constraints:__

1 <= s.length <= 5 * 10^4
s.length == t.length
s and t consist of lowercase English letters only.

__题目描述：__

给你两个长度相等的字符串 s 和 t。每一个步骤中，你可以选择将 t 中的 任一字符 替换为 另一个字符。

返回使 t 成为 s 的字母异位词的最小步骤数。

字母异位词 指字母相同，但排列不同（也可能相同）的字符串。

__示例：__

示例 1：

输出：s = "bab", t = "aba"
输出：1
提示：用 'b' 替换 t 中的第一个 'a'，t = "bba" 是 s 的一个字母异位词。

示例 2：

输出：s = "leetcode", t = "practice"
输出：5
提示：用合适的字符替换 t 中的 'p', 'r', 'a', 'i' 和 'c'，使 t 变成 s 的字母异位词。

示例 3：

输出：s = "anagram", t = "mangaar"
输出：0
提示："anagram" 和 "mangaar" 本身就是一组字母异位词。

示例 4：

输出：s = "xxyyzz", t = "xxyyzz"
输出：0

示例 5：

输出：s = "friend", t = "family"
输出：4

__提示：__

1 <= s.length <= 50000
s.length == t.length
s 和 t 只包含小写英文字母

__思路：__

模拟
统计 s 和 t 的不同字符的个数
只需要看 s 比 t 多的字符, 这个数量实际上也是 t 比 s 少的字符
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minSteps(string s, string t) 
    {
        vector<int> count(26);
        int result = 0, n = s.size();
        for (int i = 0; i < n; i++)
        {
            ++count[s[i] - 'a'];
            --count[t[i] - 'a'];
        }
        for (const auto& i : count) result += max(0, i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSteps(String s, String t) {
        int count[] = new int[26], result = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            ++count[s.charAt(i) - 'a'];
            --count[t.charAt(i) - 'a'];
        }
        for (int i : count) result += Math.max(0, i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSteps(self, s: str, t: str) -> int:
        return len(list((Counter(s) - Counter(t)).elements()))
```
