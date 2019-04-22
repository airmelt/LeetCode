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

__[堆栈 Stack list-wiki](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))
)__:
> 堆栈（英语：stack）又称为栈或堆叠，是计算机科学中的一种抽象数据类型，只允许在有序的线性数据集合的一端（称为堆栈顶端，英语：top）进行加入数据（英语：push）和移除数据（英语：pop）的运算。
> 按照后进先出（LIFO, Last In First Out）的原理运作。
> 现实生活中可以类比堆在一起的光盘(唱片/书/餐盘), 只能从上往下拿, 可以堆叠
> 堆栈常用一维数组或链表来实现。
> 操作, 堆栈使用两种基本操作：推入（压栈，push）和弹出（弹栈，pop）：
> 1. 推入：将数据放入堆栈顶端，堆栈顶端移到新放入的数据。
> 2. 弹出：将堆栈顶端数据移除，堆栈顶端移到移除后的下一笔数据。
> C++定义堆栈(也可以用vector):
> ```
> struct Stack {
>    // 堆栈空间
>    int val[10];
>    // 指向栈顶
>    int top;
> };
> ```
> 另外 C++中的 STL也有stack
> #include<stack>
> 常用函数有:
> ```
> s.push(item);       //将item压入栈顶  
> s.pop();            //删除栈顶的元素，但不会返回  
> s.top();            //返回栈顶的元素，但不会删除  
> s.size();           //返回栈中元素的个数  
> s.empty();          //检查栈是否为空，如果为空返回true，否则返回false   
> ```

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
