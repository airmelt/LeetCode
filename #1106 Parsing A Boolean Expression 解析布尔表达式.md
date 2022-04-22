# 1106 Parsing A Boolean Expression 解析布尔表达式

__Description__:
Return the result of evaluating a given boolean expression, represented as a string.

An expression can either be:

"t", evaluating to True;
"f", evaluating to False;
"!(expr)", evaluating to the logical NOT of the inner expression expr;
"&(expr1,expr2,...)", evaluating to the logical AND of 2 or more inner expressions expr1, expr2, ...;
"|(expr1,expr2,...)", evaluating to the logical OR of 2 or more inner expressions expr1, expr2, ...

__Example:__

Example 1:

Input: expression = "!(f)"
Output: true

Example 2:

Input: expression = "|(f,t)"
Output: true

Example 3:

Input: expression = "&(t,f)"
Output: false

__Constraints:__

1 <= expression.length <= 2 * 10^4
expression[i] consists of characters in {'(', ')', '&', '|', '!', 't', 'f', ','}.
expression is a valid expression representing a boolean, as given in the description.

__题目描述__:
给你一个以字符串形式表述的 布尔表达式（boolean） expression，返回该式的运算结果。

有效的表达式需遵循以下约定：

"t"，运算结果为 True
"f"，运算结果为 False
"!(expr)"，运算过程为对内部表达式 expr 进行逻辑 非的运算（NOT）
"&(expr1,expr2,...)"，运算过程为对 2 个或以上内部表达式 expr1, expr2, ... 进行逻辑 与的运算（AND）
"|(expr1,expr2,...)"，运算过程为对 2 个或以上内部表达式 expr1, expr2, ... 进行逻辑 或的运算（OR）

__示例 :__

示例 1：

输入：expression = "!(f)"
输出：true

示例 2：

输入：expression = "|(f,t)"
输出：true

示例 3：

输入：expression = "&(t,f)"
输出：false

示例 4：

输入：expression = "|(&(t,f,t),!(t))"
输出：false

__提示:__

1 <= expression.length <= 20000
expression[i] 由 {'(', ')', '&', '|', '!', 't', 'f', ','} 中的字符组成。
expression 是以上述形式给出的有效表达式，表示一个布尔值。

__思路__:

1. 栈
使用两个栈, 一个栈记录操作, 另一个栈记录操作数(true, false)
遍历 expression, 将操作数和操作压入栈直到遇到右括号
按照不同的操作给出当前操作的结果并存入操作数栈
最后得到的操作数栈顶即为结果
时间复杂度O(n), 空间复杂度O(n)
2. 递归
每次遇到 '|' 或者 '&' 进入递归处理
其他正常遍历即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool parseBoolExpr(string expression) 
    {
        const char *e = expression.c_str();
        return parse(e, &e);
    }
private:
    bool parse(const char *e, const char **p)
    {
        bool result;
        *p = e + 1;
        if (*e == '!') 
        {
            result = !parse(*p + 1, p);
            ++*p;
            return result;
        }
        if (*e == 'f') return false;
        if (*e == 't') return true;
        result = (*e == '|' ? false : true);
        do 
        {
            if (*e == '|') result = parse(*p + 1, p) or result;
            else result = parse(*p + 1, p) and result;
        } while (**p != ')');
        ++*p;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean parseBoolExpr(String expression) {
        Deque<Character> operands = new LinkedList<>(), operators = new LinkedList<>();
        for (char c :expression.toCharArray()) {
            if ('t' == c || 'f' == c || '(' == c) operands.push(c);
            else if ('|' == c || '&' == c || '!' == c) operators.push(c);
            else if (')' == c) {
                char operator = operators.pop();
                boolean result = '&' == operator;
                while (!operands.isEmpty()) {
                    char operand = operands.pop();
                    if ('(' == operand) break;
                    else if ('&' == operator) result &= (operand == 't');
                    else if ('|' == operator) result |= (operand == 't');
                    else result = operand == 'f';
                }
                operands.push(result ? 't' : 'f');
            }
        }
        return operands.pop() == 't';
    }
}
```

__Python__:

```Python
class Solution:
    def parseBoolExpr(self, expression: str) -> bool:
        return eval(expression.replace('f', '0').replace('t', '1').replace('!', 'not |').replace('&(', 'all([').replace('|(', 'any([').replace(')', '])'))
```
