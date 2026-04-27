# 2318 Number of Distinct Roll Sequences 不同骰子序列的数目

__Description:__

You are given an integer `n`. You roll a fair 6-sided dice `n` times. Determine the total number of __distinct__ sequences of rolls possible such that the following conditions are satisfied:

Return _the __total number__ of distinct sequences possible_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

Two sequences are considered distinct if at least one element is different.

__Example:__

Example 1:

```text
Input: n = 4
Output: 184
Explanation: Some of the possible sequences are (1, 2, 3, 4), (6, 1, 2, 3), (1, 2, 3, 1), etc.
Some invalid sequences are (1, 2, 1, 3), (1, 2, 3, 6).
(1, 2, 1, 3) is invalid since the first and third roll have an equal value and abs(1 - 3) = 2 (i and j are 1-indexed).
(1, 2, 3, 6) is invalid since the greatest common divisor of 3 and 6 = 3.
There are a total of 184 distinct sequences possible, so we return 184.
```

Example 2:

```text
Input: n = 2
Output: 22
Explanation: Some of the possible sequences are (1, 2), (2, 1), (3, 2).
Some invalid sequences are (3, 6), (2, 4) since the greatest common divisor is not equal to 1.
There are a total of 22 distinct sequences possible, so we return 22.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`

__题目描述:__

给你一个整数 `n` 。你需要掷一个 6 面的骰子 `n` 次。请你在满足以下要求的前提下，求出 __不同__ 骰子序列的数目:

请你返回不同序列的 __总数目__ 。由于答案可能很大，请你将答案对 `10 ^ 9 + 7` __取余__ 后返回。

如果两个序列中至少有一个元素不同，那么它们被视为不同的序列。

__示例:__

示例 1：

```text
输入：n = 4
输出：184
解释：一些可行的序列为 (1, 2, 3, 4) ，(6, 1, 2, 3) ，(1, 2, 3, 1) 等等。
一些不可行的序列为 (1, 2, 1, 3) ，(1, 2, 3, 6) 。
(1, 2, 1, 3) 是不可行的，因为第一个和第三个骰子值相等且 abs(1 - 3) = 2 （下标从 1 开始表示）。
(1, 2, 3, 6) i是不可行的，因为 3 和 6 的最大公约数是 3 。
总共有 184 个不同的可行序列，所以我们返回 184 。
```

示例 2：

```text
输入：n = 2
输出：22
解释：一些可行的序列为 (1, 2) ，(2, 1) ，(3, 2) 。
一些不可行的序列为 (3, 6) ，(2, 4) ，因为最大公约数不为 1 。
总共有 22 个不同的可行序列，所以我们返回 22 。
```

__提示：__

- `1 <= n <= 10 ^ 4`

__思路:__

```text
动态规划
设 dp[i][last1][last2] 表示投掷 i 次骰子，最后两次的结果分别为 last1 和 last2 的序列数目
每次投掷骰子，我们需要判断当前的结果是否与上一次的结果及上上次的结果满足条件
需要保证 last1 和 last2 不相等，且 gcd(last1 + 1, last2 + 1) = 1
初始化 dp[2][i][j] = 1, 0 <= i, j < 6, i != j and gcd(i + 1, j + 1) = 1
对于新的结果 dp[i + 1][j][last]
首先 last 和 j 不相等，且 gcd(last + 1, j + 1) = 1
然后遍历上一次的结果 last2, 如果 last2 和 j 不相等，那么 dp[i + 1][j][last] += dp[i][last][last2], 0 <= last2 < 6
最后累计 dp[n] 即可
如果定义 dp[i][j] 表示投掷 i 次骰子，最后一次的结果为 j 的序列数目
那么 dp[i][j] = sum(dp[i - 1][k] - dp[i - 2][j] for k in range(6) for j in range(6) if k != j and gcd(k + 1, j + 1) == 1) + dp[i - 2][j]
这是因为 dp[i][4] 应该从 dp[i - 1][1], dp[i - 1][3], dp[i - 1][5] 转移过来，但是每一个 dp[i - 2][4] 都不能转移过来
所以先要减去 dp[i - 2][4] 的值
而 dp[i - 2][4] 又刚好是 dp[i - 3][1], dp[i - 3][3], dp[i - 3][5] 转移过来的值
所以再加上一个 dp[i - 2][4] 的值即可
时间复杂度为 O(N), 空间复杂度为 O(N), 常数为 6 忽略不计
```

__代码:__

__C++__:

```C++
const int MOD = 1e9 + 7, MX = 1e4;
int dp[MX + 1][6][6];
int init = []()
{
    for (int i = 0; i < 6; i++) for (int j = 0; j < 6; j++) dp[2][i][j] = j != i and gcd(i + 1, j + 1) == 1;
    for (int i = 2; i < MX; i++) for (int j = 0; j < 6; j++) for (int last = 0; last < 6; last++) if (j != last and gcd(last + 1, j + 1) == 1) for (int last2 = 0; last2 < 6; last2++) if (last2 != j) dp[i + 1][j][last] = (dp[i + 1][j][last] + dp[i][last][last2]) % MOD;
    return 0;
}();
class Solution 
{
public:
    int distinctSequences(int n) 
    {
        int result = 0;
        for (int i = 0; i < 6; i++) for (int j = 0; j < 6; j++) result = (result + dp[n][i][j]) % MOD;
        return n == 1 ? 6 : result;
    }
};
```

__Java__:

```Java
class Solution {
    private static final int MOD = 1_000_000_007, MX = 10_000;
    private static final int[][] dp = new int[MX + 1][6];

    static {
        for (int i = 0; i < 6; i++) dp[1][i] = 1;
        for (int i = 2; i <= MX; i++)
            for (int j = 0; j < 6; j++) {
                long s = 0L;
                for (int last = 0; last < 6; last++) if (last != j && gcd(last + 1, j + 1) == 1) s += dp[i - 1][last];
                s -= (long)(i > 3 ? dp[2][j] - 1 : dp[2][j]) * dp[i - 2][j];
                dp[i][j] = (int)(s % MOD);
            }
    }

    private static int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    public int distinctSequences(int n) {
        long result = 0L;
        for (int v : dp[n]) result += v;
        return (int)(result % MOD + MOD) % MOD;
    }
}
```

__Python__:

```Python
MOD, MX = 10 ** 9 + 7, 10 ** 4
dp = [[[0] * 6 for _ in range(6)] for _ in range(MX + 1)]
dp[2] = [[j != i and gcd(j + 1, i + 1) == 1 for j in range(6)] for i in range(6)]
for i in range(2, MX):
    for j in range(6):
        for last in range(6):
            if last != j and gcd(last + 1, j + 1) == 1:
                dp[i + 1][j][last] = sum(dp[i][last][last2] for last2 in range(6) if last2 != j) % MOD

class Solution:
    def distinctSequences(self, n: int) -> int:
        return sum(sum(row) for row in dp[n]) % MOD if n > 1 else 6
```
