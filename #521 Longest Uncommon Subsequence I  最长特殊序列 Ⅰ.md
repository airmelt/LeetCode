# 521 Longest Uncommon Subsequence I  最长特殊序列 Ⅰ

__Description__:
Given a group of two strings, you need to find the longest uncommon subsequence of this group of two strings. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be any subsequence of the other strings.

A subsequence is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be two strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return -1.

__Example:__

Example 1:
Input: "aba", "cdc"
Output: 3
Explanation: The longest uncommon subsequence is "aba" (or "cdc"),
because "aba" is a subsequence of "aba",
but not a subsequence of any other strings in the group of two strings.

__Note:__
Both strings' lengths will not exceed 100.
Only letters from a ~ z will appear in input strings.

__题目描述__:
给定两个字符串，你需要从这两个字符串中找出最长的特殊序列。最长特殊序列定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。

子序列可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。

输入为两个字符串，输出最长特殊序列的长度。如果不存在，则返回 -1。

__示例：__

输入: "aba", "cdc"
输出: 3
解析: 最长特殊序列可为 "aba" (或 "cdc")

__说明:__
两个字符串长度均小于100。
字符串中的字符仅含有 'a'~'z'。

__思路__:

两个字符串完全相同就输出 -1, 否则输出较长的字符串长度
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findLUSlength(string a, string b) 
    {
        return (a == b) ? -1 : (a.size() > b.size() ? a.size() : b.size());
    }
};
```

__Java__:

```Java
class Solution {
    public int findLUSlength(String a, String b) {
        return (a.equals(b)) ? -1 : (a.length() > b.length() ? a.length() : b.length());
    }
}
```

__Python__:

```Python
class Solution:
    def findLUSlength(self, a: str, b: str) -> int:
        return -1 if a == b else max(len(a), len(b))
```
