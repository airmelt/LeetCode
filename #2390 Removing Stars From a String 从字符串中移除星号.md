# 2390 Removing Stars From a String 从字符串中移除星号

__Description:__

You are given a string `s`, which contains stars `*`.

In one operation, you can:

- Choose a star in `s`.
- Remove the closest __non-star__ character to its __left__, as well as remove the star itself.

Return the string after all stars have been removed.

Note:

- The input will be generated such that the operation is always possible.
- It can be shown that the resulting string will always be unique.

__Example:__

Example 1:

```text
Input: s = "leet**cod*e"
Output: "lecoe"
Explanation: Performing the removals from left to right:
```

- The closest character to the 1st star is 't' in "leet**cod*e". s becomes "lee*cod*e".
- The closest character to the 2nd star is 'e' in "lee*cod*e". s becomes "lecod*e".
- The closest character to the 3rd star is 'd' in "lecod*e". s becomes "lecoe".

There are no more stars, so we return "lecoe".

Example 2:

```text
Input: s = "erase*****"
Output: ""
Explanation: The entire string is removed, so we return an empty string.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters and stars `*`.
- The operation above can be performed on `s`.

__题目描述:__

给你一个包含若干星号 `*` 的字符串 `s` 。

在一步操作中，你可以：

- 选中 `s` 中的一个星号。
- 移除星号 __左侧__ 最近的那个 __非星号__ 字符，并移除该星号自身。

返回移除 所有 星号之后的字符串。

注意：

- 生成的输入保证总是可以执行题面中描述的操作。
- 可以证明结果字符串是唯一的。

__示例:__

示例 1：

```text
输入：s = "leet**cod*e"
输出："lecoe"
解释：从左到右执行移除操作：
```

- 距离第 1 个星号最近的字符是 "leet**cod*e" 中的 't' ，s 变为 "lee*cod*e" 。
- 距离第 2 个星号最近的字符是 "lee*cod*e" 中的 'e' ，s 变为 "lecod*e" 。
- 距离第 3 个星号最近的字符是 "lecod*e" 中的 'd' ，s 变为 "lecoe" 。

不存在其他星号，返回 "lecoe" 。

示例 2：

```text
输入：s = "erase*****"
输出：""
解释：整个字符串都会被移除，所以返回空字符串。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 由小写英文字母和星号 `*` 组成
- `s` 可以执行上述操作

__思路:__

```text
栈
遍历字符串 s
如果当前字符为星号
则弹出栈顶元素
否则将当前字符入栈
最后返回栈中元素组成的字符串
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string removeStars(string s) 
    {
        string result;
        for (const auto& c : s) 
        {
            if (c == '*') result.pop_back();
            else result += c;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String removeStars(String s) {
        StringBuilder sb = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c == '*') sb.deleteCharAt(sb.length() - 1);
            else sb.append(c);
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def removeStars(self, s: str) -> str:
        stack = []
        for c in s:
            if c == "*":
                stack.pop()
            else:
                stack.append(c)
        return "".join(stack)
```
