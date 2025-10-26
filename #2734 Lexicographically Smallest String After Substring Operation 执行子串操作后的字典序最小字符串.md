# 2734 Lexicographically Smallest String After Substring Operation 执行子串操作后的字典序最小字符串

__Description:__

Given a string `s` consisting of lowercase English letters. Perform the following operation:

- Select any non-empty substring then replace every letter of the substring with the preceding letter of the English alphabet. For example, 'b' is converted to 'a', and 'a' is converted to 'z'.

Return the lexicographically smallest string after performing the operation.

__Example:__

Example 1:

```text
Input: s = "cbabc"

Output: "baabc"

Explanation:

Perform the operation on the substring starting at index 0, and ending at index 1 inclusive.
```

Example 2:

```text
Input: s = "aa"

Output: "az"

Explanation:

Perform the operation on the last letter.
```

Example 3:

```text
Input: s = "acbbc"

Output: "abaab"

Explanation:

Perform the operation on the substring starting at index 1, and ending at index 4 inclusive.
```

Example 4:

```text
Input: s = "leetcode"

Output: "kddsbncd"

Explanation:

Perform the operation on the entire string.
```

__Constraints:__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` consists of lowercase English letters

__题目描述:__

给你一个仅由小写英文字母组成的字符串 `s` 。在一步操作中，你可以完成以下行为:

- 选择 `s` 的任一非空子字符串，可能是整个字符串，接着将字符串中的每一个字符替换为英文字母表中的前一个字符。例如，'b' 用 'a' 替换，'a' 用 'z' 替换。

返回执行上述操作 恰好一次 后可以获得的 字典序最小 的字符串。

子字符串 是字符串中的一个连续字符序列。

__示例:__

示例 1：

```text
输入：s = "cbabc"
输出："baabc"
解释：我们选择从下标 0 开始、到下标 1 结束的子字符串执行操作。 
可以证明最终得到的字符串是字典序最小的。
```

示例 2：

```text
输入：s = "acbbc"
输出："abaab"
解释：我们选择从下标 1 开始、到下标 4 结束的子字符串执行操作。
可以证明最终得到的字符串是字典序最小的。
```

示例 3：

```text
输入：s = "leetcode"
输出："kddsbncd"
解释：我们选择整个字符串执行操作。
可以证明最终得到的字符串是字典序最小的。
```

__提示：__

- `1 <= s.length <= 3 * 10 ^ 5`
- `s` 仅由小写英文字母组成

__思路:__

```text
贪心
找到第一个非字符 'a'
从该字符开始直到第一个为 'a' 的字符
都可以操作
例外情况是全 'a'
将最后一个字符改为 'z'
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string smallestString(string s) 
    {
        for (int i = 0, n = s.size(); i < n; i++) 
        {
            if (s[i] > 'a') 
            {
                while (i < n and s[i] > 'a') s[i++]--;
                return s;
            }
        }
        return s.substr(1) + 'z';
    }
};
```

__Java__:

```Java
class Solution {
    public String smallestString(String s) {
        char result[] = s.toCharArray();
        for (int i = 0, n = result.length; i < n; i++) {
            if (s.charAt(i) == 'a') {
                if (i > 0 && result[i - 1] != s.charAt(i - 1)) break;
                continue;
            }
            result[i] = (char)(s.charAt(i) - 1);
        }
        return s.chars().filter(c -> c != 'a').count() == 0L ? (s.substring(1) + "z") : new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def smallestString(self, s: str) -> str:
        result = list(s)
        for i, c in enumerate(s):
            if c == 'a':
                if i > 0 and result[i - 1] != s[i - 1]:
                    break
                continue
            result[i] = chr(ord(c) - 1)
        return s[:-1] + 'z' if set(s) == { 'a' } else ''.join(result)
```
