# 1541 Minimum Insertions to Balance a Parentheses String 平衡括号字符串的最少插入次数

__Description:__

Given a parentheses string `s` containing only the characters `'('` and `')'`. A parentheses string is __balanced__ if:

- Any left parenthesis `'('` must have a corresponding two consecutive right parenthesis `'))'`.
- Left parenthesis `'('` must go before the corresponding two consecutive right parenthesis `'))'`.

In other words, we treat `'('` as an opening parenthesis and `'))'` as a closing parenthesis.

- For example, `"())"`, `"())(())))"` and `"(())())))"` are balanced, `")()"`, `"()))"` and `"(()))"` are not balanced.

You can insert the characters `'('` and `')'` at any position of the string to balance it if needed.

Return _the minimum number of insertions_ needed to make `s` balanced.

__Example:__

Example 1:

```text
Input: s = "(()))"
Output: 1
Explanation: The second '(' has two matching '))', but the first '(' has only ')' matching. We need to add one more ')' at the end of the string to be "(())))" which is balanced.
```

Example 2:

```text
Input: s = "())"
Output: 0
Explanation: The string is already balanced.
```

Example 3:

```text
Input: s = "))())("
Output: 3
Explanation: Add '(' to match the first '))', Add '))' to match the last '('.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of `'('` and `')'` only.

__题目描述:__

给你一个括号字符串 `s` ，它只包含字符 `'('` 和 `')'` 。一个括号字符串被称为平衡的当它满足：

- 任何左括号 `'('` 必须对应两个连续的右括号 `'))'` 。
- 左括号 `'('` 必须在对应的连续两个右括号 `'))'` 之前。

比方说 `"())"`， `"())(())))"` 和 `"(())())))"` 都是平衡的， `")()"`， `"()))"` 和 `"(()))"` 都是不平衡的。

你可以在任意位置插入字符 '(' 和 ')' 使字符串平衡。

请你返回让 `s` 平衡的最少插入次数。

__示例:__

示例 1：

```text
输入：s = "(()))"
输出：1
解释：第二个左括号有与之匹配的两个右括号，但是第一个左括号只有一个右括号。我们需要在字符串结尾额外增加一个 ')' 使字符串变成平衡字符串 "(())))" 。
```

示例 2：

```text
输入：s = "())"
输出：0
解释：字符串已经平衡了。
```

示例 3：

```text
输入：s = "))())("
输出：3
解释：添加 '(' 去匹配最开头的 '))' ，然后添加 '))' 去匹配最后一个 '(' 。
```

示例 4：

```text
输入：s = "(((((("
输出：12
解释：添加 12 个 ')' 得到平衡字符串。
```

示例 5：

```text
输入：s = ")))))))"
输出：5
解释：在字符串开头添加 4 个 '(' 并在结尾添加 1 个 ')' ，字符串变成平衡字符串 "(((())))))))" 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含 `'('` 和 `')'` 。

__思路:__

```text
模拟
统计 left 的个数
每次遇到右括号时，left 减 1
left 减到 0 时, 需要补充一个 '('
当只遇到一个 ')' 时，需要补充一个 ')'
否则遍历时跳过下一个 ')'
最后如果 left 还有剩余需要加到结果中去
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minInsertions(string s) 
    {
        int result = 0, n = s.size(), left = 0;
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == '(') ++left;
            else 
            {
                if (!left) ++result;
                else --left;
                if (i == n - 1 or s[i + 1] != ')') ++result;
                else i++;
            }
        }
        return result + (left << 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int minInsertions(String s) {
        int result = 0, n = s.length(), left = 0;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(') ++left;
            else {
                if (left == 0) ++result;
                else --left;
                if (i == n - 1 || s.charAt(i + 1) != ')') ++result;
                else i++;
            }
        }
        return result + (left << 1);
    }
}
```

__Python__:

```Python
class Solution:
    def minInsertions(self, s: str) -> int:
        result, left, n, i = 0, 0, len(s), 0
        while i < n:
            if s[i] == '(':
                left += 1
            else:
                if not left:
                    result += 1
                else:
                    left -= 1
                if i == n - 1 or s[i + 1] != ')':
                    result += 1
                else:
                    i += 1
            i += 1
        return result + (left << 1)
```
