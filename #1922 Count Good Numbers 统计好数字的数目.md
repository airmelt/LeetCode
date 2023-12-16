# 1922 Count Good Numbers 统计好数字的数目

__Description:__

A digit string is __good__ if the digits __(0-indexed)__ at __even__ indices are __even__ and the digits at __odd__ indices are __prime__ ( `2`, `3`, `5`, or `7`).

- For example, `"2582"` is good because the digits ( `2` and `8`) at even positions are even and the digits ( `5` and `2`) at odd positions are prime. However, `"3245"` is __not__ good because `3` is at an even index but is not even.

Given an integer `n`, return _the __total__ number of good digit strings of length_ `n`. Since the answer may be large, __return it modulo__ `10 ^ 9 + 7`.

A __digit string__ is a string consisting of digits `0` through `9` that may contain leading zeros.

__Example:__

Example 1:

```text
Input: n = 1
Output: 5
Explanation: The good numbers of length 1 are "0", "2", "4", "6", "8".
```

Example 2:

```text
Input: n = 4
Output: 400
```

Example 3:

```text
Input: n = 50
Output: 564908303
```

__Constraints:__

- `1 <= n <= 10 ^ 15`

__题目描述:__

我们称一个数字字符串是 __好数字__ 当它满足（下标从 __0__ 开始）__偶数__ 下标处的数字为 __偶数__ 且 __奇数__ 下标处的数字为 __质数__ （ `2`， `3`， `5` 或 `7`）。

- 比方说， `"2582"` 是好数字，因为偶数下标处的数字（ `2` 和 `8`）是偶数且奇数下标处的数字（ `5` 和 `2`）为质数。但 `"3245"` __不是__ 好数字，因为 `3` 在偶数下标处但不是偶数。

给你一个整数 `n` ，请你返回长度为 `n` 且为好数字的数字字符串 __总数__ 。由于答案可能会很大，请你将它对 `10 ^ 9 + 7` __取余后返回__ 。

一个 __数字字符串__ 是每一位都由 `0` 到 `9` 组成的字符串，且可能包含前导 0 。

__示例:__

示例 1：

```text
输入：n = 1
输出：5
解释：长度为 1 的好数字包括 "0"，"2"，"4"，"6"，"8" 。
```

示例 2：

```text
输入：n = 4
输出：400
```

示例 3：

```text
输入：n = 50
输出：564908303
```

__提示：__

- `1 <= n <= 10 ^ 15`

__思路:__

```text
数学
奇数位可以使用 2, 3, 5, 7, 一共有 n >> 1 个奇数位
偶数位可以使用 2, 4, 6, 8, 0, 一共有 (n + 1) >> 1 个偶数位
所以需要求 5 ^ ((n + 1) >> 1) * 4 ^ (n >> 1) 再取余
计算幂需要用快速幂加快计算速度
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countGoodNumbers(long long n) 
    {
        return helper(5, (n + 1) >> 1) * helper(4, n >> 1) % MOD;
    }
private:
    static constexpr int MOD = 1e9 + 7;

    long long helper(long long a, long long n) 
    {
        long long result = 1;
        while (n > 0) 
        {
            if (n & 1) result = result * a % MOD;
            a = a * a % MOD;
            n >>= 1; 
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countGoodNumbers(long n) {
        return (int)(helper(5, (n + 1) >> 1) * helper(4, n >> 1) % 1_000_000_007L);
    }

    private long helper(long a, long n) {
        long result = 1, MOD = 1_000_000_007L;
        while (n > 0) {
            if ((n & 1) == 1) result = result * a % MOD;
            a = a * a % MOD;
            n >>>= 1; 
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countGoodNumbers(self, n: int) -> int:
        return pow(5, (n + 1) >> 1, 10 ** 9 + 7) * pow(4, n >> 1, 10 ** 9 + 7) % (10 ** 9 + 7)
```
