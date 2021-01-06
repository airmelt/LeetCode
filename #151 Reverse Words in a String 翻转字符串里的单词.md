# 151 Reverse Words in a String 翻转字符串里的单词

__Description__:
Given an input string, reverse the string word by word.

__Example:__

Example 1:

Input: "the sky is blue"
Output: "blue is sky the"

Example 2:

Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.

Example 3:

Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.

__Note:__

A word is defined as a sequence of non-space characters.
Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
You need to reduce multiple spaces between two words to a single space in the reversed string.

__Follow up:__

For C programmers, try to solve it in-place in O(1) extra space.

__题目描述__:
给定一个字符串，逐个翻转字符串中的每个单词。

__示例 :__

示例 1：

输入: "the sky is blue"
输出: "blue is sky the"

示例 2：

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

示例 3：

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

__说明：__

无空格字符构成一个单词。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

__进阶：__

请选用 C 语言的用户尝试使用 O(1) 额外空间复杂度的原地解法。

__思路__:

参考[LeetCode #189 Rotate Array 旋转数组](https://www.jianshu.com/p/15443e6ea0de)
先将整个字符串反转
再按空格反转每个单词并去掉额外的空格
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string reverseWords(string s) 
    {
        stringstream ss;
        string result = "", temp;
        ss << s;
        while (ss >> temp) result = " " + temp + result;
        if (result.size()) result.erase(result.begin());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split(" +");
        Collections.reverse(Arrays.asList(words));
        return String.join(" ", words);
    }
}
```

__Python__:

```Python
class Solution:
    def reverseWords(self, s: str) -> str:
        return ' '.join(reversed(s.split()))
```
