# 1631 Path With Minimum Effort 最小体力消耗路径

__Description:__

You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., __0-indexed__). You can move __up__, __down__, __left__, or __right__, and you wish to find a route that requires the minimum __effort__.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

__Example:__

Example 1:

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-1](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

```text
Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
Output: 2
Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
```

Example 2:

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-2](https://assets.leetcode.com/uploads/2020/10/04/ex2.png)

```text
Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
Output: 1
Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].
```

Example 3:

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-3](https://assets.leetcode.com/uploads/2020/10/04/ex3.png)

```text
Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
Output: 0
Explanation: This route does not require any effort.
```

__Constraints:__

- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10 ^ 6`

__题目描述:__

你准备参加一场远足活动。给你一个二维 `rows x columns` 的地图 `heights` ，其中 `heights[row][col]` 表示格子 `(row, col)` 的高度。一开始你在最左上角的格子 `(0, 0)` ，且你希望去最右下角的格子 `(rows-1, columns-1)` （注意下标从 __0__ 开始编号）。你每次可以往 __上__，__下__，__左__，__右__ 四个方向之一移动，你想要找到耗费 __体力__ 最小的一条路径。

一条路径耗费的 体力值 是路径上相邻格子之间 高度差绝对值 的 最大值 决定的。

请你返回从左上角走到右下角的最小 体力消耗值 。

__示例:__

示例 1：

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex1.png)

```text
输入：heights = [[1,2,2],[3,8,2],[5,3,5]]
输出：2
解释：路径 [1,3,5,3,5] 连续格子的差值绝对值最大为 2 。
这条路径比路径 [1,2,2,2,5] 更优，因为另一条路径差值最大值为 3 。
```

示例 2：

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex2.png)

```text
输入：heights = [[1,2,3],[3,8,4],[5,3,5]]
输出：1
解释：路径 [1,2,3,4,5] 的相邻格子差值绝对值最大为 1 ，比路径 [1,3,5,3,5] 更优。
```

示例 3：

![最小体力消耗路径 - 最小体力消耗路径 - 力扣（LeetCode）-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex3.png)

```text
输入：heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
输出：0
解释：上图所示路径不需要消耗任何体力。
```

__提示：__

- `rows == heights.length`
- `columns == heights[i].length`
- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10 ^ 6`

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumEffortPath(vector<vector<int>>& heights) 
    {
        int n = heights.size(), m = heights.front().size();
        vector<vector<int>> f(n, vector<int>(m, INT_MAX));
        int dx[4] = {0, 1, 0, -1}, dy[4] = {1, 0, -1, 0};
        f.front().front() = 0;
        queue<int> q;
        q.push(0);
        while(!q.empty()) 
        {
            auto t = q.front();
            q.pop();
            int x = t / m, y = t % m;
            for (int d = 0; d < 4; d++) 
            {
                int a = x + dx[d], b = y + dy[d];
                if (a >= 0 and a < n and b >= 0 and b < m) 
                {
                    int cur = max(f[x][y], abs(heights[a][b] - heights[x][y]));
                    if (cur >= f[a][b]) continue;
                    f[a][b] = cur;
                    q.push(a * m + b);
                }
            }
        }
        return f.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumEffortPath(int[][] heights) {

    }
}
```

__Python__:

```Python
class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        n, m, visited, result, q, d = len(heights), len(heights[0]), set(), [[-1] * len(heights[0]) for _ in range(len(heights))], [(0, 0, 0)], (-1, 0, 1, 0, -1)
        while q:
            dis, r, c = heapq.heappop(q)
            if (r, c) not in visited:
                visited.add((r, c))
                result[r][c] = dis
                for i in range(4):
                    if -1 < r + d[i] < n and -1 < c + d[i + 1] < m and (r + d[i], c + d[i + 1]) not in visited:
                        heapq.heappush(q, (max(dis, abs(heights[r][c] - heights[r + d[i]][c + d[i + 1]])), r + d[i], c + d[i + 1]))
        return result[-1][-1]
```
