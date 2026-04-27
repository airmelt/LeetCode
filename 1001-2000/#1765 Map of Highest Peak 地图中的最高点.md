# 1765 Map of Highest Peak 地图中的最高点

__Description:__

You are given an integer matrix `isWater` of size `m x n` that represents a map of __land__ and __water__ cells.

- If `isWater[i][j] == 0`, cell `(i, j)` is a __land__ cell.
- If `isWater[i][j] == 1`, cell `(i, j)` is a __water__ cell.

You must assign each cell a height in a way that follows these rules:

- The height of each cell must be non-negative.
- If the cell is a __water__ cell, its height must be `0`.
- Any two adjacent cells must have an absolute height difference of __at most__ `1`. A cell is adjacent to another cell if the former is directly north, east, south, or west of the latter (i.e., their sides are touching).

Find an assignment of heights such that the maximum height in the matrix is maximized.

Return _an integer matrix_ `height` _of size_ `m x n` _where_ `height[i][j]` _is cell_ `(i, j)`_'s height. If there are multiple solutions, return __any__ of them_.

__Example:__

Example 1:

![1765-1](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82045-am.png)

```text
Input: isWater = [[0,1],[0,0]]
Output: [[1,0],[2,1]]
Explanation: The image shows the assigned heights of each cell.
The blue cell is the water cell, and the green cells are the land cells.
```

Example 2:

![1765-2](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82050-am.png)

```text
Input: isWater = [[0,0,1],[1,0,0],[0,0,0]]
Output: [[1,1,0],[0,1,1],[1,2,2]]
Explanation: A height of 2 is the maximum possible height of any assignment.
Any height assignment that has a maximum height of 2 while still meeting the rules will also be accepted.
```

__Constraints:__

- `m == isWater.length`
- `n == isWater[i].length`
- `1 <= m, n <= 1000`
- `isWater[i][j]` is `0` or `1`.
- There is at least __one__ water cell.

__题目描述:__

给你一个大小为 `m x n` 的整数矩阵 `isWater` ，它代表了一个由 __陆地__ 和 __水域__ 单元格组成的地图。

- 如果 `isWater[i][j] == 0` ，格子 `(i, j)` 是一个 __陆地__ 格子。
- 如果 `isWater[i][j] == 1` ，格子 `(i, j)` 是一个 __水域__ 格子。

你需要按照如下规则给每个单元格安排高度：

- 每个格子的高度都必须是非负的。
- 如果一个格子是 __水域__ ，那么它的高度必须为 `0` 。
- 任意相邻的格子高度差 __至多__ 为 `1` 。当两个格子在正东、南、西、北方向上相互紧挨着，就称它们为相邻的格子。（也就是说它们有一条公共边）

找到一种安排高度的方案，使得矩阵中的最高高度值 最大 。

请你返回一个大小为 `m x n` 的整数矩阵 `height` ，其中 `height[i][j]` 是格子 `(i, j)` 的高度。如果有多种解法，请返回 __任意一个__ 。

__示例:__

示例 1：

![1765-3](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82045-am.png)

```text
输入：isWater = [[0,1],[0,0]]
输出：[[1,0],[2,1]]
解释：上图展示了给各个格子安排的高度。
蓝色格子是水域格，绿色格子是陆地格。
```

示例 2：

![1765-4](https://assets.leetcode.com/uploads/2021/01/10/screenshot-2021-01-11-at-82050-am.png)

```text
输入：isWater = [[0,0,1],[1,0,0],[0,0,0]]
输出：[[1,1,0],[0,1,1],[1,2,2]]
解释：所有安排方案中，最高可行高度为 2 。
任意安排方案中，只要最高高度为 2 且符合上述规则的，都为可行方案。
```

__提示：__

- `m == isWater.length`
- `n == isWater[i].length`
- `1 <= m, n <= 1000`
- `isWater[i][j]` 要么是 `0` ，要么是 `1` 。
- 至少有 __1__ 个水域格子。

__思路:__

```text
BFS
将所有水源加入队列
从每个水源出发开始 BFS
将水源周围没被计算过的陆地高度加 1, 并将陆地加入队列
接着遍历队列中的陆地, 重复上述过程
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) 
    {
        int m = isWater.size(), n = isWater.front().size(), x = 0, y = 0;
        vector<vector<int>> result(m, vector<int>(n)), d{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        queue<pair<int, int>> q;
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                result[i][j] = isWater[i][j] - 1;
                if (isWater[i][j]) q.push({i, j});
            }
        }
        while (!q.empty()) 
        {
            int i = q.front().first, j = q.front().second;
            q.pop();
            for (const auto& delta : d) 
            {
                if ((x = delta.front() + i) < m and x > -1 and (y = delta.back() + j) < n and y > -1 and result[x][y] == -1) 
                {
                    result[x][y] = result[i][j] + 1;
                    q.push({x, y});
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
    public int[][] highestPeak(int[][] isWater) {
        int m = isWater.length, n = isWater[0].length, result[][] = new int[m][n], d[][] = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, x = 0, y = 0;
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                result[i][j] = isWater[i][j] - 1;
                if (isWater[i][j] == 1) queue.offer(new int[]{i, j});
            }
        }
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            for (int[] delta : d) {
                if ((x = delta[0] + cur[0]) < m && x > -1 && (y = delta[1] + cur[1]) < n && y > -1 && result[x][y] == -1) {
                    result[x][y] = result[cur[0]][cur[1]] + 1;
                    queue.offer(new int[]{x, y});
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
    def highestPeak(self, isWater: List[List[int]]) -> List[List[int]]:
        m, n, result, q = len(isWater), len(isWater[0]), [[water - 1 for water in row] for row in isWater], deque((i, j) for i, row in enumerate(isWater) for j, water in enumerate(row) if water)
        while q:
            i, j = q.popleft()
            for x, y in ((i - 1, j), (i + 1, j), (i, j - 1), (i, j + 1)):
                if -1 < x < m and -1 < y < n and result[x][y] == -1:
                    result[x][y] = result[i][j] + 1
                    q.append((x, y))
        return result
```
