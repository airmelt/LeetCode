# 1434 Number of Ways to Wear Different Hats to Each Other 每个人戴不同帽子的方案数

__Description:__

There are `n` people and `40` types of hats labeled from `1` to `40`.

Given a 2D integer array `hats`, where `hats[i]` is a list of all hats preferred by the `ith` person.

Return _the number of ways that the `n` people wear different hats to each other_.

Since the answer may be too large, return it modulo `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: hats = [[3,4],[4,5],[5]]
Output: 1
Explanation: There is only one way to choose hats given the conditions. 
First person choose hat 3, Second person choose hat 4 and last one hat 5.
```

Example 2:

```text
Input: hats = [[3,5,1],[3,5]]
Output: 4
Explanation: There are 4 ways to choose hats:
(3,5), (5,3), (1,3) and (1,5)
```

Example 3:

```text
Input: hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
Output: 24
Explanation: Each person can choose hats labeled from 1 to 4.
Number of Permutations of (1,2,3,4) = 24.
```

__Constraints:__

- `n == hats.length`
- `1 <= n <= 10`
- `1 <= hats[i].length <= 40`
- `1 <= hats[i][j] <= 40`
- `hats[i]` contains a list of __unique__ integers.

__题目描述:__

总共有 `n` 个人和 `40` 种不同的帽子，帽子编号从 `1` 到 `40` 。

给你一个整数列表的列表 `hats` ，其中 `hats[i]` 是第 `i` 个人所有喜欢帽子的列表。

请你给每个人安排一顶他喜欢的帽子，确保每个人戴的帽子跟别人都不一样，并返回方案数。

由于答案可能很大，请返回它对 `10 ^ 9 + 7` 取余后的结果。

__示例:__

示例 1：

```text
输入：hats = [[3,4],[4,5],[5]]
输出：1
解释：给定条件下只有一种方法选择帽子。
第一个人选择帽子 3，第二个人选择帽子 4，最后一个人选择帽子 5。
```

示例 2：

```text
输入：hats = [[3,5,1],[3,5]]
输出：4
解释：总共有 4 种安排帽子的方法：
(3,5)，(5,3)，(1,3) 和 (1,5)
```

示例 3：

```text
输入：hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
输出：24
解释：每个人都可以从编号为 1 到 4 的帽子中选。
(1,2,3,4) 4 个帽子的排列方案数为 24 。
```

示例 4：

```text
输入：hats = [[1,2,3],[2,3,5,6],[1,3,7,9],[1,8,9],[2,5,7]]
输出：111
```

__提示：__

- `n == hats.length`
- `1 <= n <= 10`
- `1 <= hats[i].length <= 40`
- `1 <= hats[i][j] <= 40`
- `hats[i]` 包含一个数字互不相同的整数列表。

__思路:__

```text
动态规划 ➕ 状态压缩
设 dp[i][mask] 表示处理了前 i 顶帽子, 分配方式为 mask 的种类
其中 mask 用一个整形表示分配帽子的方式
比如 0b00101 表示将第 1 顶帽子和第 3 顶帽子分配了, 其他帽子未分配
如果帽子没分配, dp[i][mask] += dp[i - 1][mask]
如果帽子分配出去了, dp[i][mask] += dp[i - 1][mask ^ (1 << i)], mask ^ (1 << i) 表示将帽子分配给某个特定的人, 注意这个人需要满足喜欢这顶帽子
预处理 dp[0][0] = 1, 表示分配给 0 个人 0 顶帽子有 1 种方式
时间复杂度为 O(2 ^ N * H), 空间复杂度为 O(2 ^ N), H 表示 hats 数组中的元素数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberWays(vector<vector<int>>& hats) 
    {
        int n = hats.size(), mod = 1e9 + 7, m = (1 << n);
        vector<vector<bool>> valid(41, vector<bool>(n));
        for (int i = 0; i < n; i++) for (const auto& h: hats[i]) valid[h][i] = true;
        vector<vector<int>> dp(41, vector<int>(m));
        dp.front().front() = 1;
        for (int h = 1; h <= 40; h++)
        {
            for (int mask = 0; mask < m; mask++)
            {
                dp[h][mask] = (dp[h][mask] + dp[h - 1][mask]) % mod;
                for (int i = 0; i < n; i++) if ((mask & (1 << i)) and valid[h][i]) dp[h][mask] = (dp[h][mask] + dp[h - 1][mask ^ (1 << i)]) % mod;
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution 
{
    public int numberWays(List<List<Integer>> hats)
    {
        int n = hats.size(), mod = 1_000_000_007, m = (1 << n), dp[][] = new int[41][m];
        dp[0][0] = 1;
        boolean[][] valid = new boolean[41][n];
        for (int i = 0; i < n; i++) for (int h : hats.get(i)) valid[h][i] = true;
        for (int h = 1; h <= 40; h++) {
            for (int mask = 0; mask < m; ++mask) {
                dp[h][mask] = (dp[h][mask] + dp[h - 1][mask]) % mod;
                for (int i = 0; i < n; i++) if ((mask & (1 << i)) != 0 && valid[h][i]) dp[h][mask] = (dp[h][mask] + dp[h - 1][mask ^ (1 << i)]) % mod;
            }
        }
        return dp[40][m - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def numberWays(self, hats: List[List[int]]) -> int:
        m, mod = (1 << (n := len(hats))), 10 ** 9 + 7
        dp, valid = [[0] * m for _ in range(41)], {i: set(v) for i, v in enumerate(hats)}
        dp[0][0] = 1
        for h in range(1, 41):
            for mask in range(m):
                dp[h][mask] += dp[h - 1][mask]
                for i in range(n):
                    if (mask & (1 << i)) and h in valid[i]:
                        dp[h][mask] += dp[h - 1][mask ^ (1 << i)]
        return dp[-1][-1] % mod
```
