# 3130 Find All Possible Stable Binary Arrays II 找出所有稳定的二进制数组 II

__Description:__

You are given 3 positive integers `num_zeros`, `num_ones`, and `limit`.

A binary array `arr` is called __stable__ if:

- The number of occurrences of 0 in `arr` is __exactly__ `num_zeros`.
- The number of occurrences of 1 in `arr` is __exactly__ `num_ones`.
- Each subarray of `arr` with a size greater than `limit` must contain __at least__ one occurrence of __both__ 0 and 1.

Return an integer denoting the total number of stable binary arrays.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: zero = 1, one = 1, limit = 2

Output: 2

Explanation:
```

The two possible stable binary arrays are `[1,0]` and `[0,1]`.

Example 2:

```text
Input: zero = 1, one = 2, limit = 1

Output: 1

Explanation:
```

The only possible stable binary array is `[1,0,1]`.

Example 3:

```text
Input: zero = 3, one = 3, limit = 2

Output: 14

Explanation:
```

All the possible stable binary arrays are `[0,0,1,0,1,1]`, `[0,0,1,1,0,1]`, `[0,1,0,0,1,1]`, `[0,1,0,1,0,1]`, `[0,1,0,1,1,0]`, `[0,1,1,0,0,1]`, `[0,1,1,0,1,0]`, `[1,0,0,1,0,1]`, `[1,0,0,1,1,0]`, `[1,0,1,0,0,1]`, `[1,0,1,0,1,0]`, `[1,0,1,1,0,0]`, `[1,1,0,0,1,0]`, and `[1,1,0,1,0,0]`.

__Constraints:__

- `1 <= zero, one, limit <= 1000`

__题目描述:__

给你 3 个正整数 `num_zeros` ， `num_ones` 和 `limit` 。

一个 二进制数组 `arr` 如果满足以下条件，那么我们称它是 __稳定的__ :

- 0 在 `arr` 中出现次数 __恰好__ 为 `num_zeros`。
- 1 在 `arr` 中出现次数 __恰好__ 为 `num_ones`。
- `arr` 中每个长度超过 `limit` 的 子数组 都 __同时__ 包含 0 和 1 。

请你返回一个整数，表示 稳定 二进制数组的 总 数目。

由于答案可能很大，将它对 `10 ^ 9 + 7` _取余_ 后返回。

__示例:__

示例 1：

```text
输入：zero = 1, one = 1, limit = 2

输出：2

解释：
```

两个稳定的二进制数组为 `[1,0]` 和 `[0,1]` ，两个数组都有一个 0 和一个 1 ，且没有子数组长度大于 2 。

示例 2：

```text
输入：zero = 1, one = 2, limit = 1

输出：1

解释：
```

唯一稳定的二进制数组是 `[1,0,1]` 。

二进制数组 `[1,1,0]` 和 `[0,1,1]` 都有长度为 2 且元素全都相同的子数组，所以它们不稳定。

示例 3：

```text
输入：zero = 3, one = 3, limit = 2

输出：14

解释：
```

所有稳定的二进制数组包括 `[0,0,1,0,1,1]` ， `[0,0,1,1,0,1]` ， `[0,1,0,0,1,1]` ， `[0,1,0,1,0,1]` ， `[0,1,0,1,1,0]` ， `[0,1,1,0,0,1]` ， `[0,1,1,0,1,0]` ， `[1,0,0,1,0,1]` ， `[1,0,0,1,1,0]` ， `[1,0,1,0,0,1]` ， `[1,0,1,0,1,0]` ， `[1,0,1,1,0,0]` ， `[1,1,0,0,1,0]` 和 `[1,1,0,1,0,0]` 。

__提示：__

- `1 <= zero, one, limit <= 1000`

__思路:__

```text
动态规划
设 dp[i + 1][j + 1][0/1] 表示使用了 i 个 0 和 j 个 1 且结尾为 0/1 的稳定二进制数组的个数
初始化 dp[i][0][0] = 1, i <= min(limit, zero), dp[0][j][1], j <= min(limit, one)
对于任意一个 dp[i][j]
如果结尾可以接 0, 那么枚举从上一个 1 到现在使用了几个 0, 之前所有的 dp[i - limit][j][1] 到 dp[i - 1][j][1] 都可以接 0, 所以需要求出 sum(dp[i - k][j][1]), 1 <= k <= limit, 可以使用前缀和优化
同理求 dp[i][j][1] 即为 sum(dp[i][j - k][0]), 也可以使用前缀和优化
时间复杂度为 O(MN), 空间复杂度为 O(MN), M 和 N 分别为 0 和 1 的个数
```

__代码:__

__C++__:

```C++
int dp[1005][1005][2];
int pre[1005];
constexpr int MOD = 1e9 + 7;
class Solution 
{
public:
    int numberOfStableArrays(int zero, int one, int limit) 
    {
        for (int i = 0; i <= zero; i++) dp[i][0][0] = i <= limit;
        for (int i = 0; i <= zero; i++) dp[i][0][1] = 0;
        for (int j = 0; j <= one; j++) dp[0][j][1] = pre[j] = j <= limit;
        for (int j = 0; j <= one; j++) dp[0][j][0] = 0;
        for (int i = 1; i <= zero; i++)
        {
            for (int j = 1, cur = dp[i][0][0]; j <= one; j++)
            {
                dp[i][j][0] = pre[j];
                dp[i][j][1] = cur;
                cur = ((cur + dp[i][j][0]) % MOD - (j >= limit ? dp[i][j - limit][0] : 0) + MOD) % MOD;
                pre[j] = ((pre[j] + dp[i][j][1]) % MOD - (i >= limit ? dp[i - limit][j][1] : 0) + MOD) % MOD;
            }
        }
        return (dp[zero][one][0] + dp[zero][one][1]) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfStableArrays(int zero, int one, int limit) {
        int dp[][][] = new int[zero + 1][one + 1][2], pre[] = new int[one + 1], MOD = 1_000_000_007;
        for (int i = 0; i <= Math.min(limit, zero); i++) dp[i][0][0] = 1;
        for (int j = 0; j <= Math.min(limit, one); j++) dp[0][j][1] = pre[j] = 1;
        for (int i = 1; i <= zero; i++) {
            for (int j = 1, cur = dp[i][0][0]; j <= one; j++) {
                dp[i][j] = new int[]{ pre[j], cur };
                cur = ((cur + dp[i][j][0]) % MOD - (j >= limit ? dp[i][j - limit][0] : 0) + MOD) % MOD;
                pre[j] = ((pre[j] + dp[i][j][1]) % MOD - (i >= limit ? dp[i - limit][j][1] : 0) + MOD) % MOD;
            }
        }
        return (dp[zero][one][0] + dp[zero][one][1]) % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfStableArrays(self, zero: int, one: int, limit: int) -> int:
        dp, pre = [[[0] * 2 for _ in range(one + 1)] for _ in range(zero + 1)], [0] * (one + 1)
        for i in range(min(zero, limit) + 1):
            dp[i][0][0] = 1
        for j in range(min(one, limit) + 1):
            dp[0][j][1] = pre[j] = 1
        for i in range(1, zero + 1):
            cur = dp[i][0][0]
            for j in range(1, one + 1):
                dp[i][j] = [pre[j], cur]
                cur += dp[i][j][0] - (dp[i][j - limit][0] if j >= limit else 0)
                pre[j] += dp[i][j][1] - (dp[i - limit][j][1] if i >= limit else 0)
        return sum(dp[zero][one]) % (10 ** 9 + 7)
```
