# 524 Longest Word in Dictionary through Deleting 通过删除字母匹配到字典里最长单词

__Description__:
Given a string s and a string array dictionary, return the longest string in the dictionary that can be formed by deleting some of the given string characters. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

__Example:__

Example 1:

Input: s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]
Output: "apple"

Example 2:

Input: s = "abpcplea", dictionary = ["a","b","c"]
Output: "a"

__Constraints:__

1 <= s.length <= 1000
1 <= dictionary.length <= 1000
1 <= dictionary[i].length <= 1000
s and dictionary[i] consist of lowercase English letters.

__题目描述__:
给定一个字符串和一个字符串字典，找到字典里面最长的字符串，该字符串可以通过删除给定字符串的某些字符来得到。如果答案不止一个，返回长度最长且字典顺序最小的字符串。如果答案不存在，则返回空字符串。

__示例 :__

示例 1:

输入:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

输出:
"apple"

示例 2:

输入:
s = "abpcplea", d = ["a","b","c"]

输出:
"a"

__说明:__

所有输入的字符串只包含小写字母。
字典的大小不会超过 1000。
所有输入的字符串长度不会超过 1000。

__思路__:

1. 先对 dictionary 按照字符串长度降序, 字典序升序排序
然后用双指针遍历 dictionary, 找到第一个满足 s 子序列的就返回
时间复杂度 O(mnlgn), 空间复杂度 O(lgn), m 表示字符串的平均长度, n 表示字符串中字符的数量
2. 不排序直接查找是否有满足题意的字符串
找到之后再比较字符串的长度和字典序
时间复杂度 O(mn), 空间复杂度 O(m), m 表示字符串的平均长度, n 表示字符串中字符的数量

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string findLongestWord(string s, vector<string>& dictionary) 
    {
        string result = "";
        for (auto &word: dictionary) if (is_subsequence(word, s)) if (word.size() > result.size() or (word.size() == result.size() and word < result)) result = word;
        return result;
    }
private:
    bool is_subsequence(string &x, string &y) 
    {
        int j = 0, m = y.size(), n = x.size();
        for (int i = 0; i < m and j < n; i++) if (x[j] == y[i]) ++j;
        return j == n;
    }
};
```

__Java__:

```Java
class Solution {
    public String findLongestWord(String s, List<String> dictionary) {
        String result = "";
        for (String word: dictionary) if (isSubsequence(word, s)) if (word.length() > result.length() || (word.length() == result.length() && word.compareTo(result) < 0)) result = word;
        return result;
    }
    
    private boolean isSubsequence(String x, String y) {
        int j = 0, m = y.length(), n = x.length();
        for (int i = 0; i < m && j < n; i++) if (x.charAt(j) == y.charAt(i)) ++j;
        return j == n;
    }
}
```

__Python__:

```Python
class Solution:
    def findLongestWord(self, s: str, dictionary: List[str]) -> str:
        dictionary.sort(key=lambda x: (-len(x), x))
        for word in dictionary:
            i = 0
            for c in s:
                if c == word[i:i + 1]:
                    i += 1
            if i == len(word):
                return word
        return ""
```
