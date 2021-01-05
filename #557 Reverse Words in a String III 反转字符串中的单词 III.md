# 557 Reverse Words in a String III 反转字符串中的单词 III

__Description__:
Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

__Example:__

Example 1:
Input: "Let's take LeetCode contest"
Output: "s'teL ekat edoCteeL tsetnoc"

__Note:__
In the string, each word is separated by single space and there will not be any extra space in the string.

__题目描述__:
给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

__示例 :__

示例 1:

输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc"
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。

__思路__:

按空格分割开字符串, 依次反转
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string reverseWords(string s) 
    {
        int j = s.find(" ") == s.npos ? s.size() : s.find(" "), i = 0;
        while (j <= s.size()) 
        {
            if (s[j] == ' ' || j == s.size()) 
            {
                reverse(s, i, j - 1);
                i = j + 1;
            }
            j++;
        }
        return s;
    }
private:
    void reverse(string &s, int i, int j) 
    {
        while (i < j) {
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i++] ^= s[j--];
        }
    }
};
```

__Java__:

```Java
class Solution {
    public String reverseWords(String s) {
        String[] temp = s.split(" ");
        for (int i = 0; i < temp.length; i++) temp[i] = new StringBuilder(temp[i]).reverse().toString();
        return String.join(" ", temp);
    }
}
```

__Python__:

```Python
class Solution:
    def reverseWords(self, s: str) -> str:
        return ' '.join([word[::-1] for word in s.split(' ')])
```
