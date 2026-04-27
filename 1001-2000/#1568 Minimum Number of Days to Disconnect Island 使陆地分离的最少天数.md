# 1568 Minimum Number of Days to Disconnect Island 使陆地分离的最少天数

__Description:__

You are given an `m x n` binary grid `grid` where `1` represents land and `0` represents water. An __island__ is a maximal __4-directionally__ (horizontal or vertical) connected group of `1`'s.

The grid is said to be connected if we have exactly one island, otherwise is said disconnected.

In one day, we are allowed to change __any__ single land cell `(1)` into a water cell `(0)`.

Return the minimum number of days to disconnect the grid.

__Example:__

Example 1:

![1568-1](https://assets.leetcode.com/uploads/2021/12/24/land1.jpg)

```text
Input: grid = [[0,1,1,0],[0,1,1,0],[0,0,0,0]]

Output: 2
Explanation: We need at least 2 days to get a disconnected grid.
Change land grid[1][1] and grid[0][2] to water and get 2 disconnected island.
```

Example 2:

![1568-2](https://assets.leetcode.com/uploads/2021/12/24/land2.jpg)

```text
Input: grid = [[1,1]]
Output: 2
Explanation: Grid of full water is also disconnected ([[1,1]] -> [[0,0]]), 0 islands.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 30`
- `grid[i][j]` is either `0` or `1`.

__题目描述:__

给你一个大小为 `m x n` ，由若干 `0` 和 `1` 组成的二维网格 `grid` ，其中 `1` 表示陆地， `0` 表示水。__岛屿__ 由水平方向或竖直方向上相邻的 `1` （陆地）连接形成。

如果 恰好只有一座岛屿 ，则认为陆地是 连通的 ；否则，陆地就是 分离的 。

一天内，可以将 __任何单个__ 陆地单元（ `1`）更改为水单元（ `0`）。

返回使陆地分离的最少天数。

__示例:__

示例 1：

![1568-3](https://assets.leetcode.com/uploads/2021/12/24/land1.jpg)

```text
输入：grid = [[0,1,1,0],[0,1,1,0],[0,0,0,0]]
输出：2
解释：至少需要 2 天才能得到分离的陆地。
将陆地 grid[1][1] 和 grid[0][2] 更改为水，得到两个分离的岛屿。
```

示例 2：

![1568-4](https://assets.leetcode.com/uploads/2021/12/24/land2.jpg)

```text
输入：grid = [[1,1]]
输出：2
解释：如果网格中都是水，也认为是分离的 ([[1,1]] -> [[0,0]])，0 岛屿。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 30`
- `grid[i][j]` 为 `0` 或 `1`

__思路:__

```text
1. 模拟
对于该题答案只能为 0, 1, 2
因为无论如何, 左下角的陆地, 最多可以移除 2 块陆地就能与主大陆分离
首先检查所有陆地是否只有 1 块, 如果不是直接返回 0
对每块陆地进行移除操作, 检查陆地是否多于 1 块, 如果是, 返回 1
否则返回 2
时间复杂度为 O(M ^ 2N ^ 2), 空间复杂度为 O(MN)
2. Tarjan 法 ➕ 并查集
如果能够找到 Tarjan 的割点
从割点分离可以将陆地分成 2 块, 返回 1
首先用并查集检查所有陆地的分布情况
然后使用 Tarjan 找割点
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
    
    void dfs(int x, int y, vector<vector<int>>& grid, int m, int n)
    {
        grid[x][y] = 2;
        for (int i = 0; i < 4; i++) 
        {
            int tx = dx[i] + x, ty = dy[i] + y;
            if (tx < 0 or tx >= m or ty < 0 or ty >= n or grid[tx][ty] != 1) continue;
            dfs(tx, ty, grid, m, n);
        }
    }
    
    int count(vector<vector<int>>& grid, int m, int n) 
    {
        int result = 0;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] == 1) 
                {
                    ++result;
                    dfs(i, j, grid, m, n);
                }
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 2) grid[i][j] = 1;
        return result;
    }
public:
    int minDays(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        if (count(grid, m, n) != 1) return 0;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j]) 
                {
                    grid[i][j] = 0;
                    if (count(grid, m, n) != 1) return 1;
                    grid[i][j] = 1;
                }
            }
        }
        return 2;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] dx = new int[]{0, 1, 0, -1}, dy = new int[]{1, 0, -1, 0};
    private void dfs(int x, int y, int[][] grid, int m, int n) {
        grid[x][y] = 2;
        for (int i = 0; i < 4; i++) {
            int tx = dx[i] + x, ty = dy[i] + y;
            if (tx < 0 || tx >= m || ty < 0 || ty >= n || grid[tx][ty] != 1) continue;
            dfs(tx, ty, grid, m, n);
        }
    }
    
    private int count(int[][] grid, int m, int n) {
        int result = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    ++result;
                    dfs(i, j, grid, m, n);
                }
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (grid[i][j] == 2) grid[i][j] = 1;
        return result;
    }
    
    public int minDays(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        if (count(grid, m, n) != 1) return 0;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (grid[i][j] != 0) 
                {
                    grid[i][j] = 0;
                    if (count(grid, m, n) != 1) return 1;
                    grid[i][j] = 1;
                }
            }
        }
        return 2;
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = n

    def union(self, p: int, q: int) -> None:
        """
        连接两个点
        :param p: 一个节点
        :param q: 另一个节点
        :return:
        """
        p = self.find(p)
        q = self.find(q)
        if p == q:
            return
        if self.weight[p] > self.weight[q]:
            self.parent[q] = p
            self.weight[p] += self.weight[q]
        else:
            self.parent[p] = q
            self.weight[q] += self.weight[p]
        self.count -= 1

    def connected(self, p: int, q: int) -> bool:
        """
        检查两个点是否在同一分量
        :param p: 一个节点
        :param q: 另一个节点
        :return: 返回两个点是否在同一个分量
        """
        return self.find(p) == self.find(q)

    def find(self, p: int) -> int:
        """
        查找根节点, 并进行路径压缩
        :param p: 一个节点
        :return: 根节点
        """
        while self.parent[p] != p:
            self.parent[p] = self.parent[self.parent[p]]
            p = self.parent[p]
        return p

class Solution:
    def minDays(self, grid: List[List[int]]) -> int:
        m, n, d, island_pos, land_count, map_pos, pre_connect = len(grid), len(grid[0]), [(-1, 0), (0, 1), (1, 0), (0, -1)], None, 0, lambda t: t[0] * n + t[1], -1
        uf = UF(m * n)
        for i in range(m):
            for j in range(n):
                if grid[i][j]:
                    island_pos = (i, j)
                    land_count += 1
                    for dx, dy in d:
                        x, y = i + dx, j + dy
                        if -1 < x < m and -1 < y < n and grid[x][y] == 1:
                            uf.union(map_pos((i, j)), map_pos((x, y)))
        if not island_pos:
            return 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    p = uf.find(map_pos((i, j)))
                    if pre_connect == -1:
                        pre_connect = p
                    elif pre_connect != p:
                        return 0
        if land_count == 1:
            return 1
        dfn, low = [0] * (m * n), [0] * (m * n)
        
        def tarjan(u_pos: int, parent: int=None, tick: int=1) -> bool:
            u = map_pos(u_pos)
            dfn[u] = low[u] = tick
            child_cnt = 0
            for dx, dy in d:
                v_pos = (u_pos[0] + dx, u_pos[1] + dy)
                if not (-1 < v_pos[0] < m and -1 < v_pos[1] < n and grid[v_pos[0]][v_pos[1]] == 1): 
                    continue
                v = map_pos(v_pos)
                if dfn[v] == 0:
                    if tarjan(v_pos, u_pos, tick + 1):
                        return True
                    low[u] = min(low[u], low[v])
                    child_cnt += 1
                    q1 = parent is None and child_cnt >= 2
                    q2 = parent and low[v] >= dfn[u]
                    if q1 or q2:  
                        return True
                elif v != parent:
                    low[u] = min(low[u], dfn[v])
            return False
        return 1 if tarjan(island_pos) else 2
```
