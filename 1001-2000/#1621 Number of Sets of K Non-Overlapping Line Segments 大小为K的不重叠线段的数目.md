# 1621 Number of Sets of K Non-Overlapping Line Segments 大小为K的不重叠线段的数目

__Description:__

Given `n` points on a 1-D plane, where the `i ^ th` point (from `0` to `n-1`) is at `x = i`, find the number of ways we can draw __exactly__ `k` __non-overlapping__ line segments such that each segment covers two or more points. The endpoints of each segment must have __integral coordinates__. The `k` line segments __do not__ have to cover all `n` points, and they are __allowed__ to share endpoints.

Return _the number of ways we can draw_ `k` _non-overlapping line segments_. Since this number can be huge, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

![1621-1](https://assets.leetcode.com/uploads/2020/09/07/ex1.png)

```text
Input: n = 4, k = 2
Output: 5
Explanation: The two line segments are shown in red and blue.
The image above shows the 5 different ways {(0,2),(2,3)}, {(0,1),(1,3)}, {(0,1),(2,3)}, {(1,2),(2,3)}, {(0,1),(1,2)}.
```

Example 2:

```text
Input: n = 3, k = 1
Output: 3
Explanation: The 3 ways are {(0,1)}, {(0,2)}, {(1,2)}.
```

Example 3:

```text
Input: n = 30, k = 7
Output: 796297179
Explanation: The total number of possible ways to draw 7 line segments is 3796297200. Taking this number modulo 10^9 + 7 gives us 796297179.
```

__Constraints:__

- `2 <= n <= 1000`
- `1 <= k <= n-1`

__题目描述:__

给你一维空间的 `n` 个点，其中第 `i` 个点（编号从 `0` 到 `n-1`）位于 `x = i` 处，请你找到 __恰好__ `k` __个不重叠__ 线段且每个线段至少覆盖两个点的方案数。线段的两个端点必须都是 __整数坐标__ 。这 `k` 个线段不需要全部覆盖全部 `n` 个点，且它们的端点 __可以__ 重合。

请你返回 `k` 个不重叠线段的方案数。由于答案可能很大，请将结果对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

![1621-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/17/ex1.png)

```text
输入：n = 4, k = 2
输出：5
解释：
如图所示，两个线段分别用红色和蓝色标出。
上图展示了 5 种不同的方案 {(0,2),(2,3)}，{(0,1),(1,3)}，{(0,1),(2,3)}，{(1,2),(2,3)}，{(0,1),(1,2)} 。
```

示例 2：

```text
输入：n = 3, k = 1
输出：3
解释：总共有 3 种不同的方案 {(0,1)}, {(0,2)}, {(1,2)} 。
```

示例 3：

```text
输入：n = 30, k = 7
输出：796297179
解释：画 7 条线段的总方案数为 3796297200 种。将这个数对 10^9 + 7 取余得到 796297179 。
```

示例 4：

```text
输入：n = 5, k = 3
输出：7
```

示例 5：

```text
输入：n = 3, k = 2
输出：1
```

__提示：__

- `2 <= n <= 1000`
- `1 <= k <= n-1`

__思路:__

```text
1. 组合数学
从 n 个点中取 k 段线段
k 段线段需要 2k 个点
而这 2k 个点(k 段线段)最多可以有 k - 1 个公共点
所以实际上是从 n + k - 1 中取 2k 个点
即求 C(n + k - 1, k * 2)
时间复杂度为 O(N + K), 空间复杂度为 O(1)
2. 动态规划
设 dp[i][j] 表示从长度为 i 的线段中取 j 段线段的方案数
对第 i 个点可选可不选
如果不选择第 i 个点, 需要加上 dp[i - 1][j], 即在 i - 1 个点中选择 j 段线段
如果选择第 i 个点, 则需要枚举第 j 段线段的长度, 分别为 dp[i - 1][j - 1], dp[i - 2][j - 1], ..., dp[j][j - 1], 在前面的点中找到 j - 1 段线段
所以, dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1] + dp[i - 2][j - 1] + ... + dp[j][j - 1]
dp[i - 1][j] = dp[i - 2][j] + dp[i - 2][j - 1] + ... + dp[j][j - 1]
上面两式相减得, dp[i][j] = dp[i - 1][j] * 2 + dp[i - 1][j - 1] - dp[i - 2][j]
初始条件为 dp[i][0] = 1, 表示取 0 段有 1 种取法
时间复杂度为 O(NK), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfSets(int n, int k) 
    {
        int MOD = 1e9 + 7;
        long long dp[n + 2][n + 2];
        memset(dp, 0, sizeof dp);
        for (int i = 0; i < n + 2; i++) dp[i][0] = 1;
        for (int i = 2; i <= n; i++) for (int j = 1; j <= min(k, i - 1); j++) dp[i][j] = (((dp[i - 1][j] << 1) + dp[i - 1][j - 1] - dp[i - 2][j]) + MOD) % MOD;
        return dp[n][k];
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfSets(int n, int k) {
        int MOD = 1_000_000_007;
        long dp[][] = new long[n + 2][n + 2];
        for (int i = 0; i < n + 2; i++) dp[i][0] = 1;
        for (int i = 2; i <= n; i++) for (int j = 1; j <= Math.min(k, i - 1); j++) dp[i][j] = (((dp[i - 1][j] << 1) + dp[i - 1][j - 1] - dp[i - 2][j]) + MOD) % MOD;
        return (int)dp[n][k];
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSets(self, n: int, k: int) -> int:
        return comb(n + k - 1, k << 1) % (10 ** 9 + 7)
```
