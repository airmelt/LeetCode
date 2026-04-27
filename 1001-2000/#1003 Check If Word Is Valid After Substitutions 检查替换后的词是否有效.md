# 1003 Check If Word Is Valid After Substitutions 检查替换后的词是否有效

__Description__:
Given a string s, determine if it is valid.

A string s is valid if, starting with an empty string t = "", you can transform t into s after performing the following operation any number of times:

Insert string "abc" into any position in t. More formally, t becomes tleft + "abc" + tright, where t == tleft + tright. Note that tleft and tright may be empty.
Return true if s is a valid string, otherwise, return false.

__Example:__

Example 1:

Input: s = "aabcbc"
Output: true
Explanation:
"" -> "abc" -> "aabcbc"
Thus, "aabcbc" is valid.

Example 2:

Input: s = "abcabcababcc"
Output: true
Explanation:
"" -> "abc" -> "abcabc" -> "abcabcabc" -> "abcabcababcc"
Thus, "abcabcababcc" is valid.

Example 3:

Input: s = "abccba"
Output: false
Explanation: It is impossible to get "abccba" using the operation.

__Constraints:__

1 <= s.length <= 2 * 10^4
s consists of letters 'a', 'b', and 'c'

__题目描述__:
给你一个字符串 s ，请你判断它是否 有效 。
字符串 s 有效 需要满足：假设开始有一个空字符串 t = "" ，你可以执行 任意次 下述操作将 t 转换为 s ：

将字符串 "abc" 插入到 t 中的任意位置。形式上，t 变为 tleft + "abc" + tright，其中 t == tleft + tright 。注意，tleft 和 tright 可能为 空 。
如果字符串 s 有效，则返回 true；否则，返回 false。

__示例 :__

示例 1：

输入：s = "aabcbc"
输出：true
解释：
"" -> "abc" -> "aabcbc"
因此，"aabcbc" 有效。

示例 2：

输入：s = "abcabcababcc"
输出：true
解释：
"" -> "abc" -> "abcabc" -> "abcabcabc" -> "abcabcababcc"
因此，"abcabcababcc" 有效。

示例 3：

输入：s = "abccba"
输出：false
解释：执行操作无法得到 "abccba" 。

示例 4：

输入：s = "cababc"
输出：false
解释：执行操作无法得到 "cababc" 。

__提示:__

1 <= s.length <= 2 * 10^4
s 由字母 'a'、'b' 和 'c' 组成

__思路__:

栈
和括号匹配比较类似
如果 'a' 直接压入栈
如果 'b' 则需要栈顶为 'a'
如果 'c' 则需要栈顶为 'b'
并且如果栈顶为 'c' 将 'abc' 弹出
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isValid(string s) 
    {
        stack<char> stk;
        for (const auto& c : s)
        {
            if (c == 'b' and (stk.empty() or stk.top() != 'a')) return false;
            else if (c == 'c' and (stk.empty() or stk.top() != 'b')) return false;
            stk.push(c);
            if (c == 'c') 
            {
                stk.pop();
                stk.pop();
                stk.pop();
            }
        }
        return stk.empty();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (char c : s.toCharArray()) {
            if (c == 'b' && (stack.isEmpty() || stack.peek() != 'a')) return false;
            else if (c == 'c' && (stack.isEmpty() || stack.peek() != 'b')) return false;
            stack.push(c);
            if (c == 'c') {
                stack.pop();
                stack.pop();
                stack.pop();
            }
        }
        return stack.isEmpty();
    }
}
```

__Python__:

```Python
class Solution:
    def isValid(self, s: str) -> bool:
        while 'abc' in s:
            s = s.replace('abc', '')
        return not s
```
