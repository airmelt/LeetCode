# 2658 Maximum Number of Fish in a Grid 网格图中鱼的最大数目

__Description:__

You are given a __0-indexed__ 2D matrix `grid` of size `m x n`, where `(r, c)` represents:

- A __land__ cell if `grid[r][c] = 0`, or
- A __water__ cell containing `grid[r][c]` fish, if `grid[r][c] > 0`.

A fisher can start at any __water__ cell `(r, c)` and can do the following operations any number of times:

- Catch all the fish at cell `(r, c)`, or
- Move to any adjacent __water__ cell.

Return _the __maximum__ number of fish the fisher can catch if he chooses his starting cell optimally, or_ `0` if no water cell exists.

An __adjacent__ cell of the cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` or `(r - 1, c)` if it exists.

__Example:__

Example 1:

![2658-1](https://assets.leetcode.com/uploads/2023/03/29/example.png)

```text
Input:  grid = [[0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0]]
Output:  7
Explanation:  The fisher can start at cell (1,3) and collect 3 fish, then move to cell (2,3) and collect 4 fish.
```

Example 2:

![2658-2](https://assets.leetcode.com/uploads/2023/03/29/example2.png)

```text
Input: grid = [[1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1]]
Output: 1
Explanation: The fisher can start at cells (0,0) or (3,3) and collect a single fish.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `0 <= grid[i][j] <= 10`

__题目描述:__

给你一个下标从 __0__ 开始大小为 `m x n` 的二维整数数组 `grid` ，其中下标在 `(r, c)` 处的整数表示:

- 如果 `grid[r][c] = 0` ，那么它是一块 __陆地__ 。
- 如果 `grid[r][c] > 0` ，那么它是一块 __水域__ ，且包含 `grid[r][c]` 条鱼。

一位渔夫可以从任意 __水域__ 格子 `(r, c)` 出发，然后执行以下操作任意次:

- 捕捞格子 `(r, c)` 处所有的鱼，或者
- 移动到相邻的 __水域__ 格子。

请你返回渔夫最优策略下， __最多__ 可以捕捞多少条鱼。如果没有水域格子，请你返回 `0` 。

格子 `(r, c)` __相邻__ 的格子为 `(r, c + 1)` ， `(r, c - 1)` ， `(r + 1, c)` 和 `(r - 1, c)` ，前提是相邻格子在网格图内。

__示例:__

示例 1：

![2658-3](https://assets.leetcode.com/uploads/2023/03/29/example.png)

```text
_输入:_ grid = [[0,2,1,0],[4,0,0,3],[1,0,0,4],[0,3,2,0]]
 _输出:_ 7
 _解释:_ 渔夫可以从格子 (1,3) 出发，捕捞 3 条鱼，然后移动到格子 (2,3) ，捕捞 4 条鱼。
```

示例 2：

![2658-4](https://assets.leetcode.com/uploads/2023/03/29/example2.png)

```text
输入：grid = [[1,0,0,0],[0,0,0,0],[0,0,0,0],[0,0,0,1]]
输出：1
解释：渔夫可以从格子 (0,0) 或者 (3,3) ，捕捞 1 条鱼。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `0 <= grid[i][j] <= 10`

__思路:__

```text
DFS
从任何一个有鱼的格子出发
分别向四个方向进行深度优先搜索
如果遇到边界或者陆地格子则返回 0
如果遇到水域格子则累加当前格子的鱼数
并将该格子置为 0, 避免重复访问
最后返回累加的鱼数
统计最大值
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMaxFish(vector<vector<int>> &grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<vector<int>> dirs{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        auto dfs = [&](this auto&& dfs, int x, int y) -> int 
        {
            if (x < 0 or x >= m or y < 0 or y >= n or !grid[x][y]) return 0;
            int cur = grid[x][y];
            grid[x][y] = 0;
            for (const auto &d : dirs) cur += dfs(x + d.front(), y + d.back());
            return cur;
        };
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result = max(result, dfs(i, j));
        return result;
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int findMaxFish(vector<vector<int>> &grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<vector<int>> dirs{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        auto dfs = [&](this auto&& dfs, int x, int y) -> int 
        {
            if (x < 0 or x >= m or y < 0 or y >= n or !grid[x][y]) return 0;
            int cur = grid[x][y];
            grid[x][y] = 0;
            for (const auto &d : dirs) cur += dfs(x + d.front(), y + d.back());
            return cur;
        };
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result = max(result, dfs(i, j));
        return result;
    }
};
```

__Python__:

```Python
class Solution 
{
public:
    int findMaxFish(vector<vector<int>> &grid) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<vector<int>> dirs{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        auto dfs = [&](this auto&& dfs, int x, int y) -> int 
        {
            if (x < 0 or x >= m or y < 0 or y >= n or !grid[x][y]) return 0;
            int cur = grid[x][y];
            grid[x][y] = 0;
            for (const auto &d : dirs) cur += dfs(x + d.front(), y + d.back());
            return cur;
        };
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) result = max(result, dfs(i, j));
        return result;
    }
};
```
