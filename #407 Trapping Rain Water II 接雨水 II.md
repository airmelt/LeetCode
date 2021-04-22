# 407 Trapping Rain Water II 接雨水 II

__Description__:
Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

__Example:__

Given the following 3x6 height map:

```text
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]
```

Return 4.

![elevation map](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/rainwater_empty.png)

The above image represents the elevation map [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]] before the rain.

![rain](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/rainwater_fill.png)

After the rain, water is trapped between the blocks. The total volume of water trapped is 4.

__Constraints:__

1 <= m, n <= 110
0 <= heightMap[i][j] <= 20000

__题目描述__:
给你一个 m x n 的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。

__示例 :__

给出如下 3x6 的高度图:

```text
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]
```

返回 4 。

![高度图](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/rainwater_empty.png)

如上图所示，这是下雨前的高度图[[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]] 的状态。

![雨](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/rainwater_fill.png)

下雨后，雨水将会被存储在这些方块中。总的接雨水量是4。

__提示：__

1 <= m, n <= 110
0 <= heightMap[i][j] <= 20000

__思路__:

使用优先队列记录围栏
先将高度图的外围一圈当作围栏
每次找到最低的围栏, 在周围找到没有遍历过的点, 如果高度比当前高度小, 说明可以存放雨水
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int trapRainWater(vector<vector<int>>& heightMap) 
    {
        int m = heightMap.size(), n = heightMap[0].size(), result = 0, dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};
        vector<vector<bool>> visited(m, vector<bool>(n));
        priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> pq;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if (!i or !j or i == m - 1 or j == n - 1)
        {
            pq.push({heightMap[i][j], {i, j}});
            visited[i][j] = true;
        }
        while (pq.size())
        {
            auto t = pq.top();
            pq.pop();
            for (int k = 0; k < 4; k++)
            {
                int x = t.second.first + dx[k], y = t.second.second + dy[k], h = t.first;
                if (x > -1 and x < m and y > -1 and y < n)
                {
                    if (visited[x][y] == false)
                    {
                        visited[x][y] = true;
                        if (h > heightMap[x][y]) result += h - heightMap[x][y];
                        pq.push({max(heightMap[x][y], h), {x, y}});
                    }
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
    public int trapRainWater(int[][] heightMap) {
        int m = heightMap.length, n = heightMap[0].length, result = 0, dx[] = new int[]{-1, 0, 1, 0}, dy[] = new int[]{0, 1, 0, -1};
        boolean visited[][] = new boolean[m][n];
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        for (int i = 0; i < m; i++) {
            visited[i][0] = visited[i][n - 1] = true;
            pq.add(new int[]{heightMap[i][0], i, 0});
            pq.add(new int[]{heightMap[i][n - 1], i, n - 1});
        }
        for (int j = 0; j < n; j++) {
            visited[0][j] = visited[m - 1][j] = true;
            pq.add(new int[]{heightMap[0][j], 0, j});
            pq.add(new int[]{heightMap[m - 1][j], m - 1, j});
        }
        while (!pq.isEmpty()) {
            int t[] = pq.poll();
            for (int k = 0; k < 4; k++) {
                int x = t[1] + dx[k], y = t[2] + dy[k], h = t[0];
                if (x > -1 && x < m && y > -1 && y < n) {
                    if (visited[x][y] == false) {
                        visited[x][y] = true;
                        if (h > heightMap[x][y]) result += h - heightMap[x][y];
                        pq.add(new int[]{Math.max(heightMap[x][y], h), x, y});
                    }
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
    def trapRainWater(self, heightMap: List[List[int]]) -> int:
        m, n, visited, result, pq, dx, dy = len(heightMap), len(heightMap[0]), [[False] * len(heightMap[0]) for _ in range(len(heightMap))], 0, [], [1, 0, -1, 0], [0, 1, 0, -1]
        for i in range(m):
            for j in range(n):
                if not i or not j or i == m - 1 or j == n - 1:
                    heapq.heappush(pq, (heightMap[i][j], i, j))
                    visited[i][j] = True
        while pq:
            cur = heapq.heappop(pq)
            for k in range(4):
                x, y, h = cur[1] + dx[k], cur[2] + dy[k], cur[0]
                if -1 < x < m and -1 < y < n and not visited[x][y]:
                    visited[x][y] = True
                    if h > heightMap[x][y]:
                        result += h - heightMap[x][y]
                    heapq.heappush(pq, (max(heightMap[x][y], h), x, y))
        return result
```
