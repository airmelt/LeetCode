# 2338 Count the Number of Ideal Arrays 统计理想数组的数目

__Description:__

You are given two integers `n` and `maxValue`, which are used to describe an __ideal__ array.

A __0-indexed__ integer array `arr` of length `n` is considered __ideal__ if the following conditions hold:

- Every `arr[i]` is a value from `1` to `maxValue`, for `0 <= i < n`.
- Every `arr[i]` is divisible by `arr[i - 1]`, for `0 < i < n`.

Return _the number of __distinct__ ideal arrays of length_ `n`. Since the answer may be very large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 2, maxValue = 5
Output: 10
Explanation: The following are the possible ideal arrays:
```

- Arrays starting with the value 1 (5 arrays): [1,1], [1,2], [1,3], [1,4], [1,5]
- Arrays starting with the value 2 (2 arrays): [2,2], [2,4]
- Arrays starting with the value 3 (1 array): [3,3]
- Arrays starting with the value 4 (1 array): [4,4]
- Arrays starting with the value 5 (1 array): [5,5]

There are a total of 5 + 2 + 1 + 1 + 1 = 10 distinct ideal arrays.

Example 2:

```text
Input: n = 5, maxValue = 3
Output: 11
Explanation: The following are the possible ideal arrays:
```

- Arrays starting with the value 1 (9 arrays):
  - With no other distinct values (1 array): [1,1,1,1,1]
  - With 2nd distinct value 2 (4 arrays): [1,1,1,1,2], [1,1,1,2,2], [1,1,2,2,2], [1,2,2,2,2]
  - With 2nd distinct value 3 (4 arrays): [1,1,1,1,3], [1,1,1,3,3], [1,1,3,3,3], [1,3,3,3,3]
- Arrays starting with the value 2 (1 array): [2,2,2,2,2]
- Arrays starting with the value 3 (1 array): [3,3,3,3,3]

There are a total of 9 + 1 + 1 = 11 distinct ideal arrays.

__Constraints:__

- `2 <= n <= 10 ^ 4`
- `1 <= maxValue <= 10 ^ 4`

__题目描述:__

给你两个整数 `n` 和 `maxValue` ，用于描述一个 __理想数组__ 。

对于下标从 __0__ 开始、长度为 `n` 的整数数组 `arr` ，如果满足以下条件，则认为该数组是一个 __理想数组__ :

- 每个 `arr[i]` 都是从 `1` 到 `maxValue` 范围内的一个值，其中 `0 <= i < n` 。
- 每个 `arr[i]` 都可以被 `arr[i - 1]` 整除，其中 `0 < i < n` 。

返回长度为 `n` 的 __不同__ 理想数组的数目。由于答案可能很大，返回对 `10 ^ 9 + 7` 取余的结果。

__示例:__

示例 1：

```text
输入：n = 2, maxValue = 5
输出：10
解释：存在以下理想数组：
```

- 以 1 开头的数组（5 个）：[1,1]、[1,2]、[1,3]、[1,4]、[1,5]
- 以 2 开头的数组（2 个）：[2,2]、[2,4]
- 以 3 开头的数组（1 个）：[3,3]
- 以 4 开头的数组（1 个）：[4,4]
- 以 5 开头的数组（1 个）：[5,5]

共计 5 + 2 + 1 + 1 + 1 = 10 个不同理想数组。

示例 2：

```text
输入：n = 5, maxValue = 3
输出：11
解释：存在以下理想数组：
```

- 以 1 开头的数组（9 个）：
  - 不含其他不同值（1 个）：[1,1,1,1,1]
  - 含一个不同值 2（4 个）：[1,1,1,1,2], [1,1,1,2,2], [1,1,2,2,2], [1,2,2,2,2]
  - 含一个不同值 3（4 个）：[1,1,1,1,3], [1,1,1,3,3], [1,1,3,3,3], [1,3,3,3,3]
- 以 2 开头的数组（1 个）：[2,2,2,2,2]
- 以 3 开头的数组（1 个）：[3,3,3,3,3]

共计 9 + 1 + 1 = 11 个不同理想数组。

__提示：__

- `2 <= n <= 10 ^ 4`
- `1 <= maxValue <= 10 ^ 4`

__思路:__

```text
动态规划 ➕ 数学
考虑以数字 x 结尾的数组的个数
对于每个 x, 计算其所有的因子, 以及因子的个数
比如对于 n = 4, x = 4, 则其因子为 2, 以及因子的个数为 2
对于 n = 4, 有
[1, 1, 1, 4]
[1, 1, 2, 4]
[1, 1, 4, 4]
[1, 2, 2, 4]
[1, 2, 4, 4]
[1, 4, 4, 4]
[2, 2, 2, 4]
[2, 2, 4, 4]
[2, 4, 4, 4]
[4, 4, 4, 4]
共 10 个
实际上是 C(n + k - 1, k) 的组合数
因为对于结尾数字 4, 有 2 个因子 2, 这两个因子放且必须放在 4 的前面, 也就是说 对于 [1, 1, 1, 1, 4] 这样的情况, 两个 2 需要放到 4 的前面, 用隔板法 C(n + k - 1, k) 来计算
计算组合数可以用动态规划来计算
c[i][j] = c[i - 1][j] + c[i - 1][j - 1], 这是由于对 c[i][j] 来说是 i 里取 j 个, 既可以从 i - 1 个取 j 个来实现, 也可以从 i - 1 个取 j - 1 个, 然后再加上 i 这个数
由于 n 的范围为 10 ^ 4, maxValue 的范围为 10 ^ 4, 因此可以先预处理出每个数的因子
取最大数为 MX = 10 ^ 4 + 1, 最大因子数不会超过 13, 2 ** 13 = 8192
再预处理组合数
对于每一个 x, 计算其所有的因子, 以及因子的个数 k, ks[x]
累加 C(n + k - 1, k) 即可
预处理时间复杂度为 O(MloglogM)
时间复杂度为 O(MloglogM), 空间复杂度为 O(M), M 为 maxValue
```

__代码:__

__C++__:

```C++
const int MOD = 1e9 + 7, MX = 1e4 + 1, K = 13;
vector<int> ks[MX];
int c[MX + K][K + 1];

int init = []() 
{
    for (int i = 1, x = 1; i < MX; x = ++i) 
    {
        for (int p = 2; p * p <= x; p++) 
        {
            if (!(x % p)) 
            {
                int k = 1;
                for (x /= p; !(x % p); x /= p) ++k;
                ks[i].push_back(k);
            }
        }
        if (x > 1) ks[i].push_back(1);
    }

    c[0][0] = 1;
    for (int i = 1; i < MX + K; ++i) 
    {
        c[i][0] = 1;
        for (int j = 1; j <= min(i, K); j++) c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % MOD;
    }
    return 0;
}();

class Solution 
{
public:
    int idealArrays(int n, int maxValue) 
    {
        long long result = 0LL;
        for (int x = 1; x <= maxValue; x++) 
        {
            long long cur = 1LL;
            for (int k: ks[x]) cur = cur * c[n + k - 1][k] % MOD;
            result = (result + cur) % MOD;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    static final int MOD = 1_000_000_007, MX = 10_001, K = 13;
    static List[] ks = new List[MX];
    static int[][] c = new int[MX + K][K + 1];

    static {
        for (int i = 1, x = 1; i < MX; x = ++i) {
            ks[i] = new ArrayList<Integer>();
            for (int p = 2; p * p <= x; p++) {
                if (x % p == 0) {
                    int k = 1;
                    for (x /= p; x % p == 0; x /= p) ++k;
                    ks[i].add(k);
                }
            }
            if (x > 1) ks[i].add(1);
        }

        c[0][0] = 1;
        for (int i = 1; i < MX + K; i++) {
            c[i][0] = 1;
            for (int j = 1; j <= Math.min(i, K); j++) c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % MOD;
        }
    }

    public int idealArrays(int n, int maxValue) {
        long result = 0L;
        for (int x = 1; x <= maxValue; x++) {
            long cur = 1L;
            for (var k : ks[x]) cur = cur * c[n + (int)k - 1][(int)k] % MOD;
            result = (result + cur) % MOD;
        }
        return (int)result;
    }
}
```

__Python__:

```Python
MOD, MX = 10 ** 9 + 7, 10 ** 4 + 1
ks = [[] for _ in range(MX)]
for i in range(2, MX):
    p, x = 2, i
    while p * p <= x:
        if not x % p:
            k = 1
            x //= p
            while not x % p:
                k += 1
                x //= p
            ks[i].append(k)
        p += 1
    if x > 1: 
        ks[i].append(1)
print(ks)

class Solution:
    def idealArrays(self, n: int, maxValue: int) -> int:
        return sum(reduce(lambda a, b: a * comb(n + b - 1, b) % MOD, [1] + ks[x]) for x in range(1, maxValue + 1)) % MOD
```
