# 1844 Replace All Digits with Characters 将所有数字用字符替换

__Description:__

You are given a __0-indexed__ string `s` that has lowercase English letters in its __even__ indices and digits in its __odd__ indices.

There is a function `shift(c, x)`, where `c` is a character and `x` is a digit, that returns the `x ^ th` character after `c`.

- For example, `shift('a', 5) = 'f'` and `shift('x', 0) = 'x'`.

For every __odd__ index `i`, you want to replace the digit `s[i]` with `shift(s[i-1], s[i])`.

Return `s` _after replacing all digits. It is __guaranteed__ that_ `shift(s[i-1], s[i])` _will never exceed_ `'z'`.

__Example:__

Example 1:

```text
Input: s = "a1c1e1"
Output: "abcdef"
Explanation: The digits are replaced as follows:
- s[1] -> shift('a',1) = 'b'
- s[3] -> shift('c',1) = 'd'
- s[5] -> shift('e',1) = 'f'
```

Example 2:

```text
Input: s = "a1b2c3d4e"
Output: "abbdcfdhe"
Explanation: The digits are replaced as follows:
- s[1] -> shift('a',1) = 'b'
- s[3] -> shift('b',2) = 'd'
- s[5] -> shift('c',3) = 'f'
- s[7] -> shift('d',4) = 'h'
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists only of lowercase English letters and digits.
- `shift(s[i-1], s[i]) <= 'z'` for all __odd__ indices `i`.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，它的 __偶数__ 下标处为小写英文字母，__奇数__ 下标处为数字。

定义一个函数 `shift(c, x)` ，其中 `c` 是一个字符且 `x` 是一个数字，函数返回字母表中 `c` 后面第 `x` 个字符。

- 比方说， `shift('a', 5) = 'f'` 和 `shift('x', 0) = 'x'` 。

对于每个 __奇数__ 下标 `i` ，你需要将数字 `s[i]` 用 `shift(s[i-1], s[i])` 替换。

请你替换所有数字以后，将字符串 `s` 返回。题目 __保证__ `shift(s[i-1], s[i])` 不会超过 `'z'` 。

__示例:__

示例 1：

```text
输入：s = "a1c1e1"
输出："abcdef"
解释：数字被替换结果如下：
- s[1] -> shift('a',1) = 'b'
- s[3] -> shift('c',1) = 'd'
- s[5] -> shift('e',1) = 'f'
```

示例 2：

```text
输入：s = "a1b2c3d4e"
输出："abbdcfdhe"
解释：数字被替换结果如下：
- s[1] -> shift('a',1) = 'b'
- s[3] -> shift('b',2) = 'd'
- s[5] -> shift('c',3) = 'f'
- s[7] -> shift('d',4) = 'h'
```

__提示：__

- `1 <= s.length <= 100`
- `s` 只包含小写英文字母和数字。
- 对所有 __奇数__ 下标处的 `i` ，满足 `shift(s[i-1], s[i]) <= 'z'` 。

__思路:__

```text
模拟
按照奇偶变换字符
由于题目中已经说明了不会超过 'z' 的范围
不需要额外考虑取模
Cpp 可以原地修改字符串
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string replaceDigits(string s) 
    {
        for (int i = 1, n = s.size(); i < n; i += 2) s[i] += s[i - 1] - '0';
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String replaceDigits(String s) {
        StringBuilder result = new StringBuilder();
        for (int i = 1, n = s.length(); i < n; i += 2) {
            result.append(s.charAt(i - 1));
            result.append((char)(s.charAt(i - 1) + s.charAt(i) - '0'));
        }
        if ((s.length() & 1) != 0) result.append(s.charAt(s.length() - 1));
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def replaceDigits(self, s: str) -> str:
        return "".join(s[i] if not (i & 1) else chr(ord(s[i - 1]) + int(s[i])) for i in range(len(s))) 
```
