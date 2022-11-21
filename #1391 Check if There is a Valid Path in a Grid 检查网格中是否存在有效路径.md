# 1391 Check if There is a Valid Path in a Grid 检查网格中是否存在有效路径

__Description:__

You are given an m x n grid. Each cell of grid represents a street. The street of grid\[i][j] can be:

1 which means a street connecting the left cell and the right cell.
2 which means a street connecting the upper cell and the lower cell.
3 which means a street connecting the left cell and the lower cell.
4 which means a street connecting the right cell and the lower cell.
5 which means a street connecting the left cell and the upper cell.
6 which means a street connecting the right cell and the upper cell.

![1391-1](https://assets.leetcode.com/uploads/2020/03/05/main.png)

You will initially start at the street of the upper-left cell (0, 0). A valid path in the grid is a path that starts from the upper left cell (0, 0) and ends at the bottom-right cell (m - 1, n - 1). The path should only follow the streets.

Notice that you are not allowed to change any street.

Return true if there is a valid path in the grid or false otherwise.

__Example:__

Example 1:

![1391-2](https://assets.leetcode.com/uploads/2020/03/05/e1.png)

Input: grid = [[2,4,3],[6,5,2]]
Output: true
Explanation: As shown you can start at cell (0, 0) and visit all the cells of the grid to reach (m - 1, n - 1).

Example 2:

![1391-3](https://assets.leetcode.com/uploads/2020/03/05/e2.png)

Input: grid = [[1,2,1],[1,2,1]]
Output: false
Explanation: As shown you the street at cell (0, 0) is not connected with any street of any other cell and you will get stuck at cell (0, 0)

Example 3:

Input: grid = [[1,1,2]]
Output: false
Explanation: You will get stuck at cell (0, 1) and you cannot reach cell (0, 2).

__Constraints:__

m == grid.length
n == grid[i].length
1 <= m, n <= 300
1 <= grid\[i][j] <= 6

__题目描述:__

给你一个 m x n 的网格 grid。网格里的每个单元都代表一条街道。grid\[i][j] 的街道可以是：

1 表示连接左单元格和右单元格的街道。
2 表示连接上单元格和下单元格的街道。
3 表示连接左单元格和下单元格的街道。
4 表示连接右单元格和下单元格的街道。
5 表示连接左单元格和上单元格的街道。
6 表示连接右单元格和上单元格的街道。

![1391-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/main.png)

你最开始从左上角的单元格 (0,0) 开始出发，网格中的「有效路径」是指从左上方的单元格 (0,0) 开始、一直到右下方的 (m-1,n-1) 结束的路径。该路径必须只沿着街道走。

__注意：__
你 不能 变更街道。

如果网格中存在有效的路径，则返回 true，否则返回 false 。

__示例:__

示例 1：

![1391-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/e1.png)

输入：grid = [[2,4,3],[6,5,2]]
输出：true
解释：如图所示，你可以从 (0, 0) 开始，访问网格中的所有单元格并到达 (m - 1, n - 1) 。

示例 2：

![1391-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/e2.png)

输入：grid = [[1,2,1],[1,2,1]]
输出：false
解释：如图所示，单元格 (0, 0) 上的街道没有与任何其他单元格上的街道相连，你只会停在 (0, 0) 处。

示例 3：

输入：grid = [[1,1,2]]
输出：false
解释：你会停在 (0, 1)，而且无法到达 (0, 2) 。

示例 4：

输入：grid = [[1,1,1,1,1,1,3]]
输出：true

示例 5：

输入：grid = [[2],[2],[2],[2],[2],[2],[6]]
输出：true

__提示：__

m == grid.length
n == grid[i].length
1 <= m, n <= 300
1 <= grid\[i][j] <= 6

__思路:__

```text
1. 并查集
可以将能够通行的街道通过合并的方式连接在一起
比如类型 1 街道可以将当前街道和左边以及右边的街道连接在一起
预先处理所有的街道
检查起点和终点是否在一个并查集里即可
时间复杂度为 O(MN), 空间复杂度为 O(MN)
2. BFS
将上下左右分别记为 0123
通过 BFS 的方式从终点反向查询能够到达的点
直到遍历完所有的路径
检查是否能到达起点
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool hasValidPath(vector<vector<int>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        vector<vector<int>> direction{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, conn{{2, 3}, {0, 1},{1, 2}, {1, 3}, {0, 2}, {0, 3}};
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        visited.back().back() = true;
        queue<pair<int, int>> q;
        q.push(make_pair(m - 1, n - 1));
        while(!q.empty()) 
        {
            auto cur = q.front();
            q.pop();
            for(int neighbor : conn[grid[cur.first][cur.second] - 1]) 
            {
                int x = cur.first + direction[neighbor][0], y = cur.second + direction[neighbor][1], target = neighbor < 2 ? 1 - neighbor : 5 - neighbor;
                if (x > -1 and x < m and y > -1 and y < n and !visited[x][y] and (conn[grid[x][y] - 1][0] == target or conn[grid[x][y] - 1][1] == target)) 
                {
                    visited[x][y] = true;
                    q.push(make_pair(x, y));
                }
            }
        }
        return visited.front().front();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean hasValidPath(int[][] grid) {
        int m = grid.length, n = grid[0].length, direction[][] = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}, conn[][] = new int[][]{{2, 3}, {0, 1},{1, 2}, {1, 3}, {0, 2}, {0, 3}};
        boolean[][] visited = new boolean[m][n];
        visited[m - 1][n - 1] = true;
        Queue<int[]> queue = new LinkedList();
        queue.offer(new int[]{m - 1, n - 1});
        while(!queue.isEmpty()) {
            int[] cur = queue.poll();
            for(int neighbor : conn[grid[cur[0]][cur[1]] - 1]) {
                int x = cur[0] + direction[neighbor][0], y = cur[1] + direction[neighbor][1], target = neighbor < 2 ? 1 - neighbor : 5 - neighbor;
                if (x > -1 && x < m && y > -1 && y < n && !visited[x][y] && (conn[grid[x][y] - 1][0] == target || conn[grid[x][y] - 1][1] == target)) {
                    visited[x][y] = true;
                    queue.offer(new int[]{x, y});
                }
            }
        }
        return visited[0][0];
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

    def hasValidPath(self, grid: List[List[int]]) -> bool:
        uf = UF((m := len(grid)) * (n := len(grid[0])))
        
        def get_id(x: int, y: int) -> int:
            return x * n + y
        
        def left(x: int, y: int) -> NoReturn:
            if y - 1 >= 0 and grid[x][y - 1] in [1, 4, 6]:
                uf.union(get_id(x, y), get_id(x, y - 1))
        
        def right(x: int, y: int) -> NoReturn:
            if y + 1 < n and grid[x][y + 1] in [1, 3, 5]:
                uf.union(get_id(x, y), get_id(x, y + 1))
        
        def up(x: int, y: int) -> NoReturn:
            if x - 1 >= 0 and grid[x - 1][y] in [2, 3, 4]:
                uf.union(get_id(x, y), get_id(x - 1, y))
        
        def down(x: int, y: int) -> NoReturn:
            if x + 1 < m and grid[x + 1][y] in [2, 5, 6]:
                uf.union(get_id(x, y), get_id(x + 1, y))

        def helper(x: int, y: int) -> NoReturn:
            if grid[x][y] == 1:
                left(x, y), right(x, y)
            elif grid[x][y] == 2:
                up(x, y), down(x, y)
            elif grid[x][y] == 3:
                left(x, y), down(x, y)
            elif grid[x][y] == 4:
                right(x, y), down(x, y)
            elif grid[x][y] == 5:
                left(x, y), up(x, y)
            else:
                right(x, y), up(x, y)
        
        for i in range(m):
            for j in range(n):
                helper(i, j)
        
        return uf.find(get_id(0, 0)) == uf.find(get_id(m - 1, n - 1))
```
