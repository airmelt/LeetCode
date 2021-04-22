# 994 Rotting Oranges 腐烂的橘子

__Description__:
In a given grid, each cell can have one of three values:

the value 0 representing an empty cell;
the value 1 representing a fresh orange;
the value 2 representing a rotten orange.
Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange.  If this is impossible, return -1 instead.

__Example:__

Example 1:

![Rotting Oranges](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/oranges.png)

Input: [[2,1,1],[1,1,0],[0,1,1]]
Output: 4

Example 2:

Input: [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation:  The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.

Example 3:

Input: [[0,2]]
Output: 0
Explanation:  Since there are already no fresh oranges at minute 0, the answer is just 0.

__Note:__

1 <= grid.length <= 10
1 <= grid[0].length <= 10
grid[i][j] is only 0, 1, or 2.

__题目描述__:
在给定的网格中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。

__示例 :__

示例 1：

![腐烂的橘子](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/oranges.png)

输入：[[2,1,1],[1,1,0],[0,1,1]]
输出：4

示例 2：

输入：[[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。

示例 3：

输入：[[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。

__提示：__

1 <= grid.length <= 10
1 <= grid[0].length <= 10
grid[i][j] 仅为 0、1 或 2

__思路__:

遍历橘子, 将腐烂的橘子加入队列, 并记录橘子的数量. 采用 BFS的思想遍历队列, 每次对 4个方向的橘子进行腐烂, 直到队列为空. 如果所有橘子都腐烂, 返回遍历的次数, 否则返回 -1
时间复杂度O(n ^ 2), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int orangesRotting(vector<vector<int>>& grid) 
    {
        queue<pair<int, int>> q;
        vector<pair<int, int>> directs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        int count = 0, result = -1, row = grid.size(), col = grid[0].size();
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++)
        {
            if (grid[i][j] == 2) q.push(make_pair(i, j));
            if (grid[i][j]) ++count;
        }
        if (q.size() == count) return 0;
        while (q.size())
        {
            int s = q.size();
            while (s--)
            {
                auto cur = q.front();
                q.pop();
                count--;
                for (auto direct : directs)
                {
                    int x = cur.first + direct.first, y = cur.second + direct.second;
                    if (x < 0 or y < 0 or x > row - 1 or y > col - 1 or grid[x][y] != 1) continue;
                    grid[x][y] = 2;
                    q.push(make_pair(x, y));
                }
            }
            ++result;
        }
        return count ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<Integer> q = new LinkedList<>();
        int dx[] = new int[]{1, 0, -1, 0}, dy[] = new int[]{0, 1, 0, -1}, row = grid.length, col = grid[0].length, count = 0, result = -1;
        for (int i = 0; i < row; i++) for (int j = 0; j < col; j++) {
            if (grid[i][j] == 2) q.offer((i + 1 << 4) + j);
            if (grid[i][j] > 0) count++;
        }
        if (q.size() == count) return 0;
        while (!q.isEmpty()) {
            int s = q.size();
            while (s-- > 0) {
                int cur = q.poll();
                count--;
                for (int k = 0; k < 4; k++) {
                    int x = dx[k] + (cur >> 4) - 1, y = dy[k] + (cur & 15);
                    if (x < 0 || y < 0 || x > row - 1 || y > col - 1 || grid[x][y] != 1) continue;
                    grid[x][y] = 2;
                    q.offer((x + 1 << 4) + y);
                }
            }
            result++;
        }
        return count == 0 ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        queue, row, col, count, result, directs = [], len(grid), len(grid[0]), 0, -1, [(1, 0), (0, 1), (-1, 0), (0, -1)]
        for i in range(row):
            for j in range(col):
                if grid[i][j]:
                    count += 1
                    if grid[i][j] == 2:
                        queue.append((i, j))
        if count == len(queue):
            return 0
        while queue:
            s = len(queue)
            while s:
                cur = queue.pop(0)
                count -= 1
                for direct in directs:
                    x, y = direct[0] + cur[0], direct[1] + cur[1]
                    if -1 < x < row and -1 < y < col and grid[x][y] == 1:
                        grid[x][y] = 2
                        queue.append((x, y))
                s -= 1
            result += 1
        return -1 if count else result
```
