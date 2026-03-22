# 2311 Longest Binary Subsequence Less Than or Equal to K 小于等于 K 的最长二进制子序列

__Description:__

You are given a binary string `s` and a positive integer `k`.

Return _the length of the __longest__ subsequence of_ `s` _that makes up a __binary__ number less than or equal to_ `k`.

Note:

- The subsequence can contain __leading zeroes__.
- The empty string is considered to be equal to `0`.
- A __subsequence__ is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

__Example:__

Example 1:

```text
Input: s = "1001010", k = 5
Output: 5
Explanation: The longest subsequence of s that makes up a binary number less than or equal to 5 is "00010", as this number is equal to 2 in decimal.
Note that "00100" and "00101" are also possible, which are equal to 4 and 5 in decimal, respectively.
The length of this subsequence is 5, so 5 is returned.
```

Example 2:

```text
Input: s = "00101001", k = 1
Output: 6
Explanation: "000001" is the longest subsequence of s that makes up a binary number less than or equal to 1, as this number is equal to 1 in decimal.
The length of this subsequence is 6, so 6 is returned.
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s[i]` is either `'0'` or `'1'`.
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个二进制字符串 `s` 和一个正整数 `k` 。

请你返回 `s` 的 __最长__ 子序列，且该子序列对应的 __二进制__ 数字小于等于 `k` 。

注意：

- 子序列可以有 __前导 0__ 。
- 空字符串视为 `0` 。
- __子序列__ 是指从一个字符串中删除零个或者多个字符后，不改变顺序得到的剩余字符序列。

__示例:__

示例 1：

```text
输入：s = "1001010", k = 5
输出：5
解释：s 中小于等于 5 的最长子序列是 "00010" ，对应的十进制数字是 2 。
注意 "00100" 和 "00101" 也是可行的最长子序列，十进制分别对应 4 和 5 。
最长子序列的长度为 5 ，所以返回 5 。
```

示例 2：

```text
输入：s = "00101001", k = 1
输出：6
解释："000001" 是 s 中小于等于 1 的最长子序列，对应的十进制数字是 1 。
最长子序列的长度为 6 ，所以返回 6 。
```

__提示：__

- `1 <= s.length <= 1000`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。
- `1 <= k <= 10 ^ 9`

__思路:__

```text
贪心
前导 0 不会增加子序列的二进制大小, 所以我们要尽可能的保留前导 0
从后往前找到一个不超过 k 的最长后缀
设 k 的二进制长度为 m
如果 n < m, 那么答案就是 n
如果 s 的长为 m 的后缀不超过 k, 那么可以这个后缀加上前面的 0
否则, 可以用 m - 1 长度的后缀加上前面的 0
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSubsequence(string s, int k) 
    {
        return s.size() < 32 - __builtin_clz(k) ? s.size() : (stoi(s.substr(s.size() - 32 + __builtin_clz(k)), nullptr, 2) <= k ? 32 - __builtin_clz(k) : 31 - __builtin_clz(k)) + count(s.begin(), s.end() + __builtin_clz(k) - 32, '0');
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubsequence(String s, int k) {
        return s.length() < 32 - Integer.numberOfLeadingZeros(k) ? s.length() : (Integer.parseInt(s.substring(s.length() - 32 + Integer.numberOfLeadingZeros(k)), 2) <= k ? 32 - Integer.numberOfLeadingZeros(k) : 31 - Integer.numberOfLeadingZeros(k)) + (int)s.substring(0, s.length() - 32 + Integer.numberOfLeadingZeros(k)).chars().filter(c -> c == '0').count();
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubsequence(self, s: str, k: int) -> int:
        return n if (n := len(s)) < (m := k.bit_length()) else (m if int(s[-m:], 2) <= k else m - 1) + s.count('0', 0, -m)
```
