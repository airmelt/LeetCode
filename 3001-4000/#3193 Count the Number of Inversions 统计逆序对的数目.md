# 3193 Count the Number of Inversions 统计逆序对的数目

__Description:__

You are given an integer `n` and a 2D array `requirements`, where `requirements[i] = [end_i, cnt_i]` represents the end index and the __inversion__ count of each requirement.

A pair of indices `(i, j)` from an integer array `nums` is called an __inversion__ if:

- `i < j` and `nums[i] > nums[j]`

Return the number of __permutations__ `perm` of `[0, 1, 2, ..., n - 1]` such that for __all__ `requirements[i]`, `perm[0..end_i]` has exactly `cnt_i` inversions.

Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: n = 3, requirements = [[2,2],[0,0]]

Output: 2

Explanation:

The two permutations are:

[2, 0, 1]
Prefix [2, 0, 1] has inversions (0, 1) and (0, 2).
Prefix [2] has 0 inversions.
[1, 2, 0]
Prefix [1, 2, 0] has inversions (0, 2) and (1, 2).
Prefix [1] has 0 inversions.
```

Example 2:

```text
Input: n = 3, requirements = [[2,2],[1,1],[0,0]]

Output: 1

Explanation:

The only satisfying permutation is [2, 0, 1]:

Prefix [2, 0, 1] has inversions (0, 1) and (0, 2).
Prefix [2, 0] has an inversion (0, 1).
Prefix [2] has 0 inversions.
```

Example 3:

```text
Input: n = 2, requirements = [[0,0],[1,0]]

Output: 1

Explanation:

The only satisfying permutation is [0, 1]:

Prefix [0] has 0 inversions.
Prefix [0, 1] has no inversions.
```

__Constraints:__

- `2 <= n <= 300`
- `1 <= requirements.length <= n`
- `requirements[i] = [end_i, cnt_i]`
- `0 <= end_i <= n - 1`
- `0 <= cnt_i <= 400`
- The input is generated such that there is at least one `i` such that `end_i == n - 1`.
- The input is generated such that all `end_i` are unique.

__题目描述:__

给你一个整数 `n` 和一个二维数组 `requirements` ，其中 `requirements[i] = [end_i, cnt_i]` 表示这个要求中的末尾下标和 __逆序对__ 的数目。

整数数组 `nums` 中一个下标对 `(i, j)` 如果满足以下条件，那么它们被称为一个 __逆序对__ ：

- `i < j` 且 `nums[i] > nums[j]`

请你返回 `[0, 1, 2, ..., n - 1]` 的 __排列__ `perm` 的数目，满足对 __所有__ 的 `requirements[i]` 都满足 `perm[0..end_i]` 中恰好有 `cnt_i` 个逆序对。

由于答案可能会很大，将它对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：n = 3, requirements = [[2,2],[0,0]]

输出：2

解释：

两个排列为：

[2, 0, 1]
前缀 [2, 0, 1] 的逆序对为 (0, 1) 和 (0, 2) 。
前缀 [2] 的逆序对数目为 0 个。
[1, 2, 0]
前缀 [1, 2, 0] 的逆序对为 (0, 2) 和 (1, 2) 。
前缀 [1] 的逆序对数目为 0 个。
```

示例 2：

```text
输入：n = 3, requirements = [[2,2],[1,1],[0,0]]

输出：1

解释：

唯一满足要求的排列是 [2, 0, 1] ：

前缀 [2, 0, 1] 的逆序对为 (0, 1) 和 (0, 2) 。
前缀 [2, 0] 的逆序对为 (0, 1) 。
前缀 [2] 的逆序对数目为 0 。
```

示例 3：

```text
输入：n = 2, requirements = [[0,0],[1,0]]

输出：1

解释：

唯一满足要求的排列为 [0, 1] ：

前缀 [0] 的逆序对数目为 0 。
前缀 [0, 1] 没有逆序对。
```

__提示：__

- `2 <= n <= 300`
- `1 <= requirements.length <= n`
- `requirements[i] = [end_i, cnt_i]`
- `0 <= end_i <= n - 1`
- `0 <= cnt_i <= 400`
- 输入保证至少有一个 `i` 满足 `end_i == n - 1` 。
- 输入保证所有的 `end_i` 互不相同。

__思路:__

```text
动态规划
设 dp[i][j] 表示使用了 i + 1 个数字, 这 i + 1 个数字可以任意排列, 组成逆序对的数量为 j 的排列个数
设 req 表示 requirements 中对应的要求
即 req[end_i] = cnt_i, 如果 end_i 不存在, 初始化为 -1, 和 req[0] = 0 区分
如果 req[0] != 0 返回 0, 因为单个数字的前缀必然没有逆序对
初始化 dp[0][0] = 1
设 k 为 perm[i] 与 perm[:i] 组成的逆序对的数量
dp[i][j] = sum(dp[i - 1][j - k]), 0 <= k <= min(i, j)
这是因为最多为 i 个逆序对, 同时 j >= k
当 req[i - 1] > -1
如果 j < req[i - 1], 因为 j 只能减小不能增大, dp[i][j] = 0
如果 j > req[i - 1] + i, 那么即使 perm[i] 填入最小的数, 也只能增加 i 个逆序对还是不够 j
当 req[i - 1] <= j <= req[i - 1] + i, dp[i][j] = dp[i - 1][j - k], k 即 perm[i] 与 perm[:i] 组成的逆序对的数量, dp[i][j] = dp[i - 1][req[i - 1]]
最后返回 dp[n - 1][req[n - 1]] 即可
时间复杂度为 O(MN * min(M, N)), 空间复杂度为 O(MN)
使用前缀和优化可以将时间复杂度优化到 O(MN)
再使用滚动数组可以将空间复杂度优化到 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfPermutations(int n, vector<vector<int>>& requirements) 
    {
        
        vector<int> req(n, -1);
        req.front() = 0;
        for (const auto& requirement : requirements) req[requirement.front()] = requirement.back();
        int MOD = 1e9 + 7, mx = *max_element(req.begin(), req.end());
        vector<int> dp(mx + 1);
        dp.front() = 1;
        for (int i = 1, r = 0, m = 0; i < n; i++) 
        {
            if ((r = req[i - 1]) > -1) 
            {
                fill(dp.begin(), dp.begin() + r, 0);
                fill(dp.begin() + r + 1, dp.begin() + min(i + r, m = req[i] < 0 ? mx : req[i]) + 1, dp[r]);
                fill(dp.begin() + min(i + r, m) + 1, dp.end(), 0);
            }
            else
            {
                for (int j = 1; j <= (m = req[i] < 0 ? mx : req[i]); j++) dp[j] = (dp[j] + dp[j - 1]) % MOD;
                for (int j = m; j > i; j--) dp[j] = (dp[j] - dp[j - i - 1] + MOD) % MOD;
            }
        }
        return dp[req[n - 1]];
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfPermutations(int n, int[][] requirements) {
        int MOD = 1_000_000_007, req[] = new int[n];
        Arrays.fill(req, -1);
        req[0] = 0;
        for (var requirement : requirements) req[requirement[0]] = requirement[1];
        int mx = Arrays.stream(req).max().getAsInt(), dp[][] = new int[n][mx + 1];
        dp[0][0] = 1;
        for (int i = 1, r = 0, m = 0; i < n; i++) {
            if ((r = req[i - 1]) > -1) for (int j = r; j <= Math.min(i + r, m = req[i] < 0 ? mx : req[i]); j++) dp[i][j] = dp[i - 1][r];
            else for (int j = 0; j <= m; j++) for (int k = 0; k <= Math.min(i, j); k++) dp[i][j] = (dp[i][j] + dp[i - 1][j - k]) % MOD;
        }
        return dp[n - 1][req[n - 1]];
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfPermutations(self, n: int, requirements: List[List[int]]) -> int:
        requirements = dict(requirements)
        if requirements.get(0, 0):
            return 0
        requirements[0] = 0
        @lru_cache(None)
        def dfs(i: int, j: int) -> int:
            return 1 if not i else dfs(i - 1, r) if (r := requirements.get(i - 1, -1)) > -1 and r <= j <= i + r else 0 if r > -1 else sum(dfs(i - 1, j - k) for k in range(min(i, j) + 1))
        return dfs(n - 1, requirements[n - 1]) % (10 ** 9 + 7)
```
