# 736 Parse Lisp Expression Lisp 语法解析

__Description__:
You are given a string expression representing a Lisp-like expression to return the integer value of.

The syntax for these expressions is given as follows.

An expression is either an integer, let expression, add expression, mult expression, or an assigned variable. Expressions always evaluate to a single integer.
(An integer could be positive or negative.)
A let expression takes the form "(let v1 e1 v2 e2 ... vn en expr)", where let is always the string "let", then there are one or more pairs of alternating variables and expressions, meaning that the first variable v1 is assigned the value of the expression e1, the second variable v2 is assigned the value of the expression e2, and so on sequentially; and then the value of this let expression is the value of the expression expr.
An add expression takes the form "(add e1 e2)" where add is always the string "add", there are always two expressions e1, e2 and the result is the addition of the evaluation of e1 and the evaluation of e2.
A mult expression takes the form "(mult e1 e2)" where mult is always the string "mult", there are always two expressions e1, e2 and the result is the multiplication of the evaluation of e1 and the evaluation of e2.
For this question, we will use a smaller subset of variable names. A variable starts with a lowercase letter, then zero or more lowercase letters or digits. Additionally, for your convenience, the names "add", "let", and "mult" are protected and will never be used as variable names.
Finally, there is the concept of scope. When an expression of a variable name is evaluated, within the context of that evaluation, the innermost scope (in terms of parentheses) is checked first for the value of that variable, and then outer scopes are checked sequentially. It is guaranteed that every expression is legal. Please see the examples for more details on the scope.

__Example:__

Example 1:

Input: expression = "(let x 2 (mult x (let x 3 y 4 (add x y))))"
Output: 14
Explanation: In the expression (add x y), when checking for the value of the variable x,
we check from the innermost scope to the outermost in the context of the variable we are trying to evaluate.
Since x = 3 is found first, the value of x is 3.

Example 2:

Input: expression = "(let x 3 x 2 x)"
Output: 2
Explanation: Assignment in let statements is processed sequentially.

Example 3:

Input: expression = "(let x 1 y 2 x (add x y) (add x y))"
Output: 5
Explanation: The first (add x y) evaluates as 3, and is assigned to x.
The second (add x y) evaluates as 3+2 = 5.

Example 4:

Input: expression = "(let x 2 (add (let x 3 (let x 4 x)) x))"
Output: 6
Explanation: Even though (let x 4 x) has a deeper scope, it is outside the context
of the final x in the add-expression.  That final x will equal 2.

Example 5:

Input: expression = "(let a1 3 b2 (add a1 1) b2)"
Output: 4
Explanation: Variable names can contain digits after the first character.

__Constraints:__

1 <= expression.length <= 2000
There are no leading or trailing spaces in exprssion.
All tokens are separated by a single space in expressoin.
The answer and all intermediate calculations of that answer are guaranteed to fit in a 32-bit integer.
The expression is guaranteed to be legal and evaluate to an integer.

__题目描述__:
给定一个类似 Lisp 语句的表达式 expression，求出其计算结果。

表达式语法如下所示:

表达式可以为整数，let 语法，add 语法，mult 语法，或赋值的变量。表达式的结果总是一个整数。
(整数可以是正整数、负整数、0)
let 语法表示为 (let v1 e1 v2 e2 ... vn en expr), 其中 let语法总是以字符串 "let"来表示，接下来会跟随一个或多个交替变量或表达式，也就是说，第一个变量 v1被分配为表达式 e1 的值，第二个变量 v2 被分配为表达式 e2 的值，以此类推；最终 let 语法的值为 expr表达式的值。
add 语法表示为 (add e1 e2)，其中 add 语法总是以字符串 "add"来表示，该语法总是有两个表达式e1、e2, 该语法的最终结果是 e1 表达式的值与 e2 表达式的值之和。
mult 语法表示为 (mult e1 e2) ，其中 mult 语法总是以字符串"mult"表示， 该语法总是有两个表达式 e1、e2，该语法的最终结果是 e1 表达式的值与 e2 表达式的值之积。
在该题目中，变量的命名以小写字符开始，之后跟随0个或多个小写字符或数字。为了方便，"add"，"let"，"mult"会被定义为"关键字"，不会在表达式的变量命名中出现。
最后，要说一下作用域的概念。计算变量名所对应的表达式时，在计算上下文中，首先检查最内层作用域（按括号计），然后按顺序依次检查外部作用域。我们将保证每一个测试的表达式都是合法的。有关作用域的更多详细信息，请参阅示例。

__示例 :__

示例 1：

输入: (add 1 2)
输出: 3

示例 2：

输入: (mult 3 (add 2 3))
输出: 15

示例 3：

输入: (let x 2 (mult x 5))
输出: 10

示例 4：

输入: (let x 2 (mult x (let x 3 y 4 (add x y))))
输出: 14
解释:
表达式 (add x y), 在获取 x 值时, 我们应当由最内层依次向外计算, 首先遇到了 x=3, 所以此处的 x 值是 3.

示例 5：

输入: (let x 3 x 2 x)
输出: 2
解释: let 语句中的赋值运算按顺序处理即可

示例 6：

输入: (let x 1 y 2 x (add x y) (add x y))
输出: 5
解释:
第一个 (add x y) 计算结果是 3，并且将此值赋给了 x 。
第二个 (add x y) 计算结果就是 3+2 = 5 。

示例 7：

输入: (let x 2 (add (let x 3 (let x 4 x)) x))
输出: 6
解释:
(let x 4 x) 中的 x 的作用域仅在()之内。所以最终做加法操作时，x 的值是 2 。

示例 8：

输入: (let a1 3 b2 (add a1 1) b2)
输出: 4
解释:
变量命名时可以在第一个小写字母后跟随数字.

__注意:__

我们给定的 expression 表达式都是格式化后的：表达式前后没有多余的空格，表达式的不同部分(关键字、变量、表达式)之间仅使用一个空格分割，并且在相邻括号之间也没有空格。我们给定的表达式均为合法的且最终结果为整数。
我们给定的表达式长度最多为 2000 (表达式也不会为空，因为那不是一个合法的表达式)。
最终的结果和中间的计算结果都将是一个 32 位整数。

__思路__:

递归 ➕ 栈 ➕ 哈希表
每次递归处理一个括号里面的所有字符串
如果处理的是一个数字字符串直接返回, 数字要求是以负号开头或者开头是数字
如果处理的是一个字母开头的字符串, 则按照 let 和 add/muti 分别递归处理
let 的情况, 将变量的值存入对应的哈希表中, 注意因为会有相同的变量, 所以每次处理哈希表的时候应该新建一个哈希表, 不能使用引用
add/muti 的情况, 分别对两个变量进行计算即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int evaluate(string expression) 
    {
        unordered_map<string, int> m;
        return helper(expression, m, 1, expression.size() - 2);
    }
private:
    int helper(string &expression, unordered_map<string, int> m, int start, int end)
    {
        string cur = "";
        while (expression[start] != ' ') cur += expression[start++];
        if (cur != "let" and cur != "add" and cur != "mult") 
        {
            if (isdigit(cur[0]) or cur[0] == '-') return stoi(cur);
            return m[cur];
        }
        if (cur == "let")
        {
            ++start;
            while (expression[start] != '(')
            {
                string name = "";
                int val = 0;
                while (expression[start] != ' ' and expression[start] != ')') name += expression[start++];
                if (expression[start] == ')')
                {
                    if (isdigit(name[0]) or name[0] == '-') return stoi(name);
                    return m[name];
                }
                ++start;
                if (expression[start] == '(')
                {
                    int count = 1, begin = start + 1;
                    ++start;
                    while (count)
                    {
                        if (expression[start] == '(') ++count;
                        else if (expression[start] == ')') --count;
                        ++start;
                    }
                    val = helper(expression, m, begin, start - 2);
                }
                else
                {
                    string another = "";
                    while (expression[start] != ' ') another += expression[start++];
                    if (isdigit(another[0]) or another[0] == '-') val = stoi(another);
                    else val = m[another];
                }
                m[name] = val;
                ++start;
            }
            return helper(expression, m, start + 1, end - 1);
        }
        vector<int> nums;
        for (int i = start; i <= end;)
        {
            if (expression[i] == ' ') ++i;
            else if (expression[i] == '(')
            {
                int count = 1, begin = i + 1;
                ++i;
                while (count)
                {
                    if (expression[i] == '(') ++count;
                    else if (expression[i] == ')') --count;
                    ++i;
                }
                nums.emplace_back(helper(expression, m, begin, i - 2));
            }
            else
            {
                string another = "";
                while (i <= end and expression[i] != ' ') another += expression[i++];
                if (isdigit(another[0]) or another[0] == '-') nums.emplace_back(stoi(another));
                else nums.emplace_back(m[another]);
            }
        }
        return cur == "add" ? nums.front() + nums.back() : nums.front() * nums.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int evaluate(String expression) {
        return helper(Arrays.asList(expression.replace(")", " )").split(" ")), new HashMap<>());
    }
    
    private int helper(List<String> vals, Map<String, Integer> params) {
        boolean let = "(let".equals(vals.get(0)), add = "(add".equals(vals.get(0));
        Stack<String> stack = new Stack<>();
        for (int i = 1; i < vals.size() - 1; i++) {
            String val = vals.get(i);
            if (val.startsWith("(")) {
                int j = i + 1;
                for (int count = 1; count > 0; j++) {
                    if (vals.get(j).startsWith("(")) ++count;
                    else if (vals.get(j).equals(")")) --count;
                }
                val = Integer.toString(helper(vals.subList(i, j), new HashMap<>(params)));
                i = j - 1;
            }
            if (let && (stack.size() & 1) != 0) params.put(stack.peek(), params.containsKey(val) ? params.get(val) : Integer.parseInt(val));
            stack.push(val);
        }
        String top = stack.pop();
        int a = params.containsKey(top) ? params.get(top) : Integer.parseInt(top);
        if (let) return a;
        top = stack.pop();
        int b = params.containsKey(top) ? params.get(top) : Integer.parseInt(top);
        return add ? a + b : a * b;
    }
}
```

__Python__:

```Python
class Solution:
    def evaluate(self, expression: str) -> int:
        def helper(vals: dict, obj: any) -> int:
            if isinstance(obj, tuple):
                if obj[0] == 'let':
                    vals = vals.copy()
                    for i in range(1, len(obj) - 1, 2):
                        vals[obj[i]] = helper(vals, obj[i + 1])
                    return helper(vals, obj[-1])
                if obj[0] == 'add':
                    return helper(vals, obj[1]) + helper(vals, obj[2])
                if obj[0] == 'mult':
                    return helper(vals, obj[1]) * helper(vals, obj[2])
            return eval(obj, {}, vals)
        return helper({}, eval(re.sub(r'([^( )]+)', r"'\1'", expression).replace(' ', ',')))
```
