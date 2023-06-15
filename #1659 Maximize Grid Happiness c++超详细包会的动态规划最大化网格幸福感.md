# 1659 Maximize Grid Happiness c++超详细包会的动态规划最大化网格幸福感

__Description:__

You are given four integers, `m`, `n`, `introvertsCount`, and `extrovertsCount`. You have an `m x n` grid, and there are two types of people: introverts and extroverts. There are `introvertsCount` introverts and `extrovertsCount` extroverts.

You should decide how many people you want to live in the grid and assign each of them one grid cell. Note that you do not have to have all the people living in the grid.

The happiness of each person is calculated as follows:

- Introverts __start__ with `120` happiness and __lose__ `30` happiness for each neighbor (introvert or extrovert).
- Extroverts __start__ with `40` happiness and __gain__ `20` happiness for each neighbor (introvert or extrovert).

Neighbors live in the directly adjacent cells north, east, south, and west of a person's cell.

The grid happiness is the sum of each person's happiness. Return the maximum possible grid happiness.

__Example:__

Example 1:

![1659-1](https://assets.leetcode.com/uploads/2020/11/05/grid_happiness.png)

```text
Input: m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2
Output: 240
Explanation: Assume the grid is 1-indexed with coordinates (row, column).
We can put the introvert in cell (1,1) and put the extroverts in cells (1,3) and (2,3).
- Introvert at (1,1) happiness: 120 (starting happiness) - (0 * 30) (0 neighbors) = 120
- Extrovert at (1,3) happiness: 40 (starting happiness) + (1 * 20) (1 neighbor) = 60
- Extrovert at (2,3) happiness: 40 (starting happiness) + (1 * 20) (1 neighbor) = 60
The grid happiness is 120 + 60 + 60 = 240.
The above figure shows the grid in this example with each person's happiness. The introvert stays in the light green cell while the extroverts live on the light purple cells.
```

Example 2:

```text
Input: m = 3, n = 1, introvertsCount = 2, extrovertsCount = 1
Output: 260
Explanation: Place the two introverts in (1,1) and (3,1) and the extrovert at (2,1).
- Introvert at (1,1) happiness: 120 (starting happiness) - (1 * 30) (1 neighbor) = 90
- Extrovert at (2,1) happiness: 40 (starting happiness) + (2 * 20) (2 neighbors) = 80
- Introvert at (3,1) happiness: 120 (starting happiness) - (1 * 30) (1 neighbor) = 90
The grid happiness is 90 + 80 + 90 = 260.
```

Example 3:

```text
Input: m = 2, n = 2, introvertsCount = 4, extrovertsCount = 0
Output: 240
```

__Constraints:__

- `1 <= m, n <= 5`
- `0 <= introvertsCount, extrovertsCount <= min(m * n, 6)`

__题目描述:__

给你四个整数 `m`、 `n`、 `introvertsCount` 和 `extrovertsCount` 。有一个 `m x n` 网格，和两种类型的人:内向的人和外向的人。总共有 `introvertsCount` 个内向的人和 `extrovertsCount` 个外向的人。

请你决定网格中应当居住多少人，并为每个人分配一个网格单元。 注意，不必 让所有人都生活在网格中。

每个人的 幸福感 计算如下：

- 内向的人 __开始__ 时有 `120` 个幸福感，但每存在一个邻居（内向的或外向的）他都会 __失去__  `30` 个幸福感。
- 外向的人 __开始__ 时有 `40` 个幸福感，每存在一个邻居（内向的或外向的）他都会 __得到__  `20` 个幸福感。

邻居是指居住在一个人所在单元的上、下、左、右四个直接相邻的单元中的其他人。

网格幸福感 是每个人幸福感的 总和 。 返回 最大可能的网格幸福感 。

__示例:__

示例 1：

![1659-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/15/grid_happiness.png)

```text
输入：m = 2, n = 3, introvertsCount = 1, extrovertsCount = 2
输出：240
解释：假设网格坐标 (row, column) 从 1 开始编号。
将内向的人放置在单元 (1,1) ，将外向的人放置在单元 (1,3) 和 (2,3) 。
- 位于 (1,1) 的内向的人的幸福感：120（初始幸福感）- (0 * 30)（0 位邻居）= 120
- 位于 (1,3) 的外向的人的幸福感：40（初始幸福感）+ (1 * 20)（1 位邻居）= 60
- 位于 (2,3) 的外向的人的幸福感：40（初始幸福感）+ (1 * 20)（1 位邻居）= 60
网格幸福感为：120 + 60 + 60 = 240
上图展示该示例对应网格中每个人的幸福感。内向的人在浅绿色单元中，而外向的人在浅紫色单元中。
```

示例 2：

```text
输入：m = 3, n = 1, introvertsCount = 2, extrovertsCount = 1
输出：260
解释：将内向的人放置在单元 (1,1) 和 (3,1) ，将外向的人放置在单元 (2,1) 。
- 位于 (1,1) 的内向的人的幸福感：120（初始幸福感）- (1 * 30)（1 位邻居）= 90
- 位于 (2,1) 的外向的人的幸福感：40（初始幸福感）+ (2 * 20)（2 位邻居）= 80
- 位于 (3,1) 的内向的人的幸福感：120（初始幸福感）- (1 * 30)（1 位邻居）= 90
网格幸福感为 90 + 80 + 90 = 260
```

示例 3：

```text
输入：m = 2, n = 2, introvertsCount = 4, extrovertsCount = 0
输出：240
```

__提示：__

- `1 <= m, n <= 5`
- `0 <= introvertsCount, extrovertsCount <= min(m * n, 6)`

__思路:__

```text
动态规划 ➕ 状态压缩 ➕ 轮廓线优化
设 dp[i][j][a][b][cur] 表示在 i 行 j 列，剩余内向人数为 a，外向人数为 b，上一行状态为 cur 时的最大幸福感
cur 由一个三进制数表示，每一位表示当前位置的状态，0 表示空，1 表示内向人，2 表示外向人
可以先将位置压缩为一维 dp[pos][a][b][cur], 其中 i = pos / n, j = pos % n
注意到每一格的状态只与它的左边和上边有关，因此可以使用轮廓线优化
即记录当前格子的前 n 个格子的状态, 这样可以在 O(1) 的时间内计算出左边和上边的格子的状态, 完成状态转移
设 n 个格子总的状态为 status = 3 ^ n, mask = status / 3, left 表示下一格左边 n 个格子的状态, 即当前状态 cur 是从 left 转移来的, 则 left = cur * 3 % status, 即将 cur 左移一位并取模使得 left 仍然在 [0, status) 范围内
diff 数组用来计算放入人之后幸福感的变化情况, 比如 diff[1][2] = -10 表示在内向的人边上放一个外向的人会减少 10 的幸福感
cur 的第一位表示上边的格子的状态, 即 cur / mask, cur 的 n - 1 位表示左边的格子的状态, 即 cur % 3
从后往前遍历
如果当前位置不放人 dp[pos][a][b][cur] = dp[pos + 1][a][b][left]
如果当前位置放内向的人, 要求当前还剩的内向的人大于 0, dp[pos][a][b][cur] = max(dp[pos][a][b][cur], 120 + dp[pos + 1][a - 1][b][left + 1] + (j != 0) * diff[1][cur % 3] + diff[1][cur / mask])
其中 
120 为基础幸福感
dp[pos + 1][a - 1][b][left + 1] 表示从 pos + 1 转移过来, 从 left 转移过来, 因为是内向的人需要 + 1, 内向的人需要减少 1
(j != 0) * diff[1][cur % 3] 表示如果当前位置不是第一列, 则需要加上左边的格子的幸福感变化, cur % 3 表示左边的格子的状态
diff[1][cur / mask] 表示当加上上边的格子的幸福感变化
外向的类似
最后返回 dp[0][introvertsCount][extrovertsCount][0]
即在初始位置, 剩余内向人数为 introvertsCount, 外向人数为 extrovertsCount, 上一行状态为 0 时的最大幸福感
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) 
    {
        int diff[3][3]{{0, 0, 0}, {0, -60, -10}, {0, -10, 40}}, status = pow(3, n), mask = status / 3, dp[m * n + 1][introvertsCount + 1][extrovertsCount + 1][status];
        memset(dp, 0, sizeof dp);
        for (int pos = m * n - 1, i = m - 1, j = n - 1; pos > -1; pos--, i = pos / n, j = pos % n) for (int a = 0; a <= introvertsCount; a++) for (int b = 0; b <= extrovertsCount; b++) for (int cur = 0, left = 0; cur < status; cur++, left = cur * 3 % status) dp[pos][a][b][cur] = max({dp[pos + 1][a][b][left], a > 0 ? 120 + dp[pos + 1][a - 1][b][left + 1] + (j != 0) * diff[1][cur % 3] + diff[1][cur / mask] : 0, b > 0 ? 40 + dp[pos + 1][a][b - 1][left + 2] + (j != 0) * diff[2][cur % 3] + diff[2][cur / mask] : 0});
        return dp[0][introvertsCount][extrovertsCount][0];
    }
};
```

__Java__:

```Java
class Solution {
    public int getMaxGridHappiness(int m, int n, int introvertsCount, int extrovertsCount) {
        int diff[][] = new int[][]{{0, 0, 0}, {0, -60, -10}, {0, -10, 40}}, status = (int)Math.pow(3, n), mask = status / 3, dp[][][][] = new int[m * n + 1][introvertsCount + 1][extrovertsCount + 1][status];
        for (int pos = m * n - 1, i = m - 1, j = n - 1; pos > -1; pos--, i = pos / n, j = pos % n) for (int a = 0; a <= introvertsCount; a++) for (int b = 0; b <= extrovertsCount; b++) for (int cur = 0, left = 0; cur < status; cur++, left = cur * 3 % status) dp[pos][a][b][cur] = Math.max(dp[pos + 1][a][b][left], Math.max(a > 0 ? 120 + dp[pos + 1][a - 1][b][left + 1] + (j != 0 ? 1 : 0) * diff[1][cur % 3] + diff[1][cur / mask] : 0, b > 0 ? 40 + dp[pos + 1][a][b - 1][left + 2] + (j != 0 ? 1 : 0) * diff[2][cur % 3] + diff[2][cur / mask] : 0));
        return dp[0][introvertsCount][extrovertsCount][0];
    }
}
```

__Python__:

```Python
class Solution:
    def getMaxGridHappiness(self, m: int, n: int, introvertsCount: int, extrovertsCount: int) -> int:
        mask, dp, diff = 3 ** (n - 1), [[[[0] * (status := 3 ** n) for _ in range(extrovertsCount + 1)] for _ in range(introvertsCount + 1)] for _ in range(m * n + 1)], [[0, 0, 0], [0, -60, -10], [0, -10, 40]]
        for pos in range(m * n - 1, -1, -1):
            i, j = pos // n, pos % n
            for a in range(introvertsCount + 1):
                for b in range(extrovertsCount + 1):
                    for cur in range(status):
                        dp[pos][a][b][cur] = max(dp[pos + 1][a][b][left := cur * 3 % status], 0 if a < 1 else (120 + dp[pos + 1][a - 1][b][left + 1] + (j != 0) * diff[1][cur % 3] + diff[1][cur // mask]), 0 if b < 1 else (40 + dp[pos + 1][a][b - 1][left + 2] + (j != 0) * diff[2][cur % 3] + diff[2][cur // mask]))
        return dp[0][introvertsCount][extrovertsCount][0]
```
