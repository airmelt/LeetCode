# 1663 Smallest String With A Given Numeric Value 具有给定数值的最小字符串

__Description:__

The __numeric value__ of a __lowercase character__ is defined as its position `(1-indexed)` in the alphabet, so the numeric value of `a` is `1`, the numeric value of `b` is `2`, the numeric value of `c` is `3`, and so on.

The __numeric value__ of a __string__ consisting of lowercase characters is defined as the sum of its characters' numeric values. For example, the numeric value of the string `"abe"` is equal to `1 + 2 + 5 = 8`.

You are given two integers `n` and `k`. Return _the __lexicographically smallest string__ with __length__ equal to `n` and __numeric value__ equal to `k`._

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

__Example:__

Example 1:

```text
Input: n = 3, k = 27
Output: "aay"
Explanation: The numeric value of the string is 1 + 1 + 25 = 27, and it is the smallest string with such a value and length equal to 3.
```

Example 2:

```text
Input: n = 5, k = 73
Output: "aaszz"
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `n <= k <= 26 * n`

__题目描述:__

__小写字符__ 的 __数值__ 是它在字母表中的位置（从 `1` 开始），因此 `a` 的数值为 `1` ， `b` 的数值为 `2` ， `c` 的数值为 `3` ，以此类推。

字符串由若干小写字符组成，__字符串的数值__ 为各字符的数值之和。例如，字符串 `"abe"` 的数值等于 `1 + 2 + 5 = 8` 。

给你两个整数 `n` 和 `k` 。返回 __长度__ 等于 `n` 且 __数值__ 等于 `k` 的 __字典序最小__ 的字符串。

注意，如果字符串 `x` 在字典排序中位于 `y` 之前，就认为 `x` 字典序比 `y` 小，有以下两种情况:

- `x` 是 `y` 的一个前缀；
- 如果 `i` 是 `x[i] != y[i]` 的第一个位置，且 `x[i]` 在字母表中的位置比 `y[i]` 靠前。

__示例:__

示例 1：

```text
输入：n = 3, k = 27
输出："aay"
解释：字符串的数值为 1 + 1 + 25 = 27，它是数值满足要求且长度等于 3 字典序最小的字符串。
```

示例 2：

```text
输入：n = 5, k = 73
输出："aaszz"
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `n <= k <= 26 * n`

__思路:__

```text
贪心
先将字符串初始化为 n 个 'a'
从后往前遍历，每次将当前位置的字符替换为能够使得 k 减小最多的字符
遍历前 k 减去 n - 1 表示之前已经使用了 n - 1 个 'a'
如果 k > 0, 则说明需要修改，如果 k <= 0, 则说明已经不需要再修改字符, 直接返回
如果 k > 25, 则把最后一个字符直接修改为 'z', 并且 k 减去 26
否则, 把最后一个字符修改为 'a' + k - 1, 并且 k 减去当前字符 - 'a' + 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    string getSmallestString(int n, int k)
    {
        string result(n, 'a');
        for (int i = n - 1, t = k - (n - 1); i > -1 and t > 0; i--) 
        { 
            result[i] = 'a' + min(26, t) - 1;
            t -= result[i] - 'a';
        } 
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public String getSmallestString(int n, int k) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) sb.append('a');
        for (int i = n - 1, t = k - (n - 1); i > -1 && t > 0; i--) { 
            sb.setCharAt(i, (char)('a' + Math.min(26, t) - 1));
            t -= sb.charAt(i) - 'a';
        } 
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def getSmallestString(self, n: int, k: int) -> str:
        result, t, i = ['a'] * n, k - (n - 1), n
        while (i := i - 1) > -1 and t > 0:
            result[i] = chr(ord('a') + min(t, 26) - 1)
            t -= ord(result[i]) - ord('a')
        return ''.join(result)
```
