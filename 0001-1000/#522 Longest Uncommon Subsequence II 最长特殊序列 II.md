# 522 Longest Uncommon Subsequence II 最长特殊序列 II

__Description__:
Given a list of strings, you need to find the longest uncommon subsequence among them. The longest uncommon subsequence is defined as the longest subsequence of one of these strings and this subsequence should not be any subsequence of the other strings.

A subsequence is a sequence that can be derived from one sequence by deleting some characters without changing the order of the remaining elements. Trivially, any string is a subsequence of itself and an empty string is a subsequence of any string.

The input will be a list of strings, and the output needs to be the length of the longest uncommon subsequence. If the longest uncommon subsequence doesn't exist, return -1.

__Example:__

Example 1:
Input: "aba", "cdc", "eae"
Output: 3

__Note:__

All the given strings' lengths will not exceed 10.
The length of the given list will be in the range of [2, 50].

__题目描述__:
给定字符串列表，你需要从它们中找出最长的特殊序列。最长特殊序列定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。

子序列可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。

输入将是一个字符串列表，输出是最长特殊序列的长度。如果最长特殊序列不存在，返回 -1 。

__示例 :__

输入: "aba", "cdc", "eae"
输出: 3

__提示:__

所有给定的字符串长度不会超过 10 。
给定字符串列表的长度将在 [2, 50 ] 之间。

__思路__:

按照字符串长度有长到短排序
对每一对字符串查看是否是为子串的关系
时间复杂度O(mn ^ 2), 空间复杂度O(1), 其中 m为字符串的长度, n为字符串的数量

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findLUSlength(vector<string>& strs) 
    {
        sort(strs.begin(), strs.end(), [](const auto &s1, const auto &s2) { return s2.size() < s1.size(); });
        for (int i = 0; i < strs.size(); i++) 
        {
            bool flag = true;
            for (int j = 0; j < strs.size(); j++) 
            {
                if (i == j) continue;
                if (helper(strs[i], strs[j])) 
                {
                    flag = false;
                    break;
                }
            }
            if (flag) return strs[i].size();
        }
        return -1;
    }
private:
    bool helper(string &s1, string &s2) 
    {
        int j = 0;
        for (int i = 0; i < s2.size() and j < s1.size(); i++) if (s1[j] == s2[i]) ++j;
        return j == s1.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int findLUSlength(String[] strs) {
        Arrays.sort(strs, new Comparator <String> () {
            public int compare(String s1, String s2) {
                return s2.length() - s1.length();
            }
        });
        for (int i = 0, j; i < strs.length; i++) {
            boolean flag = true;
            for (j = 0; j < strs.length; j++) {
                if (i == j) continue;
                if (helper(strs[i], strs[j])) {
                    flag = false;
                    break;
                }
            }
            if (flag) return strs[i].length();
        }
        return -1;
    }
    
    public boolean helper(String s1, String s2) {
        int j = 0;
        for (int i = 0; i < s2.length() && j < s1.length(); i++) if (s1.charAt(j) == s2.charAt(i)) ++j;
        return j == s1.length();
    }
}
```

__Python__:

```Python
class Solution:
    def findLUSlength(self, strs: List[str]) -> int:
        strs.sort(key=len, reverse=True)
        def helper(s1: str, s2: str) -> bool:
            j = 0
            for c in s2:
                if s1[j:j + 1] == c:
                    j += 1
            return j == len(s1)
        for i in range(len(strs)):
            flag = True
            for j in range(len(strs)):
                if i == j:
                    continue
                if helper(strs[i], strs[j]):
                    flag = False
                    break
            if flag:
                return len(strs[i])
        return -1
```
