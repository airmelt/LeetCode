# 1368 Minimum Cost to Make at Least One Valid Path in a Grid 使网格图至少有一条有效路径的最小代价

__Description:__

Given an m x n grid. Each cell of the grid has a sign pointing to the next cell you should visit if you are currently in this cell. The sign of grid\[i][j] can be:

1 which means go to the cell to the right. (i.e go from grid\[i][j] to grid\[i][j + 1])
2 which means go to the cell to the left. (i.e go from grid\[i][j] to grid\[i][j - 1])
3 which means go to the lower cell. (i.e go from grid\[i][j] to grid\[i + 1][j])
4 which means go to the upper cell. (i.e go from grid\[i][j] to grid\[i - 1][j])
Notice that there could be some signs on the cells of the grid that point outside the grid.

You will initially start at the upper left cell (0, 0). A valid path in the grid is a path that starts from the upper left cell (0, 0) and ends at the bottom-right cell (m - 1, n - 1) following the signs on the grid. The valid path does not have to be the shortest.

You can modify the sign on a cell with cost = 1. You can modify the sign on a cell one time only.

Return the minimum cost to make the grid have at least one valid path.

__Example:__

Example 1:

![Grid 1](https://assets.leetcode.com/uploads/2020/02/13/grid1.png)

Input: grid = [[1,1,1,1],[2,2,2,2],[1,1,1,1],[2,2,2,2]]
Output: 3
Explanation: You will start at point (0, 0).
The path to (3, 3) is as follows. (0, 0) --> (0, 1) --> (0, 2) --> (0, 3) change the arrow to down with cost = 1 --> (1, 3) --> (1, 2) --> (1, 1) --> (1, 0) change the arrow to down with cost = 1 --> (2, 0) --> (2, 1) --> (2, 2) --> (2, 3) change the arrow to down with cost = 1 --> (3, 3)
The total cost = 3.

Example 2:

![Grid 2](https://assets.leetcode.com/uploads/2020/02/13/grid2.png)

Input: grid = [[1,1,3],[3,2,2],[1,1,4]]
Output: 0
Explanation: You can follow the path from (0, 0) to (2, 2).

Example 3:

![Grid 3](https://assets.leetcode.com/uploads/2020/02/13/grid3.png)

Input: grid = [[1,2],[4,3]]
Output: 1

__Constraints:__

m == grid.length
n == grid\[i].length
1 <= m, n <= 100
1 <= grid\[i][j] <= 4

__题目描述：__
给你一个 m x n 的网格图 grid 。 grid 中每个格子都有一个数字，对应着从该格子出发下一步走的方向。 grid\[i][j] 中的数字可能为以下几种情况：

1 ，下一步往右走，也就是你会从 grid\[i][j] 走到 grid\[i][j + 1]
2 ，下一步往左走，也就是你会从 grid\[i][j] 走到 grid\[i][j - 1]
3 ，下一步往下走，也就是你会从 grid\[i][j] 走到 grid\[i + 1][j]
4 ，下一步往上走，也就是你会从 grid\[i][j] 走到 grid\[i - 1][j]
注意网格图中可能会有 无效数字 ，因为它们可能指向 grid 以外的区域。

一开始，你会从最左上角的格子 (0,0) 出发。我们定义一条 有效路径 为从格子 (0,0) 出发，每一步都顺着数字对应方向走，最终在最右下角的格子 (m - 1, n - 1) 结束的路径。有效路径 不需要是最短路径 。

你可以花费 cost = 1 的代价修改一个格子中的数字，但每个格子中的数字 只能修改一次 。

请你返回让网格图至少有一条有效路径的最小代价。

__示例：__

示例 1：

![网格图 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/grid1.png)

输入：grid = [[1,1,1,1],[2,2,2,2],[1,1,1,1],[2,2,2,2]]
输出：3
解释：你将从点 (0, 0) 出发。
到达 (3, 3) 的路径为： (0, 0) --> (0, 1) --> (0, 2) --> (0, 3) 花费代价 cost = 1 使方向向下 --> (1, 3) --> (1, 2) --> (1, 1) --> (1, 0) 花费代价 cost = 1 使方向向下 --> (2, 0) --> (2, 1) --> (2, 2) --> (2, 3) 花费代价 cost = 1 使方向向下 --> (3, 3)
总花费为 cost = 3.

示例 2：

![网格图 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/grid2.png)

输入：grid = [[1,1,3],[3,2,2],[1,1,4]]
输出：0
解释：不修改任何数字你就可以从 (0, 0) 到达 (2, 2) 。

示例 3：

![网格图 3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/grid3.png)

输入：grid = [[1,2],[4,3]]
输出：1

示例 4：

输入：grid = [[2,2,2],[2,2,2]]
输出：3

示例 5：

输入：grid = [[4]]
输出：0

__提示：__

m == grid.length
n == grid\[i].length
1 <= m, n <= 100

__思路：__

优先队列 ➕ BFS
队列中记录走过的步数以及当前的坐标值
由于 m 和 n 都小于 100, 步数肯定小于 mn
可以使用一个整数来记录坐标以及步数, 高位记录步数, 中间记录 x 坐标, 低位记录 y 坐标, 即记录 step \* 1_000_000 + x \* 1_000 + y
访问过的坐标一定不需要再次访问
每次取出当前代价最小的坐标分别访问 4 个方向并记录
时间复杂度为 O(mnlg(mn)), 空间复杂度为 O(n), n 为 words 数组长度, m 为 words 中字符串的平均长度

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minCost(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> d{{0, 1, 1}, {0, -1, 2}, {1, 0, 3}, {-1, 0, 4}};
        unordered_map<int, int> visited;
        priority_queue<int, vector<int>, greater<int>> pq;
        pq.push(0);
        while (!pq.empty()) 
        {
            int top = pq.top(), step = top / (int)1e6, key = top % (int)1e6, x = key / (int)1e3, y = key % (int)1e3;
            pq.pop();
            if (visited.count(key)) continue;
            visited[key] = step;
            for (int i = 0; i < 4; i++) if (-1 < x + d[i][0] and x + d[i][0] < m and -1 < y + d[i][1] and y + d[i][1] < n) pq.push((step + (grid[x][y] != d[i][2])) * (int)1e6 + (x + d[i][0]) * (int)1e3 + y + d[i][1]);
        }
        return visited[(m - 1) * (int)1e3 + (n - 1)];
    }
};
```

__Java__:

```Java
class Solution {
    public int minCost(int[][] grid) {
        int m = grid.length, n = grid[0].length, d[][] = {{0, 1, 1}, {0, -1, 2}, {1, 0, 3}, {-1, 0, 4}};
        Map<Integer, Integer> visited = new HashMap<>();
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        pq.offer(0);
        while (!pq.isEmpty()) {
            int top = pq.poll(), step = top / 1_000_000, key = top % 1_000_000, x = key / 1_000, y = key % 1_000;
            if (visited.containsKey(key)) continue;
            visited.put(key, step);
            for (int i = 0; i < 4; i++) if (-1 < x + d[i][0] && x + d[i][0] < m && -1 < y + d[i][1] && y + d[i][1] < n) pq.offer((step + (grid[x][y] == d[i][2] ? 0 : 1)) * 1_000_000 + (x + d[i][0]) * 1_000 + y + d[i][1]);
        }
        return visited.get((m - 1) * 1_000 + (n - 1));
    }
}
```

__Python__:

```Python
class Solution:
    def minCost(self, grid: List[List[int]]) -> int:
        visited, m, n, queue, d = defaultdict(int), len(grid), len(grid[0]), [(0, 0, 0)], [(0, 1, 1), (0, -1, 2), (1, 0, 3), (-1, 0, 4)]
        while queue:
            step, x, y = heappop(queue)
            if (x, y) in visited:
                continue
            visited[(x, y)] = step
            for dx, dy, t in d:
                if -1 < x + dx < m and -1 < y + dy < n:
                    heappush(queue, [step + (grid[x][y] != t), x + dx, y + dy])
        return visited[(m - 1, n - 1)]
```
