# 1576 Replace All ?'s to Avoid Consecutive Repeating Characters 替换所有的问号

__Description:__

Given a string `s` containing only lowercase English letters and the `'?'` character, convert __all__ the `'?'` characters into lowercase letters such that the final string does not contain any __consecutive repeating__ characters. You __cannot__ modify the non `'?'` characters.

It is __guaranteed__ that there are no consecutive repeating characters in the given string __except__ for `'?'`.

Return the final string after all the conversions (possibly zero) have been made. If there is more than one solution, return any of them. It can be shown that an answer is always possible with the given constraints.

__Example:__

Example 1:

```text
Input: s = "?zs"
Output: "azs"
Explanation: There are 25 solutions for this problem. From "azs" to "yzs", all are valid. Only "z" is an invalid modification as the string will consist of consecutive repeating characters in "zzs".
```

Example 2:

```text
Input: s = "ubv?w"
Output: "ubvaw"
Explanation: There are 24 solutions for this problem. Only "v" and "w" are invalid modifications as the strings will consist of consecutive repeating characters in "ubvvw" and "ubvww".
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consist of lowercase English letters and `'?'`.

__题目描述:__

给你一个仅包含小写英文字母和 `'?'` 字符的字符串 `s`，请你将所有的 `'?'` 转换为若干小写字母，使最终的字符串不包含任何 __连续重复__ 的字符。

注意：你 __不能__ 修改非 `'?'` 字符。

题目测试用例保证 __除__ `'?'` 字符 __之外__，不存在连续重复的字符。

在完成所有转换（可能无需转换）后返回最终的字符串。如果有多个解决方案，请返回其中任何一个。可以证明，在给定的约束条件下，答案总是存在的。

__示例:__

示例 1：

输入：s = "?zs"
输出："azs"
解释：该示例共有 25 种解决方案，从 "azs" 到 "yzs" 都是符合题目要求的。只有 "z" 是无效的修改，因为字符串 "zzs" 中有连续重复的两个 'z' 。

示例 2：

输入：s = "ubv?w"
输出："ubvaw"
解释：该示例共有 24 种解决方案，只有替换成 "v" 和 "w" 不符合题目要求。因为 "ubvvw" 和 "ubvww" 都包含连续重复的字符。

__提示：__

- `1 <= s.length <= 100`
- `s` 仅包含小写英文字母和 `'?'` 字符

`1 <= s.length <= 100`

`s` 仅包含小写英文字母和 `'?'` 字符

__思路:__

```text
模拟
可以分类讨论判断是否为字符串的两端, 分别给当前字符赋值
也可以在字符串两端加上 '?', 来规避讨论
也可以先给第一个字符赋 'a', 然后根据两个相邻字符的关系调整
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__进阶:__
求替换的种类数
一个两端问号有 25 种选择, 夹在两个相同字符之间的问号有 25 种, 夹在两个不同字符之间的问号有 24 种选择
连续多个问号, 第一个问号与第一种类似, 对第 2 个问号来说, 为 24 种选择
再利用乘法原理求和

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string modifyString(string s) 
    {
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            if (s[i] == '?') 
            {
                if (!i) 
                {
                    for (int j = 0; j < 26; j++) 
                    {
                        char cur = j + 'a';
                        if (cur != s[i + 1]) 
                        {
                            s[i] = cur;
                            break;
                        }
                    }
                } 
                else if (i == n - 1) 
                {
                    for (int j = 0; j < 26; j++) 
                    {
                        char cur = j + 'a';
                        if (cur != s[i - 1]) 
                        {
                            s[i] = cur;
                            break;
                        }
                    }
                } 
                else 
                {
                    for (int j = 0; j < 26; j++) 
                    {
                        char cur = j + 'a';
                        if (cur != s[i - 1] and cur != s[i + 1]) 
                        {
                            s[i] = cur;
                            break;
                        }
                    }
                }
            }
        }
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String modifyString(String s) {
        StringBuilder sb = new StringBuilder(s);
        if (sb.charAt(0) == '?') sb.setCharAt(0, 'a');
        for (int i = 1, n = s.length(); i < n; i++) {
            if (sb.charAt(i) == '?') sb.setCharAt(i, (char)('a' + (sb.charAt(i - 1) - 'a' + 1) % 26));
            else if (sb.charAt(i) == sb.charAt(i - 1)) sb.setCharAt(i - 1, (char)('a' + (sb.charAt(i - 1) - 'a' + 1) % 26));
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def modifyString(self, s: str) -> str:
        s, n = list('?' + s + '?'), len(s) + 2
        for i in range(1, n - 1):
            if s[i] == '?':
                for j in 'abc':
                    if j != s[i + 1] and j != s[i - 1]:
                        s[i] = j
                        break
        return ''.join(s[1:-1])
```
