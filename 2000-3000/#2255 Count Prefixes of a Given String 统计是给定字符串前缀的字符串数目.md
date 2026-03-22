# 2255 Count Prefixes of a Given String 统计是给定字符串前缀的字符串数目

__Description:__

You are given a string array `words` and a string `s`, where `words[i]` and `s` comprise only of __lowercase English letters__.

Return _the __number of strings__ in_ `words` _that are a __prefix__ of_ `s`.

A prefix of a string is a substring that occurs at the beginning of the string. A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: words = ["a","b","c","ab","bc","abc"], s = "abc"
Output: 3
Explanation:
The strings in words which are a prefix of s = "abc" are:
"a", "ab", and "abc".
Thus the number of strings in words which are a prefix of s is 3.
```

Example 2:

```text
Input: words = ["a","a"], s = "aa"
Output: 2
Explanation:
Both of the strings are a prefix of s. 
Note that the same string can occur multiple times in words, and it should be counted each time.
```

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length, s.length <= 10`
- `words[i]` and `s` consist of lowercase English letters __only__.

__题目描述:__

给你一个字符串数组 `words` 和一个字符串 `s` ，其中 `words[i]` 和 `s` 只包含 __小写英文字母__ 。

请你返回 `words` 中是字符串 `s` __前缀__ 的 __字符串数目__ 。

一个字符串的 前缀 是出现在字符串开头的子字符串。子字符串 是一个字符串中的连续一段字符序列。

__示例:__

示例 1：

```text
输入：words = ["a","b","c","ab","bc","abc"], s = "abc"
输出：3
解释：
words 中是 s = "abc" 前缀的字符串为：
"a" ，"ab" 和 "abc" 。
所以 words 中是字符串 s 前缀的字符串数目为 3 。
```

示例 2：

```text
输入：words = ["a","a"], s = "aa"
输出：2
解释：
两个字符串都是 s 的前缀。
注意，相同的字符串可能在 words 中出现多次，它们应该被计数多次。
```

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length, s.length <= 10`
- `words[i]` 和 `s` __只__ 包含小写英文字母。

__思路:__

```text
模拟
将所有的字符串遍历一遍, 判断是否是 s 的前缀, 如果是则计数
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 M 为 words 的长度, N 为 s 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPrefixes(vector<string>& words, string s) 
    {
        return accumulate(words.begin(), words.end(), 0, [&s](int result, auto& word) { return s.find(word) ? result : ++result; });
    }
};
```

__Java__:

```Java
class Solution {
    public int countPrefixes(String[] words, String s) {
        return (int)Arrays.stream(words).filter(word -> s.startsWith(word)).count();
    }
}
```

__Python__:

```Python
class Solution:
    def countPrefixes(self, words: List[str], s: str) -> int:
        return sum(s.startswith(word) for word in words)
```
