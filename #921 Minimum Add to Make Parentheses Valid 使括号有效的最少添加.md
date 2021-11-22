# 921 Minimum Add to Make Parentheses Valid 使括号有效的最少添加

__Description__:
A parentheses string is valid if and only if:

It is the empty string,
It can be written as AB (A concatenated with B), where A and B are valid strings, or
It can be written as (A), where A is a valid string.
You are given a parentheses string s. In one move, you can insert a parenthesis at any position of the string.

For example, if s = "()))", you can insert an opening parenthesis to be "(()))" or a closing parenthesis to be "())))".
Return the minimum number of moves required to make s valid.

__Example:__

Example 1:

Input: s = "())"
Output: 1

Example 2:

Input: s = "((("
Output: 3

Example 3:

Input: s = "()"
Output: 0

Example 4:

Input: s = "()))(("
Output: 4

__Constraints:__

1 <= s.length <= 1000
s[i] is either '(' or ')'.

__题目描述__:
给定一个由 '(' 和 ')' 括号组成的字符串 S，我们需要添加最少的括号（ '(' 或是 ')'，可以在任何位置），以使得到的括号字符串有效。

从形式上讲，只有满足下面几点之一，括号字符串才是有效的：

它是一个空字符串，或者
它可以被写成 AB （A 与 B 连接）, 其中 A 和 B 都是有效字符串，或者
它可以被写作 (A)，其中 A 是有效字符串。
给定一个括号字符串，返回为使结果字符串有效而必须添加的最少括号数。

__示例 :__

示例 1：

输入："())"
输出：1

示例 2：

输入："((("
输出：3

示例 3：

输入："()"
输出：0

示例 4：

输入："()))(("
输出：4

__提示:__

S.length <= 1000
S 只包含 '(' 和 ')' 字符。

__思路__:

双指针
用两个指针分别记录左括号和需要的右括号的数量
遍历字符串, 遇到左括号 ++left
否则如果左括号有多余的 --left
否则右括号不够 ++right
最后返回 left + right, 分别是多余的左括号和缺少的右括号
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minAddToMakeValid(string s) 
    {
        int left = 0, right = 0;
        for (const auto& c : s) 
        {
            if (c == '(') ++left;
            else 
            {
                if (left > 0) --left;
                else ++right;
            }
        }
        return right + left;
    }
};
```

__Java__:

```Java
class Solution {
    public int minAddToMakeValid(String s) {
        int left = 0, right = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') ++left;
            else {
                if (left > 0) --left;
                else ++right;
            }
        }
        return right + left;
    }
}
```

__Python__:

```Python
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        while '()' in s:
            s = s.replace('()', '')
        return len(s)
```
