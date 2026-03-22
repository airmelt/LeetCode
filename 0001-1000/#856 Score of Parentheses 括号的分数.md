# 856 Score of Parentheses 括号的分数

__Description__:
Given a balanced parentheses string s, return the score of the string.

The score of a balanced parentheses string is based on the following rule:

"()" has score 1.
AB has score A + B, where A and B are balanced parentheses strings.
(A) has score 2 \* A, where A is a balanced parentheses string.

__Example:__

Example 1:

Input: s = "()"
Output: 1

Example 2:

Input: s = "(())"
Output: 2

Example 3:

Input: s = "()()"
Output: 2

Example 4:

Input: s = "(()(()))"
Output: 6

__Constraints:__

2 <= s.length <= 50
s consists of only '(' and ')'.
s is a balanced parentheses string.

__题目描述__:
给定一个平衡括号字符串 S，按下述规则计算该字符串的分数：

() 得 1 分。
AB 得 A + B 分，其中 A 和 B 是平衡括号字符串。
(A) 得 2 \* A 分，其中 A 是平衡括号字符串。

__示例 :__

示例 1：

输入： "()"
输出： 1

示例 2：

输入： "(())"
输出： 2

示例 3：

输入： "()()"
输出： 2

示例 4：

输入： "(()(()))"
输出： 6

__提示:__

S 是平衡括号字符串，且只含有 ( 和 ) 。
2 <= S.length <= 50

__思路__:

记录深度
每次碰到左括号深度加 1, 否则深度减 1
每次碰到成对的括号, 结果加上 2 ** 深度
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int scoreOfParentheses(string s) 
    {
        int n = s.size(), result = 0, level = 0;
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == '(') ++level;
            else --level;
            if (s[i] == ')' and s[i - 1] == '(') result += (1 << level);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int scoreOfParentheses(String s) {
        int n = s.length(), result = 0, level = 0;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') ++level;
            else --level;
            if (s.charAt(i) == ')' && s.charAt(i - 1) == '(') result += (1 << level);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def scoreOfParentheses(self, s: str) -> int:
        return eval(s.replace(')(',')+(').replace('()', '1').replace('(', '2*('))
```
