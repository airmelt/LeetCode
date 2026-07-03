# 3084 Count Substrings Starting and Ending with Given Character 统计以给定字符开头和结尾的子字符串总数

__Description:__

You are given a string `s` and a character `c`. Return _the total number of substrings of_ `s` _that start and end with_ `c`_._

__Example:__

Example 1:

```text
Input: s = "abada", c = "a"

Output: 6
```

__Explanation:__ Substrings starting and ending with "a" are: "__a__ bada", "__aba__ da", "__abada__", "ab __a__ da", "ab __ada__", "abad __a__".

Example 2:

```text
Input: s = "zzz", c = "z"

Output: 6
```

__Explanation:__ There are a total of `6` substrings in `s` and all start and end with "z".

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` and `c` consist only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个字符 `c` 。返回在字符串 `s` 中并且以 `c` 字符开头和结尾的非空子字符串的总数。

__示例:__

示例 1：

```text
输入：s = "abada", c = "a"

输出：6
```

__解释:__
以 "a" 开头和结尾的子字符串有: "__a__ bada"、 "__aba__ da"、 "__abada__"、 "ab __a__ da"、 "ab __ada__"、 "abad __a__"。

示例 2：

```text
输入：s = "zzz", c = "z"

输出：6
```

__解释:__
字符串 `s` 中总共有 `6` 个子字符串，并且它们都以 "z" 开头和结尾。

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 和 `c` 均由小写英文字母组成。

__思路:__

```text
数学
计算 c 在 s 中出现的次数 a
返回 a * (1 + a) / 2 即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countSubstrings(string s, char c) 
    {
        return count_if(s.begin(), s.end(), [&](const auto& ch) { return ch == c; }) * (count_if(s.begin(), s.end(), [&](const auto& ch) { return ch == c; }) + 1LL) >> 1LL;
    }
};
```

__Java__:

```Java
class Solution {
    public long countSubstrings(String s, char c) {
        return s.chars().filter(i -> i == c).count() * (s.chars().filter(i -> i == c).count() + 1L) >>> 1L;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubstrings(self, s: str, c: str) -> int:
        return (a := s.count(c)) * (a + 1) >> 1
```
