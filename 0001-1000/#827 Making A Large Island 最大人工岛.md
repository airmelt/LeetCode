# 827 Making A Large Island 最大人工岛

__Description__:
You are given an n x n binary matrix grid. You are allowed to change at most one 0 to be 1.

Return the size of the largest island in grid after applying this operation.

An island is a 4-directionally connected group of 1s.

__Example:__

Example 1:

Input: grid = [[1,0],[0,1]]
Output: 3
Explanation: Change one 0 to 1 and connect two 1s, then we get an island with area = 3.

Example 2:

Input: grid = [[1,1],[1,0]]
Output: 4
Explanation: Change the 0 to 1 and make the island bigger, only one island with area = 4.

Example 3:

Input: grid = [[1,1],[1,1]]
Output: 4
Explanation: Can't change any 0 to 1, only one island with area = 4.

__Constraints:__

n == grid.length
n == grid[i].length
1 <= n <= 500
grid[i][j] is either 0 or 1.

__题目描述__:
给你一个大小为 n x n 二进制矩阵 grid 。最多 只能将一格 0 变成 1 。

返回执行此操作后，grid 中最大的岛屿面积是多少？

岛屿 由一组上、下、左、右四个方向相连的 1 形成。

__示例 :__

示例 1:

输入: grid = [[1, 0], [0, 1]]
输出: 3
解释: 将一格0变成1，最终连通两个小岛得到面积为 3 的岛屿。

示例 2:

输入: grid = [[1, 1], [1, 0]]
输出: 4
解释: 将一格0变成1，岛屿的面积扩大为 4。

示例 3:

输入: grid = [[1, 1], [1, 1]]
输出: 4
解释: 没有0可以让我们变成1，面积依然为 4。

__提示:__

n == grid.length
n == grid[i].length
1 <= n <= 500
grid[i][j] 为 0 或 1

__思路__:

DFS
先按照岛屿将这些岛分块标记上不同的值, 记录当前最大的岛屿的面积
然后将 0 反转为 1 再查找邻居并加入, 更新最大面积
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
private:
    vector<pair<int, int>> d{ {-1, 0}, {1, 0}, {0, 1}, {0, -1} };
    
    int dfs(vector<vector<int>>& grid, int n, int r, int c, int islands) 
    {
        int result = 1;
        grid[r][c] = islands;
        for (const auto& move: neighbors(r, c, n)) 
        {
            if (grid[move / n][move % n] == 1) 
            {
                grid[move / n][move % n] = islands;
                result += dfs(grid, n, move / n, move % n, islands);
            }
        }
        return result;
    }

    vector<int> neighbors(int r, int c, int n) 
    {
        vector<int> result;
        for (const auto& p : d) 
        {
            int nr = r + p.first, nc = c + p.second;
            if (-1 < nr and nr < n and -1 < nc and nc < n) result.emplace_back(nr * n + nc);
        }
        return result;
    }
public:
    int largestIsland(vector<vector<int>>& grid) 
    {
        int n = grid.size(), islands = 2, result = 0;
        vector<int> area(n * n + islands);
        for (int r = 0; r < n; r++) for (int c = 0; c < n; c++) if (grid[r][c] == 1) 
        {
            area[islands] = dfs(grid, n, r, c, islands);
            islands++;
        }
        for (const auto& x : area) result = max(result, x);
        for (int r = 0; r < n; r++) 
        {
            for (int c = 0; c < n; c++) 
            {
                if (!grid[r][c]) 
                {
                    unordered_set<int> visited;
                    for (const auto& move: neighbors(r, c, n)) if (grid[move / n][move % n] > 1) visited.insert(grid[move / n][move % n]);
                    int s = 1;
                    for (const auto& i : visited) s += area[i];
                    result = max(result, s);
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int dr[] = new int[]{ -1, 0, 1, 0 }, dc[] = new int[]{ 0, -1, 0, 1 }, n, grid[][];
    public int largestIsland(int[][] grid) {
        this.grid = grid;
        n = grid.length;
        int index = 2, area[] = new int[n * n + index], result = 0;
        for (int r = 0; r < n; ++r) for (int c = 0; c < n; ++c) if (grid[r][c] == 1) area[index] = dfs(r, c, index++);
        for (int x : area) result = Math.max(result, x);
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < n; c++) {
                if (grid[r][c] == 0) {
                    Set<Integer> visited = new HashSet();
                    for (Integer move: neighbors(r, c)) if (grid[move / n][move % n] > 1) visited.add(grid[move / n][move % n]);
                    int s = 1;
                    for (int i : visited) s += area[i];
                    result = Math.max(result, s);
                }
            }
        }
        return result;
    }
    
    private int dfs(int r, int c, int index) {
        int result = 1;
        grid[r][c] = index;
        for (Integer move: neighbors(r, c)) {
            if (grid[move / n][move % n] == 1) {
                grid[move / n][move % n] = index;
                result += dfs(move / n, move % n, index);
            }
        }
        return result;
    }

    public List<Integer> neighbors(int r, int c) {
        List<Integer> result = new ArrayList();
        for (int k = 0; k < 4; ++k) {
            int nr = r + dr[k], nc = c + dc[k];
            if (-1 < nr && nr < n && -1 < nc && nc < n) result.add(nr * n + nc);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:

        def neighbors(r: int, c: int) -> tuple:
            for nr, nc in ((r - 1, c), (r + 1, c), (r, c - 1), (r, c + 1)):
                if -1 < nr < n and -1 < nc < n:
                    yield nr, nc

        def dfs(r: int, c: int, island: int) -> int:
            grid[r][c], result = island, 1
            for nr, nc in neighbors(r, c):
                if grid[nr][nc] == 1:
                    result += dfs(nr, nc, island)
            return result
        
        area, island, n = {}, 2, len(grid)
        for r in range(n):
            for c in range(n):
                if grid[r][c] == 1:
                    area[island] = dfs(r, c, island)
                    island += 1
        result = max(area.values() or [0])
        for r in range(n):
            for c in range(n):
                if not grid[r][c]:
                    visited = { grid[nr][nc] for nr, nc in neighbors(r, c) if grid[nr][nc] > 1 }
                    result = max(result, 1 + sum(area[i] for i in visited))
        return result
```
