# 1808 Maximize Number of Nice Divisors 好因子的最大数目

__Description:__

You are given a positive integer `primeFactors`. You are asked to construct a positive integer `n` that satisfies the following conditions:

- The number of prime factors of `n` (not necessarily distinct) is __at most__ `primeFactors`.
- The number of nice divisors of `n` is maximized. Note that a divisor of `n` is __nice__ if it is divisible by every prime factor of `n`. For example, if `n = 12`, then its prime factors are `[2,2,3]`, then `6` and `12` are nice divisors, while `3` and `4` are not.

Return _the number of nice divisors of_ `n`. Since that number can be too large, return it __modulo__ `10 ^ 9 + 7`.

Note that a prime number is a natural number greater than `1` that is not a product of two smaller natural numbers. The prime factors of a number `n` is a list of prime numbers such that their product equals `n`.

__Example:__

Example 1:

```text
Input: primeFactors = 5
Output: 6
Explanation: 200 is a valid value of n.
It has 5 prime factors: [2,2,2,5,5], and it has 6 nice divisors: [10,20,40,50,100,200].
There is not other value of n that has at most 5 prime factors and more nice divisors.
```

Example 2:

```text
Input: primeFactors = 8
Output: 18
```

__Constraints:__

- `1 <= primeFactors <= 10 ^ 9`

__题目描述:__

给你一个正整数 `primeFactors` 。你需要构造一个正整数 `n` ，它满足以下条件:

- `n` 质因数（质因数需要考虑重复的情况）的数目 __不超过__ `primeFactors` 个。
- `n` 好因子的数目最大化。如果 `n` 的一个因子可以被 `n` 的每一个质因数整除，我们称这个因子是 __好因子__ 。比方说，如果 `n = 12` ，那么它的质因数为 `[2,2,3]` ，那么 `6` 和 `12` 是好因子，但 `3` 和 `4` 不是。

请你返回 `n` 的好因子的数目。由于答案可能会很大，请返回答案对 `10 ^ 9 + 7` _取余_ 的结果。

请注意，一个质数的定义是大于 `1` ，且不能被分解为两个小于该数的自然数相乘。一个数 `n` 的质因子是将 `n` 分解为若干个质因子，且它们的乘积为 `n` 。

__示例:__

示例 1：

```text
输入：primeFactors = 5
输出：6
解释：200 是一个可行的 n 。
它有 5 个质因子：[2,2,2,5,5] ，且有 6 个好因子：[10,20,40,50,100,200] 。
不存在别的 n 有至多 5 个质因子，且同时有更多的好因子。
```

示例 2：

```text
输入：primeFactors = 8
输出：18
```

__提示：__

- `1 <= primeFactors <= 10 ^ 9`

__思路:__

```text
数学
好因子的种类与题目无关
即求 a1 + a2 + a3 + ... + an = primeFactors 时
max(a1 * a2 * a3 * ... * an)
即将 primeFactors 拆分成最大乘积
尽可能的多拆分成 3
按照对 3 取余的结果进行分类讨论
利用快速幂求解
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxNiceDivisors(int primeFactors) 
    {
        return primeFactors < 5 ? primeFactors : helper(primeFactors / 3 - (primeFactors % 3 == 1)) * (primeFactors % 3 == 1 ? 4 : (primeFactors % 3 == 2 ? 2 : 1)) % MOD;
    }
private:
    static constexpr int MOD = 1e9 + 7;

    long helper(int n)
    {
        long result = 1, p = 3;
        while (n) 
        {
            if (n & 1) result = result * p % MOD;
            p = p * p % MOD;
            n >>= 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private final int MOD = 1_000_000_007;

    public int maxNiceDivisors(int primeFactors) {
        return primeFactors < 5 ? primeFactors : (int)(helper(primeFactors / 3 - (primeFactors % 3 == 1 ? 1 : 0)) * (primeFactors % 3 == 1 ? 4 : (primeFactors % 3 == 2 ? 2 : 1)) % MOD);
    }

    private long helper(int n) {
        long result = 1, p = 3;
        while (n > 0) {
            if ((n & 1) > 0) result = result * p % MOD;
            p = p * p % MOD;
            n >>= 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxNiceDivisors(self, primeFactors: int) -> int:
        return (primeFactors - 3 * max(0, (primeFactors - 2) // 3)) * pow(3, int(max(0, (primeFactors - 2) // 3)), 10 ** 9 + 7) % (10 ** 9 + 7)
```
