# 2571 Minimum Operations to Reduce an Integer to 0 将整数减少到零需要的最少操作数

__Description:__

You are given a positive integer `n`, you can do the following operation __any__ number of times:

- Add or subtract a __power__ of `2` from `n`.

Return _the __minimum__ number of operations to make_ `n` _equal to_ `0`.

A number `x` is power of `2` if `x == 2 ^ i` where `i >= 0`_._

__Example:__

Example 1:

```text
Input: n = 39
Output: 3
Explanation: We can do the following operations:
```

- Add 20 = 1 to n, so now n = 40.
- Subtract 23 = 8 from n, so now n = 32.
- Subtract 25 = 32 from n, so now n = 0.

It can be shown that 3 is the minimum number of operations we need to make n equal to 0.

Example 2:

```text
Input: n = 54
Output: 3
Explanation: We can do the following operations:
```

- Add 21 = 2 to n, so now n = 56.
- Add 23 = 8 to n, so now n = 64.
- Subtract 26 = 64 from n, so now n = 0.

So the minimum number of operations is 3.

__Constraints:__

- `1 <= n <= 10 ^ 5`

__题目描述:__

给你一个正整数 `n` ，你可以执行下述操作 __任意__ 次:

- `n` 加上或减去 `2` 的某个 __幂__

返回使 `n` 等于 `0` 需要执行的 __最少__ 操作数。

如果 `x == 2 ^ i` 且其中 `i >= 0` ，则数字 `x` 是 `2` 的幂。

__示例:__

示例 1：

```text
输入：n = 39
输出：3
解释：我们可以执行下述操作：
```

- n 加上 20 = 1 ，得到 n = 40 。
- n 减去 23 = 8 ，得到 n = 32 。
- n 减去 25 = 32 ，得到 n = 0 。

可以证明使 n 等于 0 需要执行的最少操作数是 3 。

示例 2：

```text
输入：n = 54
输出：3
解释：我们可以执行下述操作：
```

- n 加上 21 = 2 ，得到 n = 56 。
- n 加上 23 = 8 ，得到 n = 64 。
- n 减去 26 = 64 ，得到 n = 0 。

使 n 等于 0 需要执行的最少操作数是 3 。

__提示：__

- `1 <= n <= 10 ^ 5`

__思路:__

```text
位运算
要么减去 lowbit(mask & -mask)
要么加上 lowbit
如果有连续个 1 那么加上 lowbit 比减去 lowbit 更优
注意到 n ^ 3 * n 中的 1 的个数就是需要的操作数
直接返回即可
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minOperations(int n) 
    {
        return __builtin_popcount(3 * n ^ n);
    }
};
```

__Java__:

```Java
class Solution {
    public int minOperations(int n) {
        return Integer.bitCount(3 * n ^ n);
    }
}
```

__Python__:

```Python
class Solution:
    def minOperations(self, n: int) -> int:
        return (3 * n ^ n).bit_count()
```
