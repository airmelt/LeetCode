# 1614 Maximum Nesting Depth of the Parentheses 括号的最大嵌套深度

__Description:__

A string is a valid parentheses string (denoted VPS) if it meets one of the following:

- It is an empty string `""`, or a single character not equal to `"("` or `")"`,
- It can be written as `AB` ( `A` concatenated with `B`), where `A` and `B` are __VPS__'s, or
- It can be written as `(A)`, where `A` is a __VPS__.

We can similarly define the __nesting depth__ `depth(S)` of any VPS `S` as follows:

- `depth("") = 0`
- `depth(C) = 0`, where `C` is a string with a single character not equal to `"("` or `")"`.
- `depth(A + B) = max(depth(A), depth(B))`, where `A` and `B` are __VPS__'s.
- `depth("(" + A + ")") = 1 + depth(A)`, where `A` is a __VPS__.

For example, `""`, `"()()"`, and `"()(()())"` are __VPS__'s (with nesting depths 0, 1, and 2), and `")("` and `"(()"` are not __VPS__'s.

Given a __VPS__ represented as string `s`, return _the __nesting depth__ of_ `s`.

__Example:__

Example 1:

```text
Input: s = "(1+(2*3)+((8)/4))+1"
Output: 3
Explanation: Digit 8 is inside of 3 nested parentheses in the string.
```

Example 2:

```text
Input: s = "(1)+((2))+(((3)))"
Output: 3
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of digits `0-9` and characters `'+'`, `'-'`, `'*'`, `'/'`, `'('`, and `')'`.
- It is guaranteed that parentheses expression `s` is a __VPS__.

__题目描述:__

如果字符串满足以下条件之一，则可以称之为 有效括号字符串（valid parentheses string，可以简写为 VPS）：

- 字符串是一个空字符串 `""`，或者是一个不为 `"("` 或 `")"` 的单字符。
- 字符串可以写为 `AB`（ `A` 与 `B` 字符串连接），其中 `A` 和 `B` 都是 __有效括号字符串__ 。
- 字符串可以写为 `(A)`，其中 `A` 是一个 __有效括号字符串__ 。

类似地，可以定义任何有效括号字符串 `S` 的 __嵌套深度__ `depth(S)`:

- `depth("") = 0`
- `depth(C) = 0`，其中 `C` 是单个字符的字符串，且该字符不是 `"("` 或者 `")"`
- `depth(A + B) = max(depth(A), depth(B))`，其中 `A` 和 `B` 都是 __有效括号字符串__
- `depth("(" + A + ")") = 1 + depth(A)`，其中 `A` 是一个 __有效括号字符串__

例如: `""`、 `"()()"`、 `"()(()())"` 都是 __有效括号字符串__（嵌套深度分别为 0、1、2），而 `")("` 、 `"(()"` 都不是 __有效括号字符串__ 。

给你一个 __有效括号字符串__ `s`，返回该字符串的 `s` __嵌套深度__ 。

__示例:__

示例 1：

```text
输入：s = "(1+(2*3)+((8)/4))+1"
输出：3
解释：数字 8 在嵌套的 3 层括号中。
```

示例 2：

```text
输入：s = "(1)+((2))+(((3)))"
输出：3
```

__提示：__

- `1 <= s.length <= 100`
- `s` 由数字 `0-9` 和字符 `'+'`、 `'-'`、 `'*'`、 `'/'`、 `'('`、 `')'` 组成
- 题目数据保证括号表达式 `s` 是 __有效的括号表达式__

__思路:__

```text
模拟
碰到左括号就加 1
碰到右括号就减 1
记录最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxDepth(string s) 
    {
        int left = 0, result = 0;
        for (const auto& c: s) result = max(left = left + (c == '(') - (c == ')'), result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxDepth(String s) {
        int left = 0, result = 0;
        for (char c: s.toCharArray()) result = Math.max(left = left + (c == '(' ? 1 : (c == ')' ? -1 : 0)), result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxDepth(self, s: str) -> int:
        return max(accumulate(1 if i is '(' else -1 if i is ')' else 0 for i in s))
```
