# 678 Valid Parenthesis String 有效的括号字符串

__Description__:
Given a string s containing only three types of characters: '(', ')' and '*', return true if s is valid.

The following rules define a valid string:

Any left parenthesis '(' must have a corresponding right parenthesis ')'.
Any right parenthesis ')' must have a corresponding left parenthesis '('.
Left parenthesis '(' must go before the corresponding right parenthesis ')'.
'*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string "".

__Example:__

Example 1:

Input: s = "()"
Output: true

Example 2:

Input: s = "(*)"
Output: true

Example 3:

Input: s = "(*))"
Output: true

__Constraints:__

1 <= s.length <= 100
s[i] is '(', ')' or '*'.

__题目描述__:
给定一个只包含三种字符的字符串：（ ，） 和 *，写一个函数来检验这个字符串是否为有效字符串。有效字符串具有如下规则：

任何左括号 ( 必须有相应的右括号 )。
任何右括号 ) 必须有相应的左括号 ( 。
左括号 ( 必须在对应的右括号之前 )。
* 可以被视为单个右括号 ) ，或单个左括号 ( ，或一个空字符串。
一个空字符串也被视为有效字符串。

__示例 :__

示例 1:

输入: "()"
输出: True

示例 2:

输入: "(*)"
输出: True

示例 3:

输入: "(*))"
输出: True

__注意:__

字符串大小将在 [1，100] 范围内。

__思路__:

贪心法

1. 分别从前往后和从后往前遍历字符串
设置两个指针 left, right 记录左括号和右括号的数量
每次从前往后遇到左括号 left 自增, 从后往前遇到右括号 right 自增
其余情况 left 和 right 都自减
如果出现 left 和 right 任意一个小于 0, 说明括号数量不够, 直接返回 false
遍历完成返回 true
2. 用两个指针记录多余左括号的上下界
那么每次遇到左括号, left, right 都要自增
每次遇到右括号, left, right 都要自减, 且 left 最多减到 0, right 不能小于 0
每次遇到 '*', 由于 left 是下界, left 自减直到 0, right 是上界, right 自增
也就是说对于 left, 将 '*' 看作 ')' 或者 ' '(空字符), 对于 right, 将 '*' 看作 '('
时间复杂度 O(n), 空间复杂度 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool checkValidString(string s) 
    {
        int left = 0, right = 0, n = s.size();
        for(int i = 0; i < n; ++i)
        {
            left += ((s[i] != ')') << 1) - 1;
            right += ((s[n - 1 - i] != '(') << 1) - 1;
            if (left < 0 or right < 0) return false;
        } 
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkValidString(String s) {
        int left = 0, right = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                ++left;
                ++right;
            } else if (c == ')') {
                left = Math.max(0, left - 1);
                --right;
            } else if (c == '*') {
                left = Math.max(0, left - 1);
                ++right;
            }
            if (right < 0) return false;
        }
        return left == 0;
    }
}
```

__Python__:

```Python
class Solution:
    def checkValidString(self, s: str) -> bool:
        left = right = 0
        for c in s:
            if c == '(':
                left += 1
                right += 1
            elif c == ')':
                left = max(0, left - 1)
                right -= 1
            elif c == '*':
                left = max(0, left - 1)
                right += 1
            if right < 0:
                return False
        return not left
```
