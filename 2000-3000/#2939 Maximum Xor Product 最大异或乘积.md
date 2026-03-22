# 2939 Maximum Xor Product 最大异或乘积

__Description:__

Given three integers `a`, `b`, and `n`, return _the __maximum value__ of_ `(a XOR x) * (b XOR x)` _where_ `0 <= x < 2 ^ n`.

Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Note__ that `XOR` is the bitwise XOR operation.

__Example:__

Example 1:

```text
Input: a = 12, b = 5, n = 4
Output: 98
Explanation: For x = 2, (a XOR x) = 14 and (b XOR x) = 7. Hence, (a XOR x) * (b XOR x) = 98. 
It can be shown that 98 is the maximum value of (a XOR x) * (b XOR x) for all 0 <= x < 2 ^ n.
```

Example 2:

```text
Input: a = 6, b = 7 , n = 5
Output: 930
Explanation: For x = 25, (a XOR x) = 31 and (b XOR x) = 30. Hence, (a XOR x) * (b XOR x) = 930.
It can be shown that 930 is the maximum value of (a XOR x) * (b XOR x) for all 0 <= x < 2 ^ n.
```

Example 3:

```text
Input: a = 1, b = 6, n = 3
Output: 12
Explanation: For x = 5, (a XOR x) = 4 and (b XOR x) = 3. Hence, (a XOR x) * (b XOR x) = 12.
It can be shown that 12 is the maximum value of (a XOR x) * (b XOR x) for all 0 <= x < 2 ^ n.
```

__Constraints:__

- `0 <= a, b < 2 ^ 50`
- `0 <= n <= 50`

__题目描述:__

给你三个整数 `a` ， `b` 和 `n` ，请你返回 `(a XOR x) * (b XOR x)` 的 __最大值__ 且 `x` 需要满足 `0 <= x < 2 ^ n`。

由于答案可能会很大，返回它对 `10 ^ 9 + 7` __取余__ 后的结果。

__注意__， `XOR` 是按位异或操作。

__示例:__

示例 1：

```text
输入：a = 12, b = 5, n = 4
输出：98
解释：当 x = 2 时，(a XOR x) = 14 且 (b XOR x) = 7 。所以，(a XOR x) * (b XOR x) = 98 。
98 是所有满足 0 <= x < 2 ^ n 中 (a XOR x) * (b XOR x) 的最大值。
```

示例 2：

```text
输入：a = 6, b = 7 , n = 5
输出：930
解释：当 x = 25 时，(a XOR x) = 31 且 (b XOR x) = 30 。所以，(a XOR x) * (b XOR x) = 930 。
930 是所有满足 0 <= x < 2 ^ n 中 (a XOR x) * (b XOR x) 的最大值。
```

示例 3：

```text
输入：a = 1, b = 6, n = 3
输出：12
解释： 当 x = 5 时，(a XOR x) = 4 且 (b XOR x) = 3 。所以，(a XOR x) * (b XOR x) = 12 。
12 是所有满足 0 <= x < 2 ^ n 中 (a XOR x) * (b XOR x) 的最大值。
```

__提示：__

- `0 <= a, b < 2 ^ 50`
- `0 <= n <= 50`

__思路:__

```text
位运算
不妨设 a >= b
对于 a 和 b 都为 1 或都为 0 的位置 x 只要取相反的值就能增加乘积
对于 a 和 b 不相等的位
可以转化为 a + b 是定值, 如何分配这些 1 的位置使得 a * b 最大
根据基本不等式 a 和 b 最接近的时候最大
所以把最高位分配给 a 其他位全部分配给 b 可以使 a * b 最大
首先先判断 a 和 b 的大小, 令 a 为较大值
令 mask 为 n 位二进制的最大值, 即 mask = (1 << n) - 1
因为 x 不大于 mask, 所以影响不到 a 和 b 的高位
令 ax = a & ~mask, bx = b & ~mask, 即 ax 和 bx 是将低于 n 位全部清零, 同时保持高位
令 am = a & mask, bm = b & mask, 即 a 和 b 的低位
令 left = am ^ bm, left 就为可以分配的位
令 one = mask ^ left, one 为固定为 1 的位置
令 ao = ax | one, bo = bx | one, ao 和 bo 分别为 a 和 b 的高位以及固定为 one 的和
如果 left 为 0 那么不需要分配直接返回 ao * bo % MOD 即可
否则将 left 的最高位分配给 ao
剩下的全部分配给 bo
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumXorProduct(long long a, long long b, int n) 
    {
        if (a < b) swap(a, b);
        long long mask = (1LL << n) - 1LL, ax = a & ~mask, bx = b & ~mask, am = a & mask, bm = b & mask, left = am ^ bm, one = mask ^ left, ao = ax | one, bo = bx | one, MOD = 1e9 + 7LL;
        if (left > 0LL and ao == bo) 
        {
            long high_bit = 1LL << (63LL - __builtin_clzll(left));
            ao |= high_bit;
            left ^= high_bit;
        }
        return (int)((ao % MOD) * ((bo | left) % MOD) % MOD);
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumXorProduct(long a, long b, int n) {
        if (a < b) {
            a ^= b;
            b ^= a;
            a ^= b;
        }
        long mask = (1L << n) - 1L, ax = a & ~mask, bx = b & ~mask, am = a & mask, bm = b & mask, left = am ^ bm, one = mask ^ left, ao = ax | one, bo = bx | one, MOD = 1_000_000_007L;
        if (left > 0L && ao == bo) {
            long highBit = 1L << (63 - Long.numberOfLeadingZeros(left));
            ao |= highBit;
            left ^= highBit;
        }
        return (int)((ao % MOD) * ((bo | left) % MOD) % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def maximumXorProduct(self, a: int, b: int, n: int) -> int:
        return self.maximumXorProduct(b, a, n) if a < b else 0 if (ax := a & ~(mask := (1 << n) - 1)) == -1 or (bx := b & ~mask) == -1 or (am := a & mask) == -1 or (bm := b & mask) == -1 else (ax | one) * (bx | one | left) % (10 ** 9 + 7) if (one := mask ^ (left := am ^ bm)) == -1 or not (left > 0 and ax == bx) else (ax | one | (1 << (left.bit_length() - 1))) * (bx | one | (left ^ 1 << (left.bit_length() - 1))) % (10 ** 9 + 7)
```
