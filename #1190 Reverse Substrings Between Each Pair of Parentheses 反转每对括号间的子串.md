# 1190 Reverse Substrings Between Each Pair of Parentheses 反转每对括号间的子串

__Description__:
You are given a string s that consists of lower case English letters and brackets.

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should not contain any brackets.

__Example:__

Example 1:

Input: s = "(abcd)"
Output: "dcba"

Example 2:

Input: s = "(u(love)i)"
Output: "iloveu"
Explanation: The substring "love" is reversed first, then the whole string is reversed.

Example 3:

Input: s = "(ed(et(oc))el)"
Output: "leetcode"
Explanation: First, we reverse the substring "oc", then "etco", and finally, the whole string.

__Constraints:__

1 <= s.length <= 2000
s only contains lower case English characters and parentheses.
It is guaranteed that all parentheses are balanced.

__题目描述__:
给出一个字符串 s（仅含有小写英文字母和括号）。

请你按照从括号内到外的顺序，逐层反转每对匹配括号中的字符串，并返回最终的结果。

注意，您的结果中 不应 包含任何括号。

__示例 :__

示例 1：

输入：s = "(abcd)"
输出："dcba"

示例 2：

输入：s = "(u(love)i)"
输出："iloveu"
解释：先反转子字符串 "love" ，然后反转整个字符串。

示例 3：

输入：s = "(ed(et(oc))el)"
输出："leetcode"
解释：先反转子字符串 "oc" ，接着反转 "etco" ，然后反转整个字符串。

示例 4：

输入：s = "a(bcdefghijkl(mno)p)q"
输出："apmnolkjihgfedcbq"

__提示:__

0 <= s.length <= 2000
s 中只有小写英文字母和括号
题目测试用例确保所有括号都是成对出现的

__思路__:

1. 栈简单模拟
用栈记录每次左括号出现的位置
每次遇到匹配的右括号就将括号内的所有字符反转
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)
2. 栈线性模拟
同时记录两个括号出现的位置, 然后每一对括号实际上只用记录对应方向
每次碰到括号就反向将字符加入结果中
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string reverseParentheses(string s) 
    {
        stack<int> s_stack;
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            if (s[i] == '(') s_stack.push(i);
            if (s[i] == ')') 
            {
                reverse(s.begin() + s_stack.top() + 1, s.begin() + i);
                s_stack.pop();
            }
        }
        s.erase(remove(s.begin(), s.end(), '('), s.end());
        s.erase(remove(s.begin(), s.end(), ')'), s.end());
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String reverseParentheses(String s) {
        StringBuilder sb = new StringBuilder();
        Stack<Integer> stack = new Stack<>();
        int n = s.length(), pair[] = new int[n], index = 0, step = 1;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') stack.push(i);
            else if (s.charAt(i) == ')') {
                int j = stack.pop();
                pair[i] = j;
                pair[j] = i;
            }
        }
        while (index < n) {
            if (s.charAt(index) == '(' || s.charAt(index) == ')') {
                index = pair[index];
                step = -step;
            } else sb.append(s.charAt(index));
            index += step;
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def reverseParentheses(self, s):
        return s if '(' not in s else self.reverseParentheses(re.sub(r'\(([^()]*)\)', lambda x: x.group(1)[::-1], s))
```
