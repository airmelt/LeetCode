# 224 Basic Calculator 基本计算器

__Description__:
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

__Example:__

Example 1:

Input: "1 + 1"
Output: 2

Example 2:

Input: " 2-1 + 2 "
Output: 3

Example 3:

Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23

__Note:__
You may assume that the given expression is always valid.
Do not use the eval built-in library function.

__题目描述__:
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式可以包含左括号 ( ，右括号 )，加号 + ，减号 -，非负整数和空格  。

__示例 :__

示例 1:

输入: "1 + 1"
输出: 2

示例 2:

输入: " 2-1 + 2 "
输出: 3

示例 3:

输入: "(1+(4+5+2)-3)+(6+8)"
输出: 23

__说明：__

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

__思路__:

用栈记录
因为这里只有加减法运算, 可以用一个符号, 记录加减符号, 1代表 ‘+’, -1代表 ‘-’
当遇到左括号时, 当前结果和符号入栈, 重置结果和符号
遇到右括号时, 弹出结果和符号的乘积与之前的结果求和
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int calculate(string s) 
    {
        vector<int> record;
        int sign = 1, result = 0, n = s.size();
        for (int i = 0; i < n; i++) 
        {
            auto c = s[i];
            if (isdigit(c)) 
            {
                auto cur = c - '0';
                while (i < n - 1 and isdigit(s[i + 1])) cur = 10 * cur + (s[++i] - '0');
                result += sign * cur;
            } 
            else if (c == '+') sign = 1;
            else if (c == '-') sign = -1;
            else if (c == '(') 
            {
                record.push_back(result);
                result = 0;
                record.push_back(sign);
                sign = 1;
            } 
            else if (c == ')') 
            {
                result *= record.back();
                record.pop_back();
                result += record.back(); 
                record.pop_back();
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int calculate(String s) {
        Stack<Integer> stack = new Stack<>();
        int sign = 1, result = 0, n = s.length();
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int cur = c - '0';
                while (i < n - 1 && Character.isDigit(s.charAt(i + 1))) cur = 10 * cur + s.charAt(++i) - '0';
                result += sign * cur;
            } else if (c == '+') sign = 1;
            else if (c == '-') sign = -1;
            else if (c == '(') {
                stack.push(result);
                result = 0;
                stack.push(sign);
                sign = 1;
            } else if (c == ')') result = stack.pop() * result + stack.pop(); 
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def calculate(self, s: str) -> int:
        stack, sign, result, n, i = [], 1, 0, len(s), 0
        while i < n:
            c = s[i]
            if c.isnumeric():
                cur = int(c)
                while i < n - 1 and s[i + 1].isnumeric():
                    cur *= 10
                    i += 1
                    cur += int(s[i])
                result += sign * cur
            elif c == '+':
                sign = 1
            elif c == '-':
                sign = -1
            elif c == '(':
                stack.append(result)
                result = 0
                stack.append(sign)
                sign = 1
            elif c == ')':
                result = stack.pop() * result + stack.pop()
            i += 1
        return result
```
