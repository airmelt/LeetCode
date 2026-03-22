# 2812 Find the Safest Path in a Grid 找出最安全路径

__Description:__

You are given a __0-indexed__ 2D matrix `grid` of size `n x n`, where `(r, c)` represents:

- A cell containing a thief if `grid[r][c] = 1`
- An empty cell if `grid[r][c] = 0`

You are initially positioned at cell `(0, 0)`. In one move, you can move to any adjacent cell in the grid, including cells containing thieves.

The safeness factor of a path on the grid is defined as the minimum manhattan distance from any cell in the path to any thief in the grid.

Return _the __maximum safeness factor__ of all paths leading to cell_ `(n - 1, n - 1)`_._

An __adjacent__ cell of cell `(r, c)`, is one of the cells `(r, c + 1)`, `(r, c - 1)`, `(r + 1, c)` and `(r - 1, c)` if it exists.

The __Manhattan distance__ between two cells `(a, b)` and `(x, y)` is equal to `|a - x| + |b - y|`, where `|val|` denotes the absolute value of val.

__Example:__

Example 1:

![2812-1](https://assets.leetcode.com/uploads/2023/07/02/example1.png)

```text
Input: grid = [[1,0,0],[0,0,0],[0,0,1]]
Output: 0
Explanation: All paths from (0, 0) to (n - 1, n - 1) go through the thieves in cells (0, 0) and (n - 1, n - 1).
```

Example 2:

![2812-2](https://assets.leetcode.com/uploads/2023/07/02/example2.png)

```text
Input: grid = [[0,0,1],[0,0,0],[0,0,0]]
Output: 2
Explanation: The path depicted in the picture above has a safeness factor of 2 since:
```

- The closest cell of the path to the thief at cell (0, 2) is cell (0, 0). The distance between them is | 0 - 0 | + | 0 - 2 | = 2.

It can be shown that there are no other paths with a higher safeness factor.

Example 3:

![2812-3](https://assets.leetcode.com/uploads/2023/07/02/example3.png)

```text
Input: grid = [[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]
Output: 2
Explanation: The path depicted in the picture above has a safeness factor of 2 since:
```

- The closest cell of the path to the thief at cell (0, 3) is cell (1, 2). The distance between them is | 0 - 1 | + | 3 - 2 | = 2.
- The closest cell of the path to the thief at cell (3, 0) is cell (3, 2). The distance between them is | 3 - 3 | + | 0 - 2 | = 2.

It can be shown that there are no other paths with a higher safeness factor.

__Constraints:__

- `1 <= grid.length == n <= 400`
- `grid[i].length == n`
- `grid[i][j]` is either `0` or `1`.
- There is at least one thief in the `grid`.

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `n x n` 的二维矩阵 `grid` ，其中 `(r, c)` 表示:

- 如果 `grid[r][c] = 1` ，则表示一个存在小偷的单元格
- 如果 `grid[r][c] = 0` ，则表示一个空单元格

你最开始位于单元格 `(0, 0)` 。在一步移动中，你可以移动到矩阵中的任一相邻单元格，包括存在小偷的单元格。

矩阵中路径的 安全系数 定义为：从路径中任一单元格到矩阵中任一小偷所在单元格的 最小 曼哈顿距离。

返回所有通向单元格 `(n - 1, n - 1)` 的路径中的 __最大安全系数__ 。

单元格 `(r, c)` 的某个 __相邻__ 单元格，是指在矩阵中存在的 `(r, c + 1)`、 `(r, c - 1)`、 `(r + 1, c)` 和 `(r - 1, c)` 之一。

两个单元格 `(a, b)` 和 `(x, y)` 之间的 __曼哈顿距离__ 等于 `| a - x | + | b - y |` ，其中 `|val|` 表示 `val` 的绝对值。

__示例:__

示例 1：

![2812-4](https://assets.leetcode.com/uploads/2023/07/02/example1.png)

```text
输入：grid = [[1,0,0],[0,0,0],[0,0,1]]
输出：0
解释：从 (0, 0) 到 (n - 1, n - 1) 的每条路径都经过存在小偷的单元格 (0, 0) 和 (n - 1, n - 1) 。
```

示例 2：

![2812-5](https://assets.leetcode.com/uploads/2023/07/02/example2.png)

```text
输入：grid = [[0,0,1],[0,0,0],[0,0,0]]
输出：2
解释：
上图所示路径的安全系数为 2：
```

- 该路径上距离小偷所在单元格（0，2）最近的单元格是（0，0）。它们之间的曼哈顿距离为 | 0 - 0 | + | 0 - 2 | = 2 。

可以证明，不存在安全系数更高的其他路径。

示例 3：

![2812-6](https://assets.leetcode.com/uploads/2023/07/02/example3.png)

```text
输入：grid = [[0,0,0,1],[0,0,0,0],[0,0,0,0],[1,0,0,0]]
输出：2
解释：
上图所示路径的安全系数为 2：
```

- 该路径上距离小偷所在单元格（0，3）最近的单元格是（1，2）。它们之间的曼哈顿距离为 | 0 - 1 | + | 3 - 2 | = 2 。
- 该路径上距离小偷所在单元格（3，0）最近的单元格是（3，2）。它们之间的曼哈顿距离为 | 3 - 3 | + | 0 - 2 | = 2 。

可以证明，不存在安全系数更高的其他路径。

__提示：__

- `1 <= grid.length == n <= 400`
- `grid[i].length == n`
- `grid[i][j]` 为 `0` 或 `1`
- `grid` 至少存在一个小偷

__思路:__

```text
并查集 ➕ BFS
从小偷出发按照 BFS 依次找到距离为 1, 2, 3, ..., 的位置
这里使用多源 BFS 加快查找速度
将相同距离的点加入一个列表
从后往前遍历距离列表, 由于最后一个列表为空, 可以从倒数第二个开始
将距离一样及比当前点距离小点周围的点都加入并查集
检查起点终点是否联通
联通的时候即可以返回答案
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2), 并查集时间复杂度看作 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximumSafenessFactor(vector<vector<int>> &grid) 
    {
        int n = grid.size(), dir[4][2] = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } }, x = 0, y = 0;
        vector<pair<int, int>> q;
        vector<vector<int>> dis(n, vector<int>(n));
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (grid[i][j]) q.emplace_back(i, j); else dis[i][j] = -1;
        vector<vector<pair<int, int>>> groups = {q};
        while (!q.empty()) 
        {
            vector<pair<int, int>> cur;
            for (const auto& [i, j] : q)
            {
                for (const auto& d : dir)
                {
                    if (-1 < (x = i + d[0]) and x < n and -1 < (y = j + d[1]) and y < n and dis[x][y] < 0) 
                    {
                        cur.emplace_back(x, y);
                        dis[x][y] = groups.size();
                    }
                }
            }
            groups.emplace_back(cur);
            q = move(cur);
        }
        vector<int> fa(n * n);
        iota(fa.begin(), fa.end(), 0);
        auto find = [&](auto&& find, int p) -> int { return fa[p] == p ? fa[p] : (fa[p] = find(find, fa[p])); };
        for (int result = (int)groups.size() - 2; result > 0; result--) 
        {
            for (const auto& [i, j] : groups[result]) for (const auto& d : dir) if (-1 < (x = i + d[0]) and x < n and -1 < (y = j + d[1]) and y < n and dis[x][y] >= dis[i][j]) fa[find(find, n * x + y)] = find(find, n * i + j);
            if (find(find, 0) == find(find, n * n - 1)) return result;
        }
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSafenessFactor(List<List<Integer>> grid) {
        int n = grid.size(), dis[][] = new int[n][n], dir[][] = { { -1, 0 }, { 1, 0 }, { 0, -1 }, { 0, 1 } }, fa[] = new int[n * n], x = 0, y = 0, i = 0, j = 0;
        var q = new ArrayList<int[]>();
        for (i = 0; i < n; i++) for (j = 0; j < n; j++) if (grid.get(i).get(j) > 0) q.add(new int[]{ i, j }); else dis[i][j] = -1;
        var groups = new ArrayList<List<int[]>>();
        groups.add(q);
        while (!q.isEmpty()) {
            var cur = q;
            q = new ArrayList<>();
            for (var c : cur) {
                for (var d : dir) {
                    if (-1 < (x = c[0] + d[0]) && x < n && -1 < (y = c[1] + d[1]) && y < n && dis[x][y] < 0) {
                        q.add(new int[]{ x, y });
                        dis[x][y] = groups.size();
                    }
                }
            }
            groups.add(q);
        }
        for (i = 0; i < n * n; i++) fa[i] = i;
        for (int result = groups.size() - 2; result > 0; result--) {
            for (var g : groups.get(result)) for (var d : dir) if (-1 < (x = g[0] + d[0]) && x < n && -1 < (y = g[1] + d[1]) && y < n && dis[x][y] >= dis[i = g[0]][j = g[1]]) fa[find(n * x + y, fa)] = find(n * i + j, fa);
            if (find(0, fa) == find(n * n - 1, fa)) return result;
        }
        return 0;
    }

    private int find(int p, int[] fa) {
        return fa[p] == p ? fa[p] : (fa[p] = find(fa[p], fa));
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSafenessFactor(self, grid: List[List[int]]) -> int:
        n, q = len(grid), deque()
        dis, fa = [[-1] * n for _ in range(n)], list(range(n * n))
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x:
                    q.append((i, j))
                    dis[i][j] = 0
        groups = [q]
        while q:  
            size = len(q)
            for _ in range(size):
                i, j = q.popleft()
                for x, y in (i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1):
                    if -1 < x < n and -1 < y < n and dis[x][y] < 0:
                        q.append((x, y))
                        dis[x][y] = len(groups)
            groups.append(q.copy()) 

        def find(x: int) -> int:
            if fa[x] != x:
                fa[x] = find(fa[x])
            return fa[x]
        for d in range(len(groups) - 2, 0, -1):
            for i, j in groups[d]:
                for x, y in (i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1):
                    if -1 < x < n and -1 < y < n and dis[x][y] >= dis[i][j]:
                        fa[find(x * n + y)] = find(i * n + j)
            if find(0) == find(n * n - 1):
                return d
        return 0
```
