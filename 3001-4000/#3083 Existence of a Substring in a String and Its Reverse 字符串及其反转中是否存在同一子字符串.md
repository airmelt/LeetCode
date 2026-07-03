# 3083 Existence of a Substring in a String and Its Reverse 字符串及其反转中是否存在同一子字符串

__Description:__

Given a string `s`, find any substring of length `2` which is also present in the reverse of `s`.

Return `true` _if such a substring exists, and_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: s = "leetcode"

Output: true
```

__Explanation:__ Substring `"ee"` is of length `2` which is also present in `reverse(s) == "edocteel"`.

Example 2:

```text
Input: s = "abcba"

Output: true
```

__Explanation:__ All of the substrings of length `2` `"ab"`, `"bc"`, `"cb"`, `"ba"` are also present in `reverse(s) == "abcba"`.

Example 3:

```text
Input: s = "abcd"

Output: false
```

__Explanation:__ There is no substring of length `2` in `s`, which is also present in the reverse of `s`.

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，请你判断字符串 `s` 是否存在一个长度为 `2` 的子字符串，在 `s` 反转后的字符串中也出现。

如果存在这样的子字符串，返回 `true`；如果不存在，返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "leetcode"

输出：true
```

__解释:__
子字符串 `"ee"` 的长度为 `2`，它也出现在 `reverse(s) == "edocteel"` 中。

示例 2：

```text
输入：s = "abcba"

输出：true
```

__解释:__
所有长度为 `2` 的子字符串 `"ab"`、 `"bc"`、 `"cb"`、 `"ba"` 也都出现在 `reverse(s) == "abcba"` 中。

示例 3：

```text
输入：s = "abcd"

输出：false
```

__解释:__
字符串 `s` 中不存在满足「在其反转后的字符串中也出现」且长度为 `2` 的子字符串。

__提示：__

- `1 <= s.length <= 100`
- 字符串 `s` 仅由小写英文字母组成。

__思路:__

```text
位运算
记录 x | 1 << y
其中 x = s[i - 1], y = s[i]
检查 y | 1 << x 是否出现过
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isSubstringPresent(string s) 
    {
        int visited[26]{ };
        for (int i = 1, n = s.size(), x = 0, y = 0; i < n; i++) {
            visited[x = s[i - 1] - 'a'] |= 1 << (y = s[i] - 'a');
            if (visited[y] >> x & 1) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isSubstringPresent(String s) {
        int visited[] = new int[26], n = s.length();
        for (int i = 1, x = 0, y = 0; i < n; i++) {
            visited[x = s.charAt(i - 1) - 'a'] |= 1 << (y = s.charAt(i) - 'a');
            if ((visited[y] >> x & 1) > 0) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isSubstringPresent(self, s: str) -> bool:
        return any(a + b in s and a + b in s[::-1] for a in ascii_lowercase for b in ascii_lowercase)
```
