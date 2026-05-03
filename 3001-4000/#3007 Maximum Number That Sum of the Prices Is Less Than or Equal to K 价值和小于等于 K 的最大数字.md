# 3007 Maximum Number That Sum of the Prices Is Less Than or Equal to K 价值和小于等于 K 的最大数字

__Description:__

You are given an integer `k` and an integer `x`. The price of a number `num` is calculated by the count of set bits at positions `x`, `2x`, `3x`, etc., in its binary representation, starting from the least significant bit. The following table contains examples of how price is calculated.

| x | num | Binary Representation | Price |
| - | --- | --------------------- | ----- |
| 1 | 013 | 000001101 | 3 |
| 2 | 013 | 000001101 | 1 |
| 2 | 233 | 011101001 | 3 |
| 3 | 013 | 000001101 | 1 |
| 3 | 362 | 101101010 | 2 |

The __accumulated price__ of `num` is the _total_ price of numbers from `1` to `num`. `num` is considered __cheap__ if its accumulated price is less than or equal to `k`.

Return the greatest cheap number.

__Example:__

Example 1:

```text
Input: k = 9, x = 1

Output: 6

Explanation:
```

As shown in the table below, `6` is the greatest cheap number.

| x | num | Binary Representation | Price | Accumulated Price |
| - | --- | --------------------- | ----- | ---------------- |
| 1 | 1 | 001 | 3 | 01 |
| 1 | 2 | 010 | 1 | 02 |
| 1 | 3 | 011 | 3 | 04 |
| 1 | 4 | 100 | 1 | 05 |
| 1 | 5 | 101 | 2 | 07 |
| 1 | 6 | 110 | 2 | 09 |
| 1 | 7 | 111 | 2 | 12 |

Example 2:

```text
Input: k = 7, x = 2

Output: 9

Explanation:
```

As shown in the table below, `9` is the greatest cheap number.

| x | num | Binary Representation | Price | Accumulated Price |
| - | --- | --------------------- | ----- | ---------------- |
| 2 | 01 | 0001 | 0 | 0 |
| 2 | 02 | 0010 | 1 | 1 |
| 2 | 03 | 0011 | 1 | 2 |
| 2 | 04 | 0100 | 0 | 2 |
| 2 | 05 | 0101 | 0 | 2 |
| 2 | 06 | 0110 | 1 | 3 |
| 2 | 07 | 0111 | 1 | 4 |
| 2 | 08 | 1000 | 1 | 5 |
| 2 | 09 | 1001 | 1 | 6 |
| 2 | 10 | 1010 | 2 | 8 |

__Constraints:__

- `1 <= k <= 10 ^ 15`
- `1 <= x <= 8`

__题目描述:__

给你一个整数 `k` 和一个整数 `x` 。整数 `num` 的价值是它的二进制表示中在 `x`， `2x`， `3x` 等位置处 __设置位__ 的数目（从最低有效位开始）。下面的表格包含了如何计算价值的例子。

| x | num | Binary Representation | Price |
| - | --- | --------------------- | ----- |
| 1 | 013 | 000001101 | 3 |
| 2 | 013 | 000001101 | 1 |
| 2 | 233 | 011101001 | 3 |
| 3 | 013 | 000001101 | 1 |
| 3 | 362 | 101101010 | 2 |

`num` 的 __累加价值__ 是从 `1` 到 `num` 的数字的 __总__ 价值。如果 `num` 的累加价值小于或等于 `k` 则被认为是 __廉价__ 的。

请你返回 最大 的廉价数字。

__示例:__

示例 1：

```text
输入：k = 9, x = 1
输出：6
解释：由下表所示，6 是最大的廉价数字。
```

| x | num | Binary Representation | Price | Accumulated Price |
| - | --- | --------------------- | ----- | ---------------- |
| 1 | 1 | 001 | 3 | 01 |
| 1 | 2 | 010 | 1 | 02 |
| 1 | 3 | 011 | 3 | 04 |
| 1 | 4 | 100 | 1 | 05 |
| 1 | 5 | 101 | 2 | 07 |
| 1 | 6 | 110 | 2 | 09 |
| 1 | 7 | 111 | 2 | 12 |

示例 2：

```text
输入：k = 7, x = 2
输出：9
解释：由下表所示，9 是最大的廉价数字。
```

| x | num | Binary Representation | Price | Accumulated Price |
| - | --- | --------------------- | ----- | ---------------- |
| 2 | 01 | 0001 | 0 | 0 |
| 2 | 02 | 0010 | 1 | 1 |
| 2 | 03 | 0011 | 1 | 2 |
| 2 | 04 | 0100 | 0 | 2 |
| 2 | 05 | 0101 | 0 | 2 |
| 2 | 06 | 0110 | 1 | 3 |
| 2 | 07 | 0111 | 1 | 4 |
| 2 | 08 | 1000 | 1 | 5 |
| 2 | 09 | 1001 | 1 | 6 |
| 2 | 10 | 1010 | 2 | 8 |

__提示：__

- `1 <= k <= 10 ^ 15`
- `1 <= x <= 8`

__思路:__

```text
1. 二分查找
对于求 1 到 mid 上的所有第 x 位的倍数的 1 和个数之和
由于最低位从 0 开始
i 从 x - 1 开始计算
对于 n = mid >> i 位计算其有几个奇数, 每一个奇数提供 1 个 1
一共有 n >> 1 个奇数这部分的和为 n >> 1 << i
如果 n 是奇数前缀贡献了 (mid & mask) + 1 个 1, 其中 mask = (1 << i) - 1
时间复杂度为 O(lgK ^ 2 / X), 空间复杂度为 O(1)
2. 构造法
从高到低构建 num
如果当前遍历到从低到高的 i 位
i 左边有 pre1 个编号是 x 的倍数且为 1 的位
如果第 i 位填 1
那么一共增加 pre * (1 << i) + (i / x) * (1 << (i - 1))
如果当前总共的 1 少于 k 那么这一位就能填 1
时间复杂度为 O(lgK + X), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long findMaximumNumber(long long k, int x) 
    {
        long long num = 0LL, pre1 = 0LL, cur = 0LL;
        for (long long i = __lg((k + 1) << x); i > -1LL; i--) 
        {
            if ((cur = (pre1 << i) + (i / x << i >> 1LL)) <= k) 
            {
                k -= cur;
                num |= 1LL << i;
                pre1 += !((i + 1) % x);
            }
        }
        return num - 1LL;
    }
};
```

__Java__:

```Java
class Solution {
    public long findMaximumNumber(long k, int x) {
        long left = 0L, right = (k + 1L) << x, mid = 0L;
        while (left + 1 < right) {
            if (helper(mid = (left + right) >>> 1L, x, k)) left = mid;
            else right = mid;
        }
        return left;
    }

    private boolean helper(long mid, int x, long k) {
        long cur = 0L, n = 0L;
        for (int i = x - 1; (n = (mid >> i)) > 0L; i += x) cur += (n >> 1L << i) + (n & 1L) * ((mid & ((1L << i) - 1L)) + 1L);
        return cur <= k;
    }
}
```

__Python__:

```Python
class Solution:
    def findMaximumNumber(self, k: int, x: int) -> int:
        left, right = 0, 10 ** 20

        def check(mid: int) -> bool:
            i, cur = x - 1, 0
            while (n := (mid >> i)):
                cur += (n >> 1 << i) + (n & 1) * ((mid & ((1 << i) - 1)) + 1)
                i += x
            return cur <= k
        while left + 1 < right:
            if check(mid := left + ((right - left) >> 1)):
                left = mid
            else:
                right = mid
        return left
```
