# 2116 Check if a Parentheses String Can Be Valid 判断一个括号字符串是否有效

__Description:__

A parentheses string is a __non-empty__ string consisting only of `'('` and `')'`. It is valid if __any__ of the following conditions is __true__:

- It is `()`.
- It can be written as `AB` ( `A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
- It can be written as `(A)`, where `A` is a valid parentheses string.

You are given a parentheses string `s` and a string `locked`, both of length `n`. `locked` is a binary string consisting only of `'0'`s and `'1'`s. For __each__ index `i` of `locked`,

- If `locked[i]` is `'1'`, you __cannot__ change `s[i]`.
- But if `locked[i]` is `'0'`, you __can__ change `s[i]` to either `'('` or `')'`.

Return `true` _if you can make `s` a valid parentheses string_. Otherwise, return `false`.

__Example:__

Example 1:

![2116-1](https://assets.leetcode.com/uploads/2021/11/06/eg1.png)

```text
Input: s = "))()))", locked = "010100"
Output: true
Explanation: locked[1] == '1' and locked[3] == '1', so we cannot change s[1] or s[3].
We change s[0] and s[4] to '(' while leaving s[2] and s[5] unchanged to make s valid.
```

Example 2:

```text
Input: s = "()()", locked = "0000"
Output: true
Explanation: We do not need to make any changes because s is already valid.
```

Example 3:

```text
Input: s = ")", locked = "0"
Output: false
Explanation: locked permits us to change s[0]. 
Changing s[0] to either '(' or ')' will not make s valid.
```

__Constraints:__

- `n == s.length == locked.length`
- `1 <= n <= 10 ^ 5`
- `s[i]` is either `'('` or `')'`.
- `locked[i]` is either `'0'` or `'1'`.

__题目描述:__

一个括号字符串是只由 `'('` 和 `')'` 组成的 __非空__ 字符串。如果一个字符串满足下面 _任意_ 一个条件，那么它就是有效的:

- 字符串为 `()`.
- 它可以表示为 `AB`（`A` 与 `B` 连接），其中 `A` 和 `B` 都是有效括号字符串。
- 它可以表示为 `(A)` ，其中 `A` 是一个有效括号字符串。

给你一个括号字符串 `s` 和一个字符串 `locked` ，两者长度都为 `n` 。 `locked` 是一个二进制字符串，只包含 `'0'` 和 `'1'` 。对于 `locked` 中 __每一个__ 下标 `i` :

- 如果 `locked[i]` 是 `'1'` ，你 __不能__ 改变 `s[i]` 。
- 如果 `locked[i]` 是 `'0'` ，你 __可以__ 将 `s[i]` 变为 `'('` 或者 `')'` 。

如果你可以将 `s` 变为有效括号字符串，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![2116-2](https://assets.leetcode.com/uploads/2021/11/06/eg1.png)

```text
输入：s = "))()))", locked = "010100"
输出：true
解释：locked[1] == '1' 和 locked[3] == '1' ，所以我们无法改变 s[1] 或者 s[3] 。
我们可以将 s[0] 和 s[4] 变为 '(' ，不改变 s[2] 和 s[5] ，使 s 变为有效字符串。
```

示例 2：

```text
输入：s = "()()", locked = "0000"
输出：true
解释：我们不需要做任何改变，因为 s 已经是有效字符串了。
```

示例 3：

```text
输入：s = ")", locked = "0"
输出：false
解释：locked 允许改变 s[0] 。
但无论将 s[0] 变为 '(' 或者 ')' 都无法使 s 变为有效字符串。
```

__提示：__

- `n == s.length == locked.length`
- `1 <= n <= 10 ^ 5`
- `s[i]` 要么是 `'('` 要么是 `')'` 。
- `locked[i]` 要么是 `'0'` 要么是 `'1'` 。

__思路:__

```text
计数
初始化 left = 0, right = 0
从左到右遍历字符串 s
如果 s[i] == '(' 或者 locked[i] == '0' ，则 left++
否则如果 left > 0 ，则 left--
否则返回 false
这种没有考虑左括号比右括号多的情况
所以需要从右到左遍历字符串 s
如果 s[i] == ')' 或者 locked[i] == '0' ，则 right++
否则如果 right > 0 ，则 right--
否则返回 false
最后返回 true
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canBeValid(string s, string locked) 
    {
        int n = s.size(), left = 0, right = 0;
        if (n & 1) return false;
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == '(' or locked[i] == '0') ++left;
            else if (left > 0) --left;
            else return false;
        }
        for (int i = n - 1; i > -1; i--) 
        {
            if (s[i] == ')' or locked[i] == '0') ++right;
            else if (right > 0) --right;
            else return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canBeValid(String s, String locked) {
        int n = s.length(), left = 0, right = 0;
        if ((n & 1) == 1) return false;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '(' || locked.charAt(i) == '0') ++left;
            else if (left > 0) --left;
            else return false;
        }
        for (int i = n - 1; i > -1; i--) {
            if (s.charAt(i) == ')' || locked.charAt(i) == '0') ++right;
            else if (right > 0) --right;
            else return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canBeValid(self, s: str, locked: str) -> bool:
        left, right, n = 0, 0, len(s)
        if n & 1:
            return False
        for i, c in enumerate(s):
            if c == '(' or locked[i] == '0':
                left += 1
            elif left > 0:
                left -= 1
            else:
                return False
        for i, c in enumerate(s[::-1]):
            if c == ')' or locked[n - i - 1] == '0':
                right += 1
            elif right > 0:
                right -= 1
            else:
                return False
        return True
```
