__Description__:
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

__Example:__
Example 1:

Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"

Example 2:

Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"

__题目描述__:
给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

__示例 :__
示例 1:

输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"

示例 2:

输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"

__思路__:
1. 暴力法, 对每一个位置都查找一遍最大的有效括号的长度
时间复杂度O(n ^ 2), 空间复杂度O(n)
2. 动态规划, 从方法 1中可以看出存在多个重复的子问题, 可以进行优化
时间复杂度O(n), 空间复杂度O(n)
3. 参考[LeetCode #22 Generate Parentheses 括号生成](https://www.jianshu.com/p/373e30c75959), 用两个指针记录 left和 right, 显然 left == right时能得到一个有效的括号, right > left时将指针计数清零
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int longestValidParentheses(string s) 
    {
        int left = 0, right = 0, result = 0;
        for (auto c : s)
        {
            if (c == '(') ++left;
            else if (c == ')') ++right;
            if (left == right) result = max(result, right << 1);
            else if (right > left) left = right = 0;
        }
        left = right = 0;
        reverse(s.begin(), s.end());
        for (auto c : s)
        {
            if (c == '(') ++left;
            else if (c == ')') ++right;
            if (left == right) result = max(result, left << 1);
            else if (left > right) left = right = 0;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int longestValidParentheses(String s) {
        int left = 0, right = 0, result = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') ++left;
            else if (c == ')') ++right;
            if (left == right) result = Math.max(result, right << 1);
            else if (right > left) left = right = 0;
        }
        left = right = 0;
        for (int i = s.length() - 1; i > -1; i--) {
            if (s.charAt(i) == '(') ++left;
            else if (s.charAt(i) == ')') ++right;
            if (left == right) result = Math.max(result, left << 1);
            else if (left > right) left = right = 0;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        left = right = result = 0
        for i in s:
            if i == '(':
                left += 1
            else:
                right += 1
            if left == right:
                result = max(result, right << 1)
            elif left < right:
                left = right = 0
        left, right = 0, 0
        for i in s[::-1]:
            if i == '(':
                left += 1
            else:
                right += 1
            if left == right:
                result = max(result, left << 1)
            elif left > right:
                left = right = 0
        return result
```