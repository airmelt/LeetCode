__Description__:
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

__Example__:
Example 1:
Input: "()"
Output: true

Example 2:
Input: "()[]{}"
Output: true

Example 3:
Input: "(]"
Output: false

Example 4:
Input: "([)]"
Output: false

Example 5:
Input: "{[]}"
Output: true

__题目描述__:
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

__示例__:
示例 1:
输入: "()"
输出: true

示例 2:
输入: "()[]{}"
输出: true

示例 3:
输入: "(]"
输出: false

示例 4:
输入: "([)]"
输出: false

示例 5:
输入: "{[]}"
输出: true

__思路__:
1. 采用栈这一数据结构, 遇到左半括号就压入; 在遇到右半括号时, 如果栈顶是匹配的左半括号就弹出, 如果不匹配, 直接返回'false'; 最后栈如果为空, 返回'true'.
2. 可以用replace函数反复删除匹配的括号
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    bool isValid(string s) {
        if (s.empty()) return true;
        stack<char> result;
        string::iterator it = s.begin();
        while (it != s.end()) {
            switch (*it) {
                case '(':
                    result.push(*it);
                    break;
                case '[':
                    result.push(*it);
                    break;
                case '{':
                    result.push(*it);
                    break;
                case ')':
                    if (result.empty() || result.top() != '(') return false;
                    else result.pop();
                    break;
                case ']':
                    if (result.empty() || result.top() != '[') return false;
                    else result.pop();
                    break;
                case '}':
                    if (result.empty() || result.top() != '{') return false;
                    else result.pop();
                    break;
            }
            it++;
        }
        return result.empty();
    }
};
```

__Java__:
```
class Solution {
    public boolean isValid(String s) {
        List<Character> result = new ArrayList<>();
        // 使用string的增强型for循环, string要转化成char数组
        for (Character c : s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') result.add(c);
            else if (c == ')') {
                if (result.isEmpty() || result.get(result.size() - 1) != '(') {
                    return false;
                } else {
                    result.remove(result.size() - 1);
                }
            } else if (c == ']') {
                if (result.isEmpty() || result.get(result.size() - 1) != '[') {
                    return false;
                } else {
                    result.remove(result.size() - 1);
                }
            } else if (c == '}') {
                if (result.isEmpty() || result.get(result.size() - 1) != '{') {
                    return false;
                } else {
                    result.remove(result.size() - 1);
                }
            }
        }
        return result.isEmpty();
    }
}
```

__Python__:
```
class Solution:
    def isValid(self, s: str) -> bool:
        while '()' in s or '[]' in s or '{}' in s:
            s = s.replace('()', '')
            s = s.replace('[]', '')
            s = s.replace('{}', '')
        return s == ''
```
