# 2017 Grid Game 网格游戏

__Description:__

You are given a __0-indexed__ 2D array `grid` of size `2 x n`, where `grid[r][c]` represents the number of points at position `(r, c)` on the matrix. Two robots are playing a game on this matrix.

Both robots initially start at `(0, 0)` and want to reach `(1, n-1)`. Each robot may only move to the __right__ ( `(r, c)` to `(r, c + 1)`) or __down__ ( `(r, c)` to `(r + 1, c)`).

At the start of the game, the __first__ robot moves from `(0, 0)` to `(1, n-1)`, collecting all the points from the cells on its path. For all cells `(r, c)` traversed on the path, `grid[r][c]` is set to `0`. Then, the __second__ robot moves from `(0, 0)` to `(1, n-1)`, collecting the points on its path. Note that their paths may intersect with one another.

The first robot wants to minimize the number of points collected by the second robot. In contrast, the second robot wants to maximize the number of points it collects. If both robots play optimally, return the number of points collected by the second robot.

__Example:__

Example 1:

![2017-1](https://assets.leetcode.com/uploads/2021/09/08/a1.png)

```text
Input: grid = [[2,5,4],[1,5,1]]
Output: 4
Explanation: The optimal path taken by the first robot is shown in red, and the optimal path taken by the second robot is shown in blue.
The cells visited by the first robot are set to 0.
The second robot will collect 0 + 0 + 4 + 0 = 4 points.
```

Example 2:

![2017-2](https://assets.leetcode.com/uploads/2021/09/08/a2.png)

```text
Input: grid = [[3,3,1],[8,5,2]]
Output: 4
Explanation: The optimal path taken by the first robot is shown in red, and the optimal path taken by the second robot is shown in blue.
The cells visited by the first robot are set to 0.
The second robot will collect 0 + 3 + 1 + 0 = 4 points.
```

Example 3:

![2017-3](https://assets.leetcode.com/uploads/2021/09/08/a3.png)

```text
Input: grid = [[1,3,1,15],[1,3,3,1]]
Output: 7
Explanation: The optimal path taken by the first robot is shown in red, and the optimal path taken by the second robot is shown in blue.
The cells visited by the first robot are set to 0.
The second robot will collect 0 + 1 + 3 + 3 + 0 = 7 points.
```

__Constraints:__

- `grid.length == 2`
- `n == grid[r].length`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= grid[r][c] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始的二维数组 `grid` ，数组大小为 `2 x n` ，其中 `grid[r][c]` 表示矩阵中 `(r, c)` 位置上的点数。现在有两个机器人正在矩阵上参与一场游戏。

两个机器人初始位置都是 `(0, 0)` ，目标位置是 `(1, n-1)` 。每个机器人只会 __向右__ ( `(r, c)` 到 `(r, c + 1)`) 或 __向下__ ( `(r, c)` 到 `(r + 1, c)`) 。

游戏开始，__第一个__ 机器人从 `(0, 0)` 移动到 `(1, n-1)` ，并收集路径上单元格的全部点数。对于路径上所有单元格 `(r, c)` ，途经后 `grid[r][c]` 会重置为 `0` 。然后，__第二个__ 机器人从 `(0, 0)` 移动到 `(1, n-1)` ，同样收集路径上单元的全部点数。注意，它们的路径可能会存在相交的部分。

第一个 机器人想要打击竞争对手，使 第二个 机器人收集到的点数 最小化 。与此相对，第二个 机器人想要 最大化 自己收集到的点数。两个机器人都发挥出自己的 最佳水平 的前提下，返回 第二个 机器人收集到的 点数 。

__示例:__

示例 1：

![2017-4](https://assets.leetcode.com/uploads/2021/09/08/a1.png)

```text
输入：grid = [[2,5,4],[1,5,1]]
输出：4
解释：第一个机器人的最佳路径如红色所示，第二个机器人的最佳路径如蓝色所示。
第一个机器人访问过的单元格将会重置为 0 。
第二个机器人将会收集到 0 + 0 + 4 + 0 = 4 个点。
```

示例 2：

![2017-5](https://assets.leetcode.com/uploads/2021/09/08/a2.png)

```text
输入：grid = [[3,3,1],[8,5,2]]
输出：4
解释：第一个机器人的最佳路径如红色所示，第二个机器人的最佳路径如蓝色所示。 
第一个机器人访问过的单元格将会重置为 0 。
第二个机器人将会收集到 0 + 3 + 1 + 0 = 4 个点。
```

示例 3：

![2017-6](https://assets.leetcode.com/uploads/2021/09/08/a3.png)

```text
输入：grid = [[1,3,1,15],[1,3,3,1]]
输出：7
解释：第一个机器人的最佳路径如红色所示，第二个机器人的最佳路径如蓝色所示。
第一个机器人访问过的单元格将会重置为 0 。
第二个机器人将会收集到 0 + 1 + 3 + 3 + 0 = 7 个点。
```

__提示：__

- `grid.length == 2`
- `n == grid[r].length`
- `1 <= n <= 5 * 10 ^ 4`
- `1 <= grid[r][c] <= 10 ^ 5`

__思路:__

```text
模拟
注意到矩阵的大小为 2 x n
枚举第一个机器人的路径，计算第二个机器人的最大收益
若第一个机器人在 (0, i) 处向下移动
可以将矩阵分为两部分
第一行的后缀和第二行的前缀
取上述值的最大值的最小值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long gridGame(vector<vector<int>>& grid) 
    {
        long long result = LLONG_MAX, a = accumulate(grid.front().begin(), grid.front().end(), 0LL), b = 0L;
        for (int i = 0, n = grid.front().size(); i < n; i++) 
        {
            a -= grid.front()[i];
            result = min(result, max(a, b));
            b += grid.back()[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long gridGame(int[][] grid) {
        long result = Long.MAX_VALUE, a = Arrays.stream(grid[0]).asLongStream().sum(), b = 0L;
        for (int i = 0, n = grid[0].length; i < n; i++) {
            a -= grid[0][i];
            result = Math.min(result, Math.max(a, b));
            b += grid[1][i];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def gridGame(self, grid: List[List[int]]) -> int:
        result, a, b = inf, sum(grid[0]), 0
        for i, v in enumerate(grid[0]):
            a -= v
            result = min(result, max(a, b))
            b += grid[1][i]
        return result
```
