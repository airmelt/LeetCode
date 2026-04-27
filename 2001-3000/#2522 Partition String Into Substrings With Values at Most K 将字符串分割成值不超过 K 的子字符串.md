# 2522 Partition String Into Substrings With Values at Most K 将字符串分割成值不超过 K 的子字符串

__Description:__

You are given a string `s` consisting of digits from `1` to `9` and an integer `k`.

A partition of a string `s` is called __good__ if:

- Each digit of `s` is part of __exactly__ one substring.
- The value of each substring is less than or equal to `k`.

Return _the __minimum__ number of substrings in a __good__ partition of_ `s`. If no __good__ partition of `s` exists, return `-1`.

Note that:

- The __value__ of a string is its result when interpreted as an integer. For example, the value of `"123"` is `123` and the value of `"1"` is `1`.
- A __substring__ is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "165462", k = 60
Output: 4
Explanation: We can partition the string into substrings "16", "54", "6", and "2". Each substring has a value less than or equal to k = 60.
It can be shown that we cannot partition the string into less than 4 substrings.
```

Example 2:

```text
Input: s = "238182", k = 5
Output: -1
Explanation: There is no good partition for this string.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` is a digit from `'1'` to `'9'`.
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个字符串 `s` ，它每一位都是 `1` 到 `9` 之间的数字组成，同时给你一个整数 `k` 。

如果一个字符串 `s` 的分割满足以下条件，我们称它是一个 __好__ 分割:

- `s` 中每个数位 __恰好__ 属于一个子字符串。
- 每个子字符串的值都小于等于 `k` 。

请你返回 `s` 所有的 __好__ 分割中，子字符串的 __最少__ 数目。如果不存在 `s` 的 __好__ 分割，返回 `-1` 。

注意：

- 一个字符串的 __值__ 是这个字符串对应的整数。比方说， `"123"` 的值为 `123` ， `"1"` 的值是 `1` 。
- __子字符串__ 是字符串中一段连续的字符序列。

__示例:__

示例 1：

```text
输入：s = "165462", k = 60
输出：4
解释：我们将字符串分割成子字符串 "16" ，"54" ，"6" 和 "2" 。每个子字符串的值都小于等于 k = 60 。
不存在小于 4 个子字符串的好分割。
```

示例 2：

```text
输入：s = "238182", k = 5
输出：-1
解释：这个字符串不存在好分割。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` 是 `'1'` 到 `'9'` 之间的数字。
- `1 <= k <= 10 ^ 9`

__思路:__

```text
贪心
由于是要分割子字符串
贪心的将字符放入子字符串中
只要当前字符加上之前的子字符串的值小于等于 k 就可以
如果当前字符大于 k 直接返回 -1
如果当前字符加上之前的子字符串的值大于 k
分割一个新的子字符串
将当前字符放入新的子字符串中
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumPartition(string s, int k) 
    {
        long long result = 1LL, cur = 0LL;
        for (const auto& c : s)
        {
            if (c - '0' > k) return -1;
            cur = 10LL * cur + c - '0';
            if (cur > k) 
            {
                ++result;
                cur = c - '0';
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumPartition(String s, int k) {
        long result = 1L, cur = 0L;
        for (char c : s.toCharArray()) {
            if (c - '0' > k) return -1;
            cur = 10L * cur + c - '0';
            if (cur > k) {
                ++result;
                cur = c - '0';
            }
        }
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumPartition(self, s: str, k: int) -> int:
        result, cur = 1, 0
        for c in map(int, s):
            if c > k:
                return -1
            cur = 10 * cur + c
            if cur > k:
                result += 1
                cur = c
        return result
```
