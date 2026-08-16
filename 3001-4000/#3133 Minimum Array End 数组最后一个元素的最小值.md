# 3133 Minimum Array End 数组最后一个元素的最小值

__Description:__

You are given two integers `n` and `x`. You have to construct an array of positive integers `nums` of size `n` where for every `0 <= i < n - 1`, `nums[i + 1]` is greater than `nums[i]`, and the result of the bitwise `AND` operation between all elements of `nums` is `x`.

Return the minimum possible value of `nums[n - 1]`.

__Example:__

Example 1:

```text
Input: n = 3, x = 4

Output: 6

Explanation:

nums can be [4,5,6] and its last element is 6.
```

Example 2:

```text
Input: n = 2, x = 7

Output: 15

Explanation:

nums can be [7,15] and its last element is 15.
```

__Constraints:__

- `1 <= n, x <= 10 ^ 8`

__题目描述:__

给你两个整数 `n` 和 `x` 。你需要构造一个长度为 `n` 的 正整数 数组 `nums` ，对于所有 `0 <= i < n - 1` ，满足 `nums[i + 1]` 大于 `nums[i]` ，并且数组 `nums` 中所有元素的按位 `AND` 运算结果为 `x` 。

返回 `nums[n - 1]` 可能的 __最小__ 值。

__示例:__

示例 1：

```text
输入：n = 3, x = 4

输出：6

解释：

数组 nums 可以是 [4,5,6] ，最后一个元素为 6 。
```

示例 2：

```text
输入：n = 2, x = 7

输出：15

解释：

数组 nums 可以是 [7,15] ，最后一个元素为 15 。
```

__提示：__

- `1 <= n, x <= 10 ^ 8`

__思路:__

```text
位运算
数组中最小的数必须是 x
否则位运算之后会小于 x
由于 nums[n - 1] 需要尽可能小
那么从 x 开始依次增加二进制位 1 能够取得最小的数字
一种方式是将 x 所有的空位(即 0)依次填上 n - 1 的二进制位
剩下的都放在 x 的开头
优化方式是只取 n - 1 的 1 的位(lowbit)依次填入 x 的空位
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long minEnd(int n, int x) 
    {
        long long result = x, cur = ~x, lowbit = 0LL;
        for (int j = 0, a = n - 1; a >> j; cur ^= lowbit) result |= (long long)(lowbit = cur & -cur) * (a >> j++ & 1LL);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long minEnd(int n, int x) {
        long result = x, cur = ~x, lowbit = 0L;
        for (int j = 0, a = n - 1; (a >> j) > 0; cur ^= lowbit) result |= (long)(lowbit = cur & -cur) * (a >> j++ & 1L);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minEnd(self, n: int, x: int) -> int:
        i, j, m, p = 0, 0, len(a := list(bin(x)[2::])[::-1]), len(b := list(bin(n - 1)[2:])[::-1])
        while i < m and j < p:
            if a[i] == '0':
                a[i] = b[j]
                j += 1
            i += 1
        return int(''.join((a + b[j:])[::-1]), 2)
```
