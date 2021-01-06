# 150 Evaluate Reverse Polish Notation 逆波兰表达式求值

__Description__:
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

__Note:__

Division between two integers should truncate toward zero.
The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

__Example:__

Example 1:

Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Example 2:

Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Example 3:

Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation:

```text
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

__题目描述__:
根据 逆波兰表示法，求表达式的值。

有效的运算符包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

__说明：__

整数除法只保留整数部分。
给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

__示例 :__

示例 1：

输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: 该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9

示例 2：

输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: 该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6

示例 3：

输入: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
输出: 22
解释:
该算式转化为常见的中缀算术表达式为：

```text
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

逆波兰表达式：

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) \* ( 3 + 4 ) 。
该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) \* ) 。
逆波兰表达式主要有以下两个优点：

去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + \* 也可以依据次序计算出正确结果。
适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。

__思路__:

用栈存储操作数, 也可以用数组代替
遇到数字压入栈, 否则弹出栈顶元素计算并把结果压入栈
最后返回栈顶元素
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int evalRPN(vector<string>& tokens) 
    {
        int s[(tokens.size() >> 1) + 1], index = 0;
        for (auto &token : tokens) 
        {
            if (token[0] == '+') 
            {
                s[index - 2] += s[index - 1];
                --index;
            }
            else if (token[0] == '-' && token.size() == 1)
            {
                s[index - 2] -= s[index - 1];
                --index;
            }
            else if (token[0] == '*')
            {
                s[index - 2] *= s[index - 1];
                --index;
            }
            else if (token[0] == '/') 
            {
                s[index - 2] /= s[index - 1];
                --index;
            }
            else s[index++] = stoi(token);
        }
        return s[0];
    }
};
```

__Java__:

```Java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for (String token : tokens) {
            switch (token) {
                case "+":
                    stack.push(stack.pop() + stack.pop());
                    break;
                case "-":
                    Integer minus = stack.pop();
                    stack.push(stack.pop() - minus);
                    break;
                case "*":
                    stack.push(stack.pop() * stack.pop());
                    break;
                case "/":
                    Integer div = stack.pop();
                    stack.push(stack.pop() / div);
                    break;
                default:
                    stack.push(Integer.parseInt(token));
                    break;
            }
        }
        return stack.peek();
    }
}
```

__Python__:

```Python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack, m = [], {'+': lambda a, b: a + b, '-': lambda a, b: a - b, '*': lambda a, b: a * b, '/': lambda a, b: int(a / b)}
        for i in tokens:
            if i.isdigit():
                stack.append(int(i))
            else:
                a, b = stack.pop(), stack.pop()
                stack.append(m[i](b, a))
        return stack[-1]
```
