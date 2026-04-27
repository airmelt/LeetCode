# 2923 Find Champion I 找到冠军 I

__Description:__

There are `n` teams numbered from `0` to `n - 1` in a tournament.

Given a __0-indexed__ 2D boolean matrix `grid` of size `n * n`. For all `i, j` that `0 <= i, j <= n - 1` and `i != j` team `i` is __stronger__ than team `j` if `grid[i][j] == 1`, otherwise, team `j` is __stronger__ than team `i`.

Team `a` will be the __champion__ of the tournament if there is no team `b` that is stronger than team `a`.

Return the team that will be the champion of the tournament.

__Example:__

Example 1:

```text
Input: grid = [[0,1],[0,0]]
Output: 0
Explanation: There are two teams in this tournament.
grid[0][1] == 1 means that team 0 is stronger than team 1. So team 0 will be the champion.
```

Example 2:

```text
Input: grid = [[0,0,1],[1,0,1],[0,0,0]]
Output: 1
Explanation: There are three teams in this tournament.
grid[1][0] == 1 means that team 1 is stronger than team 0.
grid[1][2] == 1 means that team 1 is stronger than team 2.
So team 1 will be the champion.
```

__Constraints:__

- `n == grid.length`
- `n == grid[i].length`
- `2 <= n <= 100`
- `grid[i][j]` is either `0` or `1`.
- For all `i grid[i][i]` is `0.`
- For all `i, j` that `i != j`, `grid[i][j] != grid[j][i]`.
- The input is generated such that if team `a` is stronger than team `b` and team `b` is stronger than team `c`, then team `a` is stronger than team `c`.

__题目描述:__

一场比赛中共有 `n` 支队伍，按从 `0` 到  `n - 1` 编号。

给你一个下标从 __0__ 开始、大小为 `n * n` 的二维布尔矩阵 `grid` 。对于满足 `0 <= i, j <= n - 1` 且 `i != j` 的所有 `i, j` :如果 `grid[i][j] == 1`，那么 `i` 队比 `j` 队 __强__ ；否则， `j` 队比 `i` 队 __强__ 。

在这场比赛中，如果不存在某支强于 `a` 队的队伍，则认为 `a` 队将会是 __冠军__ 。

返回这场比赛中将会成为冠军的队伍。

__示例:__

示例 1：

```text
输入：grid = [[0,1],[0,0]]
输出：0
解释：比赛中有两支队伍。
grid[0][1] == 1 表示 0 队比 1 队强。所以 0 队是冠军。
```

示例 2：

```text
输入：grid = [[0,0,1],[1,0,1],[0,0,0]]
输出：1
解释：比赛中有三支队伍。
grid[1][0] == 1 表示 1 队比 0 队强。
grid[1][2] == 1 表示 1 队比 2 队强。
所以 1 队是冠军。
```

__提示：__

- `n == grid.length`
- `n == grid[i].length`
- `2 <= n <= 100`
- `grid[i][j]` 的值为 `0` 或 `1`
- 对于所有 `i`， `grid[i][i]` 等于 `0.`
- 对于满足 `i != j` 的所有 `i, j` ， `grid[i][j] != grid[j][i]` 均成立
- 生成的输入满足:如果 `a` 队比 `b` 队强， `b` 队比 `c` 队强，那么 `a` 队比 `c` 队强

__思路:__

```text
1. 暴力法
对每个队伍
如果按行看, 它可以战胜任何一只队伍所以 sum(row) == n - 1
如果按列看, 没有一只队伍能战胜它 sum(col) == 0
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 投票法
假定一个队伍是冠军
那么能打败这个冠军的队伍才有资格竞争冠军
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findChampion(vector<vector<int>>& grid) 
    {
        int result = 0;
        for (int i = 1, n = grid.size(); i < n; i++) if (grid[i][result]) result = i;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findChampion(int[][] grid) {
        int result = 0;
        for (int i = 1, n = grid.length; i < n; i++) if (grid[i][result] == 1) result = i;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findChampion(self, grid: List[List[int]]) -> int:
        return [j for j in range(len(grid)) if all(not grid[i][j] for i in range(len(grid)))][0]
```
