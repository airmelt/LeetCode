# 2981 Find Longest Special Substring That Occurs Thrice I 找出出现至少三次的最长特殊子字符串 I

__Description:__

You are given a string `s` that consists of lowercase English letters.

A string is called __special__ if it is made up of only a single character. For example, the string `"abc"` is not special, whereas the strings `"ddd"`, `"zz"`, and `"f"` are special.

Return _the length of the __longest special substring__ of_ `s` _which occurs __at least thrice___, _or_ `-1` _if no special substring occurs at least thrice_.

A substring is a contiguous non-empty sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "aaaa"
Output: 2
Explanation: The longest special substring which occurs thrice is "aa": substrings "aaaa", "aaaa", and "aaaa".
It can be shown that the maximum length achievable is 2.
```

Example 2:

```text
Input: s = "abcdef"
Output: -1
Explanation: There exists no special substring which occurs at least thrice. Hence return -1.
```

Example 3:

```text
Input: s = "abcaba"
Output: 1
Explanation: The longest special substring which occurs thrice is "a": substrings "abcaba", "abcaba", and "abcaba".
It can be shown that the maximum length achievable is 1.
```

__Constraints:__

- `3 <= s.length <= 50`
- `s` consists of only lowercase English letters.

__题目描述:__

给你一个仅由小写英文字母组成的字符串 `s` 。

如果一个字符串仅由单一字符组成，那么它被称为 __特殊__ 字符串。例如，字符串 `"abc"` 不是特殊字符串，而字符串 `"ddd"`、 `"zz"` 和 `"f"` 是特殊字符串。

返回在 `s` 中出现 __至少三次__ 的 __最长特殊子字符串__ 的长度，如果不存在出现至少三次的特殊子字符串，则返回 `-1` 。

子字符串 是字符串中的一个连续 非空 字符序列。

__示例:__

示例 1：

```text
输入：s = "aaaa"
输出：2
解释：出现三次的最长特殊子字符串是 "aa" ：子字符串 "aaaa"、"aaaa" 和 "aaaa"。
可以证明最大长度是 2 。
```

示例 2：

```text
输入：s = "abcdef"
输出：-1
解释：不存在出现至少三次的特殊子字符串。因此返回 -1 。
```

示例 3：

```text
输入：s = "abcaba"
输出：1
解释：出现三次的最长特殊子字符串是 "a" ：子字符串 "abcaba"、"abcaba" 和 "abcaba"。
可以证明最大长度是 1 。
```

__提示：__

- `3 <= s.length <= 50`
- `s` 仅由小写英文字母组成。

__思路:__

```text
暴力法
对每段相同的字符分组
统计每一段的长度
如果长度大于 2 就更新结果
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumLength(string s) 
    {
        int n = s.size(), result = -1;
        unordered_map<string, int> m;
        for (int i = 0; i < n; i++) 
        {
            string cur(1, s[i]);
            ++m[cur];
            for (int j = i + 1; j < n; j++) 
            {
                if (s[j] != s[i]) break;
                ++m[cur += s[j]];
            }
        }
        for (const auto& [k, v] : m) if (v > 2) result = max(result, (int)k.size());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumLength(String s) {
        int n = s.length(), result = -1;
        var map = new HashMap<String, Integer>();
        for (int i = 0; i < n; i++) {
            var sb = new StringBuilder().append(s.charAt(i));
            map.merge(sb.toString(), 1, Integer::sum);
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(j) != s.charAt(i)) break;
                sb.append(s.charAt(j));
                map.merge(sb.toString(), 1, Integer::sum);
            }
        }
        for (var entry : map.entrySet()) if (entry.getValue() > 2) result = Math.max(result, entry.getKey().length());
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumLength(self, s: str) -> int:
        return max([len(k) for k, v in Counter([s[i:j + 1] for i in range(len(s)) for j in range(i, len(s)) if len(set(s[i:j + 1])) == 1]).items() if v > 2], default=-1)
```
