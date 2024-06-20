# 2146 K Highest Ranked Items Within a Price Range 价格范围内最高排名的 K 样物品

__Description:__

You are given a __0-indexed__ 2D integer array `grid` of size `m x n` that represents a map of the items in a shop. The integers in the grid represent the following:

- `0` represents a wall that you cannot pass through.
- `1` represents an empty cell that you can freely move to and from.
- All other positive integers represent the price of an item in that cell. You may also freely move to and from these item cells.

It takes `1` step to travel between adjacent grid cells.

You are also given integer arrays `pricing` and `start` where `pricing = [low, high]` and `start = [row, col]` indicates that you start at the position `(row, col)` and are interested only in items with a price in the range of `[low, high]` (__inclusive__). You are further given an integer `k`.

You are interested in the __positions__ of the `k` __highest-ranked__ items whose prices are __within__ the given price range. The rank is determined by the __first__ of these criteria that is different:

Return _the_ `k` _highest-ranked items within the price range __sorted__ by their rank (highest to lowest)_. If there are fewer than `k` reachable items within the price range, return ___all__ of them_.

__Example:__

Example 1:

![2146-1](https://assets.leetcode.com/uploads/2021/12/16/example1drawio.png)

```text
Input: grid = [[1,2,0,1],[1,3,0,1],[0,2,5,1]], pricing = [2,5], start = [0,0], k = 3
Output: [[0,1],[1,1],[2,1]]
Explanation: You start at (0,0).
With a price range of [2,5], we can take items from (0,1), (1,1), (2,1) and (2,2).
The ranks of these items are:
```

- (0,1) with distance 1
- (1,1) with distance 2
- (2,1) with distance 3
- (2,2) with distance 4

Thus, the 3 highest ranked items in the price range are (0,1), (1,1), and (2,1).

Example 2:

![2146-2](https://assets.leetcode.com/uploads/2021/12/16/example2drawio1.png)

```text
Input: grid = [[1,2,0,1],[1,3,3,1],[0,2,5,1]], pricing = [2,3], start = [2,3], k = 2
Output: [[2,1],[1,2]]
Explanation: You start at (2,3).
With a price range of [2,3], we can take items from (0,1), (1,1), (1,2) and (2,1).
The ranks of these items are:
```

- (2,1) with distance 2, price 2
- (1,2) with distance 2, price 3
- (1,1) with distance 3
- (0,1) with distance 4

Thus, the 2 highest ranked items in the price range are (2,1) and (1,2).

Example 3:

![2146-3](https://assets.leetcode.com/uploads/2021/12/30/example3.png)

```text
Input: grid = [[1,1,1],[0,0,1],[2,3,4]], pricing = [2,3], start = [0,0], k = 3
Output: [[2,1],[2,0]]
Explanation: You start at (0,0).
With a price range of [2,3], we can take items from (2,0) and (2,1). 
The ranks of these items are: 
```

- (2,1) with distance 5
- (2,0) with distance 6

Thus, the 2 highest ranked items in the price range are (2,1) and (2,0).
Note that k = 3 but there are only 2 reachable items within the price range.

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] <= 10 ^ 5`
- `pricing.length == 2`
- `2 <= low <= high <= 10 ^ 5`
- `start.length == 2`
- `0 <= row <= m - 1`
- `0 <= col <= n - 1`
- `grid[row][col] > 0`
- `1 <= k <= m * n`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `grid` ，它的大小为 `m x n` ，表示一个商店中物品的分布图。数组中的整数含义为:

- `0` 表示无法穿越的一堵墙。
- `1` 表示可以自由通过的一个空格子。
- 所有其他正整数表示该格子内的一样物品的价格。你可以自由经过这些格子。

从一个格子走到上下左右相邻格子花费 `1` 步。

同时给你一个整数数组 `pricing` 和 `start` ，其中 `pricing = [low, high]` 且 `start = [row, col]` ，表示你开始位置为 `(row, col)` ，同时你只对物品价格在 __闭区间__ `[low, high]` 之内的物品感兴趣。同时给你一个整数 `k` 。

你想知道给定范围 __内__ 且 __排名最高__ 的 `k` 件物品的 __位置__ 。排名按照优先级从高到低的以下规则制定:

请你返回给定价格内排名最高的 `k` 件物品的坐标，将它们按照排名排序后返回。如果给定价格内少于 `k` 件物品，那么请将它们的坐标 __全部__ 返回。

__示例:__

示例 1：

![2146-4](https://assets.leetcode.com/uploads/2021/12/16/example1drawio.png)

```text
输入：grid = [[1,2,0,1],[1,3,0,1],[0,2,5,1]], pricing = [2,5], start = [0,0], k = 3
输出：[[0,1],[1,1],[2,1]]
解释：起点为 (0,0) 。
价格范围为 [2,5] ，我们可以选择的物品坐标为 (0,1)，(1,1)，(2,1) 和 (2,2) 。
这些物品的排名为：
```

- (0,1) 距离为 1
- (1,1) 距离为 2
- (2,1) 距离为 3
- (2,2) 距离为 4

所以，给定价格范围内排名最高的 3 件物品的坐标为 (0,1)，(1,1) 和 (2,1) 。

示例 2：

![2146-5](https://assets.leetcode.com/uploads/2021/12/16/example2drawio1.png)

```text
输入：grid = [[1,2,0,1],[1,3,3,1],[0,2,5,1]], pricing = [2,3], start = [2,3], k = 2
输出：[[2,1],[1,2]]
解释：起点为 (2,3) 。
价格范围为 [2,3] ，我们可以选择的物品坐标为 (0,1)，(1,1)，(1,2) 和 (2,1) 。
这些物品的排名为： 
```

- (2,1) 距离为 2 ，价格为 2
- (1,2) 距离为 2 ，价格为 3
- (1,1) 距离为 3
- (0,1) 距离为 4

所以，给定价格范围内排名最高的 2 件物品的坐标为 (2,1) 和 (1,2) 。

示例 3：

![2146-6](https://assets.leetcode.com/uploads/2021/12/30/example3.png)

```text
输入：grid = [[1,1,1],[0,0,1],[2,3,4]], pricing = [2,3], start = [0,0], k = 3
输出：[[2,1],[2,0]]
解释：起点为 (0,0) 。
价格范围为 [2,3] ，我们可以选择的物品坐标为 (2,0) 和 (2,1) 。
这些物品的排名为：
```

- (2,1) 距离为 5
- (2,0) 距离为 6

所以，给定价格范围内排名最高的 2 件物品的坐标为 (2,1) 和 (2,0) 。
注意，k = 3 但给定价格范围内只有 2 件物品。

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- `0 <= grid[i][j] <= 10 ^ 5`
- `pricing.length == 2`
- `2 <= low <= high <= 10 ^ 5`
- `start.length == 2`
- `0 <= row <= m - 1`
- `0 <= col <= n - 1`
- `grid[row][col] > 0`
- `1 <= k <= m * n`

__思路:__

```text
BFS
按照距离由近及远进行 BFS
可以先将距离相同的点按照价格, 行列排序
也可以先将所有在价格范围内的点找出来, 然后按照价格, 行列排序
每次访问过的点需要记录下来不再访问
时间复杂度为 O(MNlogMN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

class Solution {
public:
    vector<vector<int>>
    highestRankedKItems(vector<vector<int>> &grid, vector<int> &pricing, vector<int> &start, int k) {
        vector<vector<int>> result;
        int m = grid.size(), n = grid[0].size(), low = pricing.front(), high = pricing.back(), sx = start.front(), sy = start.back();
        bool visited[m][n];
        memset(visited, 0, sizeof(visited));
        visited[sx][sy] = true;
        vector<pair<int, int>> q = {{sx, sy}};
        while (!q.empty()) 
        {
            sort(q.begin(), q.end(), [&](auto &a, auto &b) { return grid[a.first][a.second] < grid[b.first][b.second] or grid[a.first][a.second] == grid[b.first][b.second] and a < b; });
            for (auto &p: q) 
            {
                if (low <= grid[p.first][p.second] and grid[p.first][p.second] <= high) 
                {
                    result.push_back({p.first, p.second});
                    if (result.size() == k) return result;
                }
            }
            vector<pair<int, int>> cur;
            for (auto &p: q)
                for (auto &d: dirs) 
                {
                    int x = p.first + d[0], y = p.second + d[1];
                    if (-1 < x and x < m and -1 < y and y < n and !visited[x][y] and grid[x][y]) 
                    {
                        visited[x][y] = true;
                        cur.emplace_back(x, y);
                    }
                }
            q = move(cur);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    static final int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public List<List<Integer>> highestRankedKItems(int[][] grid, int[] pricing, int[] start, int k) {
        var result = new ArrayList<List<Integer>>();
        int m = grid.length, n = grid[0].length;
        var visited = new boolean[m][n];
        visited[start[0]][start[1]] = true;
        var q = new ArrayList<int[]>();
        q.add(start);
        while (!q.isEmpty()) {
            q.sort((a, b) -> { return grid[a[0]][a[1]] != grid[b[0]][b[1]] ? grid[a[0]][a[1]] - grid[b[0]][b[1]] : a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]; });
            for (var p : q)
                if (pricing[0] <= grid[p[0]][p[1]] && grid[p[0]][p[1]] <= pricing[1]) {
                    result.add(List.of(p[0], p[1]));
                    if (result.size() == k) return result;
                }
            var cur = q;
            q = new ArrayList<>();
            for (var p : cur)
                for (var d : dirs) {
                    int x = p[0] + d[0], y = p[1] + d[1];
                    if (-1 < x && x < m && -1 < y && y < n && !visited[x][y] && grid[x][y] > 0) {
                        visited[x][y] = true;
                        q.add(new int[]{x, y});
                    }
                }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def highestRankedKItems(self, grid: List[List[int]], pricing: List[int], start: List[int], k: int) -> List[List[int]]:
        q, result, d, m, n, grid[start[0]][start[1]], price = deque([(start[0], start[1])]), [start] if pricing[0] <= grid[start[0]][start[1]] <= pricing[1] else [], [(0, 1), (0, -1), (1, 0), (-1, 0)], len(grid), len(grid[0]), -1, deepcopy(grid)
        while q and len(result) < k:
            q_size, cur = len(q), []
            for _ in range(q_size):
                x, y = q.popleft()
                for dx, dy in d:
                    i, j = x + dx, y + dy
                    if -1 < i < m and -1 < j < n and grid[i][j] > 0:
                        if pricing[0] <= grid[i][j] <= pricing[1]:
                            cur.append([i, j])
                        q.append((i, j))
                        grid[i][j] = -1
            cur.sort(key=lambda x: (price[x[0]][x[1]], x[0], x[1]))
            result += cur
        return result[:k]
```
