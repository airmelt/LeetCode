# 2954 Count the Number of Infection Sequences 统计感冒序列的数目

__Description:__

You are given an integer `n` and an array `sick` sorted in increasing order, representing positions of infected people in a line of `n` people.

At each step, one uninfected person adjacent to an infected person gets infected. This process continues until everyone is infected.

An infection sequence is the order in which uninfected people become infected, excluding those initially infected.

Return the number of different infection sequences possible, modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 5, sick = [0,4]

Output: 4

Explanation:

There is a total of 6 different sequences overall.
```

- Valid infection sequences are `[1,2,3]`, `[1,3,2]`, `[3,2,1]` and `[3,1,2]`.
- `[2,3,1]` and `[2,1,3]` are not valid infection

sequences because the person at index 2 cannot be infected at the first step.

Example 2:

```text
Input: n = 4, sick = [1]

Output: 3

Explanation:

There is a total of 6 different sequences overall.
```

- Valid infection sequences are `[0,2,3]`, `[2,0,3]` and `[2,3,0]`.
- `[3,2,0]`, `[3,0,2]`, and `[0,3,2]` are not valid

infection sequences because the infection starts at the person at index 1, then the order of infection is 2, then 3, and hence 3 cannot be infected earlier than 2.

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= sick.length <= n - 1`
- `0 <= sick[i] <= n - 1`
- `sick` is sorted in increasing order.

__题目描述:__

给你一个整数 `n` 和一个下标从 __0__ 开始的整数数组 `sick` ，数组按 __升序__ 排序。

有 `n` 位小朋友站成一排，按顺序编号为 `0` 到 `n - 1` 。数组 `sick` 包含一开始得了感冒的小朋友的位置。如果位置为 `i` 的小朋友得了感冒，他会传染给下标为 `i - 1` 或者 `i + 1` 的小朋友，__前提__ 是被传染的小朋友存在且还没有得感冒。每一秒中， __至多一位__ 还没感冒的小朋友会被传染。

经过有限的秒数后，队列中所有小朋友都会感冒。感冒序列 指的是 所有 一开始没有感冒的小朋友最后得感冒的顺序序列。请你返回所有感冒序列的数目。

由于答案可能很大，请你将答案对 `10 ^ 9 + 7` 取余后返回。

注意，感冒序列 不 包含一开始就得了感冒的小朋友的下标。

__示例:__

示例 1：

```text
输入：n = 5, sick = [0,4]
输出：4
解释：一开始，下标为 1 ，2 和 3 的小朋友没有感冒。总共有 4 个可能的感冒序列：
```

- 一开始，下标为 1 和 3 的小朋友可以被传染，因为他们分别挨着有感冒的小朋友 0 和 4 ，令下标为 1 的小朋友先被传染。
然后，下标为 2 的小朋友挨着感冒的小朋友 1 ，下标为 3 的小朋友挨着感冒的小朋友 4 ，两位小朋友都可以被传染，令下标为 2 的小朋友被传染。
最后，下标为 3 的小朋友被传染，因为他挨着感冒的小朋友 2 和 4 ，感冒序列为 [1,2,3] 。
- 一开始，下标为 1 和 3 的小朋友可以被传染，因为他们分别挨着感冒的小朋友 0 和 4 ，令下标为 1 的小朋友先被传染。
然后，下标为 2 的小朋友挨着感冒的小朋友 1 ，下标为 3 的小朋友挨着感冒的小朋友 4 ，两位小朋友都可以被传染，令下标为 3 的小朋友被传染。
最后，下标为 2 的小朋友被传染，因为他挨着感冒的小朋友 1 和 3 ，感冒序列为  [1,3,2] 。
- 感冒序列 [3,1,2] ，被传染的顺序：[0,1,2,3,4] => [0,1,2,3,4] => [0,1,2,3,4] => [0,1,2,3,4] 。
- 感冒序列 [3,2,1] ，被传染的顺序：[0,1,2,3,4] => [0,1,2,3,4] => [0,1,2,3,4] => [0,1,2,3,4] 。

示例 2：

```text
输入：n = 4, sick = [1]
输出：3
解释：一开始，下标为 0 ，2 和 3 的小朋友没有感冒。总共有 3 个可能的感冒序列：
```

- 感冒序列 [0,2,3] ，被传染的顺序：[0,1,2,3] => [0,1,2,3] => [0,1,2,3] => [0,1,2,3] 。
- 感冒序列 [2,0,3] ，被传染的顺序：[0,1,2,3] => [0,1,2,3] => [0,1,2,3] => [0,1,2,3] 。
- 感冒序列 [2,3,0] ，被传染的顺序：[0,1,2,3] => [0,1,2,3] => [0,1,2,3] => [0,1,2,3] 。

__提示：__

- `2 <= n <= 10 ^ 5`
- `1 <= sick.length <= n - 1`
- `0 <= sick[i] <= n - 1`
- `sick` 按升序排列。

__思路:__

```text
数学
如果两个感冒的小朋友相邻
无事发生
当两个感冒的小朋友之间有 k 个小朋友时
这 k 个小朋友既能被左边的感染也能被右边的感染
一共有 2 ^ (k - 1) 种排列
将所有人排序一共有 (n - m)! 种排序方式
每一组相邻的感冒的人要去掉 k! 种
可以用逆元来除 k!
最后用乘法原理将上面的个数都累乘
中间可以用快速幂简化运算
时间复杂度为 O(M), 空间复杂度为 O(1), 预处理时间不计入, M 为 sick 数组长度
```

__代码:__

__C++__:

```C++
constexpr int MOD = 1e9 + 7;
constexpr int MX = 1e5;

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

long long fac[MX], inv[MX];

auto init = [] 
{
    fac[0] = 1;
    for (int i = 1; i < MX; i++) fac[i] = fac[i - 1] * i % MOD;
    inv[MX - 1] = quick_pow(fac[MX - 1], MOD - 2);
    for (int i = MX - 1; i > 0; i--) inv[i - 1] = inv[i] * i % MOD;
    return 0;
}();

class Solution 
{
public:
    int numberOfSequence(int n, vector<int>& sick) 
    {
        int m = sick.size(), e = 0;
        long long result = fac[n - m] * inv[sick.front()] % MOD * inv[n - 1 - sick.back()] % MOD;
        for (int i = 0, k = 0; i < m - 1; i++) 
        {
            if ((k = sick[i + 1] - sick[i] - 1) > 1) 
            {
                e += k - 1;
                result = result * inv[k] % MOD;
            }
        }
        return result * quick_pow(2, e) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MOD = 1_000_000_007;
    private static final int MX = 100_000;

    private static final long[] FAC = new long[MX];
    private static final long[] INV = new long[MX];

    static {
        FAC[0] = 1;
        for (int i = 1; i < MX; i++) FAC[i] = FAC[i - 1] * i % MOD;
        INV[MX - 1] = pow(FAC[MX - 1], MOD - 2);
        for (int i = MX - 1; i > 0; i--) INV[i - 1] = INV[i] * i % MOD;
    }

    public int numberOfSequence(int n, int[] sick) {
        int m = sick.length, e = 0;
        long result = FAC[n - m] * INV[sick[0]] % MOD * INV[n - 1 - sick[m - 1]] % MOD;
        for (int i = 1, k = 0; i < m; i++) {
            if ((k = sick[i] - sick[i - 1] - 1) > 1) {
                e += k - 1;
                result = result * INV[k] % MOD;
            }
        }
        return (int) (result * pow(2, e) % MOD);
    }

    private static long pow(long a, int n) {
        long result = 1L;
        for (; n > 0; n >>= 1) {
            if ((n & 1) > 0) result = result * a % MOD;
            a = a * a % MOD;
        }
        return result;
    }
}
```

__Python__:

```Python
MOD = 10 ** 9 + 7
MX = 10 ** 5

fac = [0] * MX
fac[0] = 1
for i in range(1, MX):
    fac[i] = fac[i - 1] * i % MOD

inv = [0] * MX
inv[MX - 1] = pow(fac[MX - 1], -1, MOD)
for i in range(MX - 1, 0, -1):
    inv[i - 1] = inv[i] * i % MOD


class Solution:
    def numberOfSequence(self, n: int, sick: List[int]) -> int:
        result, e = fac[n - len(sick)] * inv[sick[0]] * inv[n - 1 - sick[-1]] % MOD, 0
        for x, y in pairwise(sick):
            if (k := y - x - 1) > 1:
                e += k - 1
                result = result * inv[k] % MOD
        return result * pow(2, e, MOD) % MOD 
```
