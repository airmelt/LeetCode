# 1937 Maximum Number of Points with Cost 扣分后的最大得分

__Description:__

You are given an `m x n` integer matrix `points` (__0-indexed__). Starting with `0` points, you want to __maximize__ the number of points you can get from the matrix.

To gain points, you must pick one cell in __each row__. Picking the cell at coordinates (r, c) will __add__ `points[r][c]` to your score.

However, you will lose points if you pick a cell too far from the cell that you picked in the previous row. For every two adjacent rows `r` and `r + 1` (where `0 <= r < m - 1`), picking cells at coordinates `(r, c1)` and `(r + 1, c2)` will __subtract__ `abs(c1 - c2)` from your score.

Return the __maximum__ number of points you can achieve.

`abs(x)` is defined as:

- `x` for `x >= 0`.
- `-x` for `x < 0`.

__Example:__

Example 1:

![1937-1](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

```text
Input: points = [[1,2,3],[1,5,1],[3,1,1]]
Output: 9
Explanation:
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.
```

Example 2:

![1937-2](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png)

```text
Input: points = [[1,5],[2,3],[4,2]]
Output: 11
Explanation:
The blue cells denote the optimal cells to pick, which have coordinates (0, 1), (1, 1), and (2, 0).
You add 5 + 3 + 4 = 12 to your score.
However, you must subtract abs(1 - 1) + abs(1 - 0) = 1 from your score.
Your final score is 12 - 1 = 11.
```

__Constraints:__

- `m == points.length`
- `n == points[r].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= points[r][c] <= 10 ^ 5`

__题目描述:__

给你一个 `m x n` 的整数矩阵 `points` （下标从 __0__ 开始）。一开始你的得分为 `0` ，你想最大化从矩阵中得到的分数。

你的得分方式为：__每一行__ 中选取一个格子，选中坐标为 `(r, c)` 的格子会给你的总得分 __增加__ `points[r][c]` 。

然而，相邻行之间被选中的格子如果隔得太远，你会失去一些得分。对于相邻行 `r` 和 `r + 1` （其中 `0 <= r < m - 1`），选中坐标为 `(r, c1)` 和 `(r + 1, c2)` 的格子，你的总得分 __减少__ `abs(c1 - c2)` 。

请你返回你能得到的 __最大__ 得分。

`abs(x)` 定义为：

- 如果 `x >= 0` ，那么值为 `x` 。
- 如果 `x < 0` ，那么值为 `-x` 。

__示例:__

示例 1：

![1937-3](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-40-26-diagram-drawio-diagrams-net.png)

```text
输入：points = [[1,2,3],[1,5,1],[3,1,1]]
输出：9
解释：
蓝色格子是最优方案选中的格子，坐标分别为 (0, 2)，(1, 1) 和 (2, 0) 。
你的总得分增加 3 + 5 + 3 = 11 。
但是你的总得分需要扣除 abs(2 - 1) + abs(1 - 0) = 2 。
你的最终得分为 11 - 2 = 9 。
```

示例 2：

![1937-4](https://assets.leetcode.com/uploads/2021/07/12/screenshot-2021-07-12-at-13-42-14-diagram-drawio-diagrams-net.png)

```text
输入：points = [[1,5],[2,3],[4,2]]
输出：11
解释：
蓝色格子是最优方案选中的格子，坐标分别为 (0, 1)，(1, 1) 和 (2, 0) 。
你的总得分增加 5 + 3 + 4 = 12 。
但是你的总得分需要扣除 abs(1 - 1) + abs(1 - 0) = 1 。
你的最终得分为 12 - 1 = 11 。
```

__提示：__

- `m == points.length`
- `n == points[r].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= points[r][c] <= 10 ^ 5`

__思路:__

```text
动态规划
设 dp[i][j] 表示选择第 i 行第 j 列的格子的得分
若第 i - 1 行选择了 j'
dp[i][j] = max(dp[i - 1][j'] - abs(j - j')) + points[i][j]
可以正序遍历和反序遍历各一次分别计算出两端的最大值 left, right
利用 left 和 right 快速计算出 dp[i][j]
由于只需要用到 dp[i - 1], 还可以用滚动数组优化空间复杂度至 O(n)
时间复杂度为 O(MN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxPoints(vector<vector<int>>& points) 
    {
        int m = points.size(), n = points.front().size();
        vector<vector<long long>> dp(m, vector<long long>(n));
        for (int j = 0; j < n; j++) dp.front()[j] = points.front()[j];
        for (int i = 1; i < m; i++) 
        {
            long long left = LONG_LONG_MIN, right = LONG_LONG_MIN;
            for (int j = 0, k = n - j - 1; j < n; j++, k--) 
            {
                dp[i][j] = max(dp[i][j], (left = max(left, dp[i - 1][j] + j)) + points[i][j] - j);
                dp[i][k] = max(dp[i][k], (right = max(right, dp[i - 1][k] - k)) + points[i][k] + k);
            }
        }
        return *max_element(dp.back().begin(), dp.back().end());
    }
};
```

__Java__:

```Java
class Solution {
    public long maxPoints(int[][] points) {
        int m = points.length, n = points[0].length;
        long dp[][] = new long[m][n], result = 0;
        for (int j = 0; j < n; j++) dp[0][j] = points[0][j];
        for (int i = 1; i < m; i++) {
            long left = Long.MIN_VALUE, right = Long.MIN_VALUE;
            for (int j = 0, k = n - j - 1; j < n; j++, k--) {
                dp[i][j] = Math.max(dp[i][j], (left = Math.max(left, dp[i - 1][j] + j)) + points[i][j] - j);
                dp[i][k] = Math.max(dp[i][k], (right = Math.max(right, dp[i - 1][k] - k)) + points[i][k] + k);
            }
        }
        for (int j = 0; j < n; j++) result = Math.max(result, dp[m - 1][j]);
        return result;
    }
} 
```

__Python__:

```Python
class Solution:
    def maxPoints(self, points: List[List[int]]) -> int:
        m, n = len(points), len(points[0])
        dp = [points[0]] + [[0] * n for _ in range(m - 1)]
        for i in range(1, m):
            left = right = -inf
            for j in range(n):
                dp[i][j] = max(dp[i][j], (left := max(left, dp[i - 1][j] + j)) + points[i][j] - j)
                dp[i][n - j - 1] = max(dp[i][n - j - 1], (right := max(right, dp[i - 1][n - j - 1] - n + j + 1)) + points[i][n - j - 1] + n - j - 1)
        return max(dp[-1])
```
