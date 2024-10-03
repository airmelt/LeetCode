# 2272 Substring With Largest Variance 最大波动的子字符串

__Description:__

The __variance__ of a string is defined as the largest difference between the number of occurrences of __any__ `2` characters present in the string. Note the two characters may or may not be the same.

Given a string `s` consisting of lowercase English letters only, return _the __largest variance__ possible among all __substrings__ of_ `s`.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "aababbb"
Output: 3
Explanation:
All possible variances along with their respective substrings are listed below:
```

- Variance 0 for substrings "a", "aa", "ab", "abab", "aababb", "ba", "b", "bb", and "bbb".
- Variance 1 for substrings "aab", "aba", "abb", "aabab", "ababb", "aababbb", and "bab".
- Variance 2 for substrings "aaba", "ababbb", "abbb", and "babb".
- Variance 3 for substring "babbb".

Since the largest possible variance is 3, we return it.

Example 2:

```text
Input: s = "abcde"
Output: 0
Explanation:
No letter occurs more than once in s, so the variance of every substring is 0.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 4`
- `s` consists of lowercase English letters.

__题目描述:__

字符串的 波动 定义为子字符串中出现次数 最多 的字符次数与出现次数 最少 的字符次数之差。

给你一个字符串 `s` ，它只包含小写英文字母。请你返回 `s` 里所有 __子字符串的__ __最大波动__ 值。

子字符串 是一个字符串的一段连续字符序列。

__示例:__

示例 1：

```text
输入：s = "aababbb"
输出：3
解释：
所有可能的波动值和它们对应的子字符串如以下所示：
```

- 波动值为 0 的子字符串："a" ，"aa" ，"ab" ，"abab" ，"aababb" ，"ba" ，"b" ，"bb" 和 "bbb" 。
- 波动值为 1 的子字符串："aab" ，"aba" ，"abb" ，"aabab" ，"ababb" ，"aababbb" 和 "bab" 。
- 波动值为 2 的子字符串："aaba" ，"ababbb" ，"abbb" 和 "babb" 。
- 波动值为 3 的子字符串 "babbb" 。

所以，最大可能波动值为 3 。

示例 2：

```text
输入：s = "abcde"
输出：0
解释：
s 中没有字母出现超过 1 次，所以 s 中每个子字符串的波动值都是 0 。
```

__提示：__

- `1 <= s.length <= 10 ^ 4`
- `s` 只包含小写英文字母。

__思路:__

```text
枚举
由于 s 中只有小写字符
遍历所有的字符对 a, b, 计算 a, b 的波动值
遍历 s, 统计 a, b 的出现次数
初始化 diff = 0, diff_b = INT_MIN
如果 s[i] == a, 则 diff++, diff_b++
如果 s[i] == b, 则 diff--, diff_b = diff, diff = max(diff, 0)
更新 result = max(result, diff_b)
时间复杂度为 O(N), 空间复杂度为 O(1), 实际上需要 O(N * 26 ^ 2) 的时间复杂度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestVariance(string s) 
    {
        int result = 0, n = s.size(), diff = 0, diff_b = INT_MIN;
        for (char a = 'a'; a <= 'z'; a++) 
        {
            for (char b = 'a'; b <= 'z'; b++, diff = 0, diff_b = INT_MIN) 
            {
                if (a != b) 
                {
                    for (int i = 0; i < n; i++, result = max(result, diff_b)) 
                    {
                        if (s[i] == a) 
                        {
                            ++diff;
                            ++diff_b;
                        } 
                        else if 
                        (s[i] == b) 
                        {
                            --diff;
                            diff_b = diff;
                            diff = max(diff, 0);
                        }
                    }
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestVariance(String s) {
        int result = 0, n = s.length(), diff = 0, diffB = Integer.MIN_VALUE;
        for (char a = 'a'; a <= 'z'; a++) {
            for (char b = 'a'; b <= 'z'; b++, diff = 0, diffB = Integer.MIN_VALUE) {
                if (a != b) {
                    for (int i = 0; i < n; i++, result = Math.max(result, diffB)) {
                        if (s.charAt(i) == a) {
                            ++diff;
                            ++diffB;
                        } else if (s.charAt(i) == b) {
                            --diff;
                            diffB = diff;
                            diff = Math.max(diff, 0);
                        }
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestVariance(self, s: str) -> int:
        result = 0
        for a, b in permutations(ascii_lowercase, 2):
            diff, diff_b = 0, -inf
            for c in s:
                if c == a:
                    diff += 1
                    diff_b += 1
                elif c == b:
                    diff -= 1
                    diff_b = diff
                    diff = max(diff, 0)
                result = max(result, diff_b)
        return result
```
