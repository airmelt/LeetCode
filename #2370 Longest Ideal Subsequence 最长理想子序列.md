# 2370 Longest Ideal Subsequence 最长理想子序列

__Description:__

You are given a string `s` consisting of lowercase letters and an integer `k`. We call a string `t` __ideal__ if the following conditions are satisfied:

- `t` is a __subsequence__ of the string `s`.
- The absolute difference in the alphabet order of every two __adjacent__ letters in `t` is less than or equal to `k`.

Return the length of the longest ideal string.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

__Note__ that the alphabet order is not cyclic. For example, the absolute difference in the alphabet order of `'a'` and `'z'` is `25`, not `1`.

__Example:__

Example 1:

```text
Input: s = "acfgbd", k = 2
Output: 4
Explanation: The longest ideal string is "acbd". The length of this string is 4, so 4 is returned.
Note that "acfgbd" is not ideal because 'c' and 'f' have a difference of 3 in alphabet order.
```

Example 2:

```text
Input: s = "abcd", k = 3
Output: 4
Explanation: The longest ideal string is "abcd". The length of this string is 4, so 4 is returned.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `0 <= k <= 25`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个由小写字母组成的字符串 `s` ，和一个整数 `k` 。如果满足下述条件，则可以将字符串 `t` 视作是 __理想字符串__ :

- `t` 是字符串 `s` 的一个子序列。
- `t` 中每两个 __相邻__ 字母在字母表中位次的绝对差值小于或等于 `k` 。

返回 最长 理想字符串的长度。

字符串的子序列同样是一个字符串，并且子序列还满足：可以经由其他字符串删除某些字符（也可以不删除）但不改变剩余字符的顺序得到。

__注意:__字母表顺序不会循环。例如， `'a'` 和 `'z'` 在字母表中位次的绝对差值是 `25` ，而不是 `1` 。

__示例:__

示例 1：

```text
输入：s = "acfgbd", k = 2
输出：4
解释：最长理想字符串是 "acbd" 。该字符串长度为 4 ，所以返回 4 。
注意 "acfgbd" 不是理想字符串，因为 'c' 和 'f' 的字母表位次差值为 3 。
```

示例 2：

```text
输入：s = "abcd", k = 3
输出：4
解释：最长理想字符串是 "abcd" ，该字符串长度为 4 ，所以返回 4 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `0 <= k <= 25`
- `s` 由小写英文字母组成

__思路:__

```text
动态规划
设 dp[c] 表示以字符 c 结尾的最长理想字符串的长度
那么 dp[c] 可由 dp[c - k], dp[c - k + 1], ..., dp[c + k] 转移而来, 其中 c - k >= 0, c + k <= 25
dp[c] = 1 + max(dp[c - k], dp[c - k + 1], ..., dp[c + k])
最后返回 dp 中的最大值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestIdealString(string s, int k) 
    {
        int dp[26]{};
        for (const auto& c : s) dp[c - 'a'] = 1 + *max_element(dp + max(c - 'a' - k, 0), dp + min(c - 'a' + k + 1, 26));
        return ranges::max(dp);
    }
};
```

__Java__:

```Java
class Solution {
    public int longestIdealString(String s, int k) {
        int dp[] = new int[26];
        for (char c : s.toCharArray()) {
            for (int j = c - 'a', i = Math.max(j - k, 0); i <= Math.min(j + k, 25); i++) dp[j] = Math.max(dp[j], dp[i]);
            ++dp[c - 'a'];
        }
        return Arrays.stream(dp).max().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def longestIdealString(self, s: str, k: int) -> int:
        dp = [0] * 26
        for c in s:
            dp[c] = 1 + max(dp[max((c := ord(c) - ord('a')) - k, 0):c + k + 1])
        return max(dp)
```
