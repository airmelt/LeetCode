# 3179 Find the N-th Value After K Seconds K 秒后第 N 个元素的值

__Description:__

You are given two integers `n` and `k`.

Initially, you start with an array `a` of `n` integers where `a[i] = 1` for all `0 <= i <= n - 1`. After each second, you simultaneously update each element to be the sum of all its preceding elements plus the element itself. For example, after one second, `a[0]` remains the same, `a[1]` becomes `a[0] + a[1]`, `a[2]` becomes `a[0] + a[1] + a[2]`, and so on.

Return the __value__ of `a[n - 1]` after `k` seconds.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 4, k = 5

Output: 56

Explanation:
```

| Second | State After |
| ------ | ----------- |
| `0` | `[1,1,1,1]` |
| `1` | `[1,2,3,4]` |
| `2` | `[1,3,6,10]` |
| `3` | `[1,4,10,20]` |
| `4` | `[1,5,15,35]` |
| `5` | `[1,6,21,56]` |

Example 2:

```text
Input: n = 5, k = 3

Output: 35

Explanation:
```

| Second | State After |
| ------ | ----------- |
| `0` | `[1,1,1,1,1]` |
| `1` | `[1,2,3,4,5]` |
| `2` | `[1,3,6,10,15]` |
| `3` | `[1,4,10,20,35]` |

__Constraints:__

- `1 <= n, k <= 1000`

__题目描述:__

给你两个整数 `n` 和 `k`。

最初，你有一个长度为 `n` 的整数数组 `a`，对所有 `0 <= i <= n - 1`，都有 `a[i] = 1` 。每过一秒，你会同时更新每个元素为其前面所有元素的和加上该元素本身。例如，一秒后，`a[0]` 保持不变，`a[1]` 变为 `a[0] + a[1]`，`a[2]` 变为 `a[0] + a[1] + a[2]`，以此类推。

返回 `k` 秒后 `a[n - 1]` 的 __值__。

由于答案可能非常大，返回其对 `10 ^ 9 + 7` __取余__ 后的结果。

__示例:__

示例 1：

```text
输入：n = 4, k = 5

输出：56

解释：
```

| 时间（秒） | 数组状态 |
| ------ | ----------- |
| `0` | `[1,1,1,1]` |
| `1` | `[1,2,3,4]` |
| `2` | `[1,3,6,10]` |
| `3` | `[1,4,10,20]` |
| `4` | `[1,5,15,35]` |
| `5` | `[1,6,21,56]` |

示例 2：

```text
输入：n = 5, k = 3

输出：35

解释：
```

| 时间（秒） | 数组状态 |
| ------ | ----------- |
| `0` | `[1,1,1,1,1]` |
| `1` | `[1,2,3,4,5]` |
| `2` | `[1,3,6,10,15]` |
| `3` | `[1,4,10,20,35]` |

__提示：__

- `1 <= n, k <= 1000`

__思路:__

```text
数学
实际上就是返回 comb(n + k - 1, k)
时间复杂度为 O(1), 空间复杂度为 O(1), 预处理不计入
```

__代码:__

__C++__:

```C++
constexpr int MOD = 1e9 + 7;
constexpr int MX = 2001;

long long fac[MX], inv[MX];

long long quick_pow(long long a, int n)
{
    long long result = 1LL;
    for (; n; n >>= 1)
    {
        if (n & 1) result = result * a % MOD;
        a = a * a % MOD;
    }
    return result;
}

auto init = []
{
    fac[0] = 1LL;
    for (int i = 1; i < MX; i++) fac[i] = fac[i - 1] * i % MOD;
    inv[MX - 1] = quick_pow(fac[MX - 1], MOD - 2);
    for (int i = MX - 1; i > 0; i--) inv[i - 1] = inv[i] * i % MOD;
    return 0;
}();

class Solution 
{
public:
    int valueAfterKSeconds(int n, int k) 
    {
        return comb(n + k - 1, k);
    }
private:
    long long comb(int n, int k)
    {
        return fac[n] * inv[k] % MOD * inv[n - k] % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MOD = 1_000_000_007;
    private static final int MX = 2001;

    private static final long[] FAC = new long[MX];
    private static final long[] INV = new long[MX];

    static {
        FAC[0] = 1L;
        for (int i = 1; i < MX; i++) FAC[i] = FAC[i - 1] * i % MOD;
        INV[MX - 1] = quickPow(FAC[MX - 1], MOD - 2);
        for (int i = MX - 1; i > 0; i--) INV[i - 1] = INV[i] * i % MOD;
    }

    private static long comb(int n, int k) {
        return FAC[n] * INV[k] % MOD * INV[n - k] % MOD;
    }

    private static long quickPow(long a, int n) {
        long result = 1L;
        for (; n > 0; n >>>= 1) {
            if ((n & 1) > 0) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }

    public int valueAfterKSeconds(int n, int k) {
        return (int)comb(n + k - 1, k);
    }
}
```

__Python__:

```Python
class Solution:
    def valueAfterKSeconds(self, n: int, k: int) -> int:
        return comb(n + k - 1, k) % (10 ** 9 + 7)
```
