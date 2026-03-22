# 1689 Partitioning Into Minimum Number Of Deci-Binary Numbers 十-二进制数的最少数目

__Description:__

A decimal number is called __deci-binary__ if each of its digits is either `0` or `1` without any leading zeros. For example, `101` and `1100` are __deci-binary__, while `112` and `3001` are not.

Given a string `n` that represents a positive decimal integer, return _the __minimum__ number of positive __deci-binary__ numbers needed so that they sum up to_ `n`_._

__Example:__

Example 1:

```text
Input: n = "32"
Output: 3
Explanation: 10 + 11 + 11 = 32
```

Example 2:

```text
Input: n = "82734"
Output: 8
```

Example 3:

```text
Input: n = "27346209830709182346"
Output: 9
```

__Constraints:__

- `1 <= n.length <= 10 ^ 5`
- `n` consists of only digits.
- `n` does not contain any leading zeros and represents a positive integer.

__题目描述:__

如果一个十进制数字不含任何前导零，且每一位上的数字不是 `0` 就是 `1` ，那么该数字就是一个 __十-二进制数__ 。例如， `101` 和 `1100` 都是 __十-二进制数__，而 `112` 和 `3001` 不是。

给你一个表示十进制整数的字符串 `n` ，返回和为 `n` 的 __十-二进制数__ 的最少数目。

__示例:__

示例 1：

```text
输入：n = "32"
输出：3
解释：10 + 11 + 11 = 32
```

示例 2：

```text
输入：n = "82734"
输出：8
```

示例 3：

```text
输入：n = "27346209830709182346"
输出：9
```

__提示：__

- `1 <= n.length <= 10 ^ 5`
- `n` 仅由数字组成
- `n` 不含任何前导零并总是表示正整数

__思路:__

```text
数学
因为十-二进制数中每一位上的数字不是 0 就是 1, 所以最少的 十-二进制数 的个数就是 n 中最大的数字
时间复杂度为 O(N), 空间复杂度为 O(1), N 为 n 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minPartitions(string n) 
    {
        int result = 0;
        for (const auto& c : n) result = max(c - '0', result);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minPartitions(String n) {
        int result = 0;
        for (char c : n.toCharArray()) result = Math.max(c - '0', result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minPartitions(self, n: str) -> int:
        return int(max(n))
```
