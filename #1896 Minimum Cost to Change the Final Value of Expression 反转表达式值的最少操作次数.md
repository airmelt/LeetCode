# 1896 Minimum Cost to Change the Final Value of Expression 反转表达式值的最少操作次数

__Description:__

You are given a __valid__ boolean expression as a string `expression` consisting of the characters `'1'`, `'0'`, `'&'` (bitwise __AND__ operator), `'|'` (bitwise __OR__ operator), `'('`, and `')'`.

- For example, `"()1|1"` and `"(1)&()"` are __not valid__ while `"1"`, `"(((1))|(0))"`, and `"1|(0&(1))"` are __valid__ expressions.

Return the minimum cost to change the final value of the expression.

- For example, if `expression = "1|1|(0&0)&1"`, its __value__ is `1|1|(0&0)&1 = 1|1|0&1 = 1|0&1 = 1&1 = 1`. We want to apply operations so that the __new__ expression evaluates to `0`.

The cost of changing the final value of an expression is the number of operations performed on the expression. The types of operations are described as follows:

- Turn a `'1'` into a `'0'`.
- Turn a `'0'` into a `'1'`.
- Turn a `'&'` into a `'|'`.
- Turn a `'|'` into a `'&'`.

__Note:__ `'&'` does __not__ take precedence over `'|'` in the __order of calculation__. Evaluate parentheses __first__, then in __left-to-right__ order.

__Example:__

Example 1:

```text
Input: expression = "1&(0|1)"
Output: 1
Explanation: We can turn "1&(0|1)" into "1&(0&1)" by changing the '|' to a '&' using 1 operation.
The new expression evaluates to 0.
```

Example 2:

```text
Input: expression = "(0&0)&(0&0&0)"
Output: 3
Explanation: We can turn "(0&0)&(0&0&0)" into "(0|1)|(0&0&0)" using 3 operations.
The new expression evaluates to 1.
```

Example 3:

```text
Input: expression = "(0|(1|0&1))"
Output: 1
Explanation: We can turn "(0|(1|0&1))" into "(0|(0|0&1))" using 1 operation.
The new expression evaluates to 0.
```

__Constraints:__

- `1 <= expression.length <= 10 ^ 5`
- `expression` only contains `'1'`, `'0'`, `'&'`, `'|'`, `'('`, and `')'`
- All parentheses are properly matched.
- There will be no empty parentheses (i.e: `"()"` is not a substring of `expression`).

__题目描述:__

给你一个 __有效的__ 布尔表达式，用字符串 `expression` 表示。这个字符串包含字符 `'1'`， `'0'`， `'&'`（按位 __与__ 运算）， `'|'`（按位 __或__ 运算）， `'('` 和 `')'` 。

- 比方说， `"()1|1"` 和 `"(1)&()"` __不是有效__ 布尔表达式。而 `"1"`， `"(((1))|(0))"` 和 `"1|(0&(1))"` 是 __有效__ 布尔表达式。

你的目标是将布尔表达式的 __值__ __反转__ （也就是将 `0` 变为 `1` ，或者将 `1` 变为 `0`），请你返回达成目标需要的 __最少操作__ 次数。

- 比方说，如果表达式 `expression = "1|1|(0&0)&1"` ，它的 __值__ 为 `1|1|(0&0)&1 = 1|1|0&1 = 1|0&1 = 1&1 = 1` 。我们想要执行操作将 __新的__ 表达式的值变成 `0` 。

可执行的 操作 如下：

- 将一个 `'1'` 变成一个 `'0'` 。
- 将一个 `'0'` 变成一个 `'1'` 。
- 将一个 `'&'` 变成一个 `'|'` 。
- 将一个 `'|'` 变成一个 `'&'` 。

__注意:__`'&'` 的 __运算优先级__ 与 `'|'` __相同__ 。计算表达式时，括号优先级 __最高__ ，然后按照 __从左到右__ 的顺序运算。

__示例:__

示例 1：

```text
输入：expression = "1&(0|1)"
输出：1
解释：我们可以将 "1&(0|1)" 变成 "1&(0&1)" ，执行的操作为将一个 '|' 变成一个 '&' ，执行了 1 次操作。
新表达式的值为 0 。
```

示例 2：

```text
输入：expression = "(0&0)&(0&0&0)"
输出：3
解释：我们可以将 "(0&0)&(0&0&0)" 变成 "(0|1)|(0&0&0)" ，执行了 3 次操作。
新表达式的值为 1 。
```

示例 3：

```text
输入：expression = "(0|(1|0&1))"
输出：1
解释：我们可以将 "(0|(1|0&1))" 变成 "(0|(0|0&1))" ，执行了 1 次操作。
新表达式的值为 0 。
```

__提示：__

- `1 <= expression.length <= 10 ^ 5`
- `expression` 只包含 `'1'`， `'0'`， `'&'`， `'|'`， `'('` 和 `')'`
- 所有括号都有与之匹配的对应括号。
- 不会有空的括号（也就是说 `"()"` 不是 `expression` 的子字符串）。

__思路:__

```text
动态规划 ➕ 栈
设 p1, q1 表示最终表达式的值分别为 0, 1 时, 最少操作次数
由于有两个操作数, 再设 p2, q2 表示最终表达式的值分别为 0, 1 时, 最少操作次数
当操作符为 '&'时, 
    若最后返回的为 1, 则要么需要将左右两边的表达式都变为 1, 需要的操作为 q1 + q2; 要么将左右两边的表达式中的其中一个变为 1, 需要的操作次数为 min(q1, q2), 并且将操作符变为 '|', 需要的操作为 1, 总共需要的操作为 min(q1, q2) + 1; 取上述两种情况的最小值为 min(q1 + q2, 1 + min(q1, q2))
    若最后返回的为 0, 则需要将左右两边的表达式任意一个都变为 0, 需要的操作为 min(p1, p2)
当操作符为 '|'时,
    若最后返回的为 1, 则需要将左右两边的表达式任意一个都变为 1, 需要的操作为 min(q1, q2)
    若最后返回的为 0, 则要么需要将左右两边的表达式都变为 0, 需要的操作为 p1 + p2; 要么将左右两边的表达式中的其中一个变为 0, 需要的操作次数为 min(p1, p2), 并且将操作符变为 '&', 需要的操作为 1, 总共需要的操作为 min(p1, p2) + 1; 取上述两种情况的最小值为 min(p1 + p2, 1 + min(p1, p2))
我们用两个栈分别存储状态和操作符
具体操作是当字符为 '&|(' 时, 将对应的操作符压入操作符栈中, 当字符为 '01)' 时, 将对应的状态压入栈中
当字符为 '0' 时, 状态栈记录状态 (0, 1)
当字符为 '1' 时, 状态栈记录状态 (1, 0)
当字符为 ')' 时, 如果操作符为 '(' 则弹出, 否则应该报错, 由于题目保证了表达式的合法性, 所以不会出现这种情况
并且计算栈顶两个状态的最小操作次数, 并将结果压入栈中
时间复杂度为 O(N), 空间复杂度为 O(N), N 为表达式的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperationsToFlip(string expression) 
    {
        stack<pair<int, int>> states;
        stack<char> ops;
        string target = "01)";
        for (const auto& c : expression)
        {
            if (target.find(c) != string::npos) 
            {
                if (c == '0') states.push(make_pair(0, 1));
                else if (c == '1') states.push(make_pair(1, 0));
                else ops.pop();
                if (!ops.empty() && ops.top() != '(')
                {
                    const auto& [p2, q2] = states.top();
                    states.pop();
                    const auto& [p1, q1] = states.top();
                    states.pop();
                    const auto op = ops.top();
                    ops.pop();
                    if (op == '&') states.push(make_pair(min(p1, p2), min(q1 + q2, 1 + min(q1, q2))));
                    else states.push(make_pair(min(p1 + p2, 1 + min(p1, p2)), min(q1, q2)));
                }
            }
            else ops.push(c);
        }
        return max(states.top().first, states.top().second);
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperationsToFlip(String expression) {
        Stack<int[]> states = new Stack<>();
        Stack<Character> ops = new Stack<>();
        for (char c : expression.toCharArray()) {
            if ("01)".contains(c + "")) {
                if (c == '0') states.push(new int[]{0, 1});
                else if (c == '1') states.push(new int[]{1, 0});
                else ops.pop();
                if (!ops.isEmpty() && ops.peek() != '(') {
                    int[] p = states.pop(), q = states.pop();
                    char op = ops.pop();
                    if (op == '&') states.add(new int[]{Math.min(p[0], q[0]), Math.min(p[1] + q[1], Math.min(p[1], q[1]) + 1)});
                    else states.add(new int[]{Math.min(p[0] + q[0], Math.min(p[0], q[0]) + 1), Math.min(p[1], q[1])});
                }
            } else ops.push(c);
        }
        return Math.max(states.peek()[0], states.peek()[1]);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperationsToFlip(self, expression: str) -> int:
        states, ops = [], []
        for c in expression:
            if c in '01)':
                if c == '0':
                    states.append((0, 1))
                elif c == '1':
                    states.append((1, 0))
                else:
                    ops.pop()
                if ops and ops[-1] != '(':
                    op = ops.pop()
                    p2, q2 = states.pop()
                    p1, q1 = states.pop()
                    if op == '&':
                        states.append((min(p1, p2), min(q1 + q2, 1 + min(q1, q2))))
                    else:
                        states.append((min(p1 + p2, 1 + min(p1, p2)), min(q1, q2)))
            else:
                ops.append(c)
        return max(states[-1])
```
