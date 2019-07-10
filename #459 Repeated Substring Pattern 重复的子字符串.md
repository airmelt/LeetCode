__Description__:
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

__Example:__
Example 1:

Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.
Example 2:

Input: "aba"
Output: False
Example 3:

Input: "abcabcabcabc"
Output: True
Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)

__题目描述__:
给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

__示例:__
示例 1:

输入: "abab"

输出: True

解释: 可由子字符串 "ab" 重复两次构成。
示例 2:

输入: "aba"

输出: False
示例 3:

输入: "abcabcabcabc"

输出: True

解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)

__思路__:
1. 类似找素数的思路, 从 1个字符开始匹配到 1/2个长度的字符
时间复杂度O(n ^ 2), 空间复杂度O(n)
2. 考虑到 s是重复串, s + s也是重复串, 破坏 s + s的头尾两个字符, 剩下的如果包含 s, 则说明 s中有重复的子字符串
e.g.
- s = "aba" s + s = "abaaba" -> "baab" -> false
- s = "abab" s + s = "abababab" -> "babababa" -> true
时间复杂度O(n), 空间复杂度O(n)
3. 正则表达式, 将要匹配的子字符串当作一个组, 匹配到结尾, 这个组需要出现次数大于 1
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).substr(1, s.size() * 2 - 2).find(s) != -1;
    }
};
```

__Java__:
```
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        return (s + s).substring(1, s.length() * 2 - 1).indexOf(s) != -1;
    }
}
```

__Python__:
```
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        return re.match(r"([a-z]+)\1+$", s) != None
```
