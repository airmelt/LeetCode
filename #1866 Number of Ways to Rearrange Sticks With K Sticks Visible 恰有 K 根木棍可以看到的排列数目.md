# 1866 Number of Ways to Rearrange Sticks With K Sticks Visible 恰有 K 根木棍可以看到的排列数目

__Description:__

There are `n` uniquely-sized sticks whose lengths are integers from `1` to `n`. You want to arrange the sticks such that __exactly__ `k` sticks are __visible__ from the left. A stick is __visible__ from the left if there are no __longer__ sticks to the __left__ of it.

- For example, if the sticks are arranged `[1,3,2,5,4]`, then the sticks with lengths `1`, `3`, and `5` are visible from the left.

Given `n` and `k`, return _the __number__ of such arrangements_. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 3, k = 2
Output: 3
Explanation: [1,3,2], [2,3,1], and [2,1,3] are the only arrangements such that exactly 2 sticks are visible.
The visible sticks are underlined.
```

Example 2:

```text
Input: n = 5, k = 5
Output: 1
Explanation: [1,2,3,4,5] is the only arrangement such that all 5 sticks are visible.
```

Example 3:

```text
Input: n = 20, k = 11
Output: 647427950
Explanation: There are 647427950 (mod 10 ^ 9 + 7) ways to rearrange the sticks such that exactly 11 sticks are visible.
```

__Constraints:__

- `1 <= n <= 1000`
- `1 <= k <= n`

__题目描述:__

有 `n` 根长度互不相同的木棍，长度为从 `1` 到 `n` 的整数。请你将这些木棍排成一排，并满足从左侧 __可以看到__ __恰好__ `k` 根木棍。从左侧 __可以看到__ 木棍的前提是这个木棍的 __左侧__ 不存在比它 __更长的__ 木棍。

- 例如，如果木棍排列为 [___1___,___3___,2,___5___,4] ，那么从左侧可以看到的就是长度分别为 `1`、 `3` 、 `5` 的木棍。

给你 `n` 和 `k` ，返回符合题目要求的排列 __数目__ 。由于答案可能很大，请返回对 `10 ^ 9 + 7` __取余__ 的结果。

__示例:__

示例 1：

```text
输入：n = 3, k = 2
输出：3
解释：[1,3,2], [2,3,1] 和 [2,1,3] 是仅有的能满足恰好 2 根木棍可以看到的排列。
可以看到的木棍已经用粗体+斜体标识。
```

示例 2：

```text
输入：n = 5, k = 5
输出：1
解释：[1,2,3,4,5] 是唯一一种能满足全部 5 根木棍可以看到的排列。
```

示例 3：

```text
输入：n = 20, k = 11
输出：647427950
解释：总共有 647427950 (mod 10 ^ 9 + 7) 种能满足恰好有 11 根木棍可以看到的排列。
```

__提示：__

- `1 <= n <= 1000`
- `1 <= k <= n`

__思路:__

```text
动态规划
设 dp[i][j] 表示有 i 根木棍, 从左往右看能看到 k 根木棍的种类
初始化 dp[0][0] = 1, 表示 0 根木棍能看到 0 根木棍有 1 种方式
dp[i][j] 可以由 dp[i - 1][j - 1] 在最右边放置长度为 i 的木棍或者由 dp[i - 1][j] 在中间任意一个位置放置长度为 1 的木棍得来
即 dp[i][j] = dp[i - 1][j - 1] + (dp[i - 1][j] * (i - 1))
注意到 dp[i] 只和 dp[i - 1] 有关, 可以使用滚动数组将空间复杂度优化为 O(K)
时间复杂度为 O(NK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int rearrangeSticks(int n, int k) 
    {
        vector<long> result(n - k + 1, 0);
        result.back() = 1;
        for (int i = k; i != 0; i--) for (int j = n - k - 1; j != -1; j--) result[j] = (result[j] + ((i + j) * result[j + 1])) % (long)(1e9 + 7);
        return result.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int rearrangeSticks(int n, int k) {
        long result[] = new long[n - k + 1], MOD = 1_000_000_007;
        result[n - k] = 1;
        for (int i = k; i != 0; i--) for (int j = n - k - 1; j != -1; j--) result[j] = (result[j] + ((i + j) * result[j + 1])) % MOD;
        return (int)result[0];
    }
}
```

__Python__:

```Python
class Solution:
    def rearrangeSticks(self, n: int, k: int) -> int:
        dp, MOD = [[0] * (k + 1) for _ in range(n + 1)], 10 ** 9 + 7
        dp[0][0] = 1
        for i in range(1, n + 1):
            for j in range(1, min(k, i) + 1):
                dp[i][j] = (dp[i - 1][j - 1] + (dp[i - 1][j] * (i - 1))) % MOD
        return dp[-1][-1]
```
