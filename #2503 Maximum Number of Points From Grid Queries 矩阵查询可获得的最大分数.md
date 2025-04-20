# 2503 Maximum Number of Points From Grid Queries 矩阵查询可获得的最大分数

__Description:__

You are given an `m x n` integer matrix `grid` and an array `queries` of size `k`.

Find an array `answer` of size `k` such that for each integer `queries[i]` you start in the __top left__ cell of the matrix and repeat the following process:

- If `queries[i]` is __strictly__ greater than the value of the current cell that you are in, then you get one point if it is your first time visiting this cell, and you can move to any __adjacent__ cell in all `4` directions: up, down, left, and right.
- Otherwise, you do not get any points, and you end this process.

After the process, `answer[i]` is the __maximum__ number of points you can get. __Note__ that for each query you are allowed to visit the same cell __multiple__ times.

Return _the resulting array_ `answer`.

__Example:__

Example 1:

![2503-1](https://assets.leetcode.com/uploads/2025/03/15/image1.png)

```text
Input: grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
Output: [5,8,1]
Explanation: The diagrams above show which cells we visit to get points for each query.
```

Example 2:

![2503-2](https://assets.leetcode.com/uploads/2022/10/20/yetgriddrawio-2.png)

```text
Input: grid = [[5,2,1],[1,1,2]], queries = [3]
Output: [0]
Explanation: We can not get any points because the value of the top left cell is already greater than or equal to 3.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `k == queries.length`
- `1 <= k <= 10 ^ 4`
- `1 <= grid[i][j], queries[i] <= 10 ^ 6`

__题目描述:__

给你一个大小为 `m x n` 的整数矩阵 `grid` 和一个大小为 `k` 的数组 `queries` 。

找出一个大小为 `k` 的数组 `answer` ，且满足对于每个整数 `queries[i]` ，你从矩阵 __左上角__ 单元格开始，重复以下过程:

- 如果 `queries[i]` __严格__ 大于你当前所处位置单元格，如果该单元格是第一次访问，则获得 1 分，并且你可以移动到所有 `4` 个方向（上、下、左、右）上任一 __相邻__ 单元格。
- 否则，你不能获得任何分，并且结束这一过程。

在过程结束后， `answer[i]` 是你可以获得的最大分数。注意，对于每个查询，你可以访问同一个单元格 __多次__ 。

返回结果数组 `answer` 。

__示例:__

示例 1：

![2503-3](https://assets.leetcode.com/uploads/2025/03/15/image1.png)

```text
输入：grid = [[1,2,3],[2,5,7],[3,5,1]], queries = [5,6,2]
输出：[5,8,1]
解释：上图展示了每个查询中访问并获得分数的单元格。
```

示例 2：

![2503-4](https://assets.leetcode.com/uploads/2022/10/20/yetgriddrawio-2.png)

```text
输入：grid = [[5,2,1],[1,1,2]], queries = [3]
输出：[0]
解释：无法获得分数，因为左上角单元格的值大于等于 3 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 1000`
- `4 <= m * n <= 10 ^ 5`
- `k == queries.length`
- `1 <= k <= 10 ^ 4`
- `1 <= grid[i][j], queries[i] <= 10 ^ 6`

__思路:__

```text
由于查询一定是单调递增的, 并且矩阵是不变的
所以可以采用离线查询
先将查询按照值排序并记录下标
然后从小到大遍历查询
1. 并查集
将二维坐标转化为一维坐标
先将左上角的值加入并查集
如果当前值小于查询值, 则将当前值加入并查集
遍历当前值的四个方向
将所有小于查询值的值加入并查集
统计当前值的并查集的大小
返回左上角对应的并查集大小
时间复杂度为 O(MNlogMN + KlogK), 空间复杂度为 O(MN + K), k 为查询数
2. 优先队列
用小根堆记录当前值
将小于当前查询值的值弹出
弹出的个数即为当前查询值的结果
可以使用 visited 数组记录当前值是否已经加入过
也可以直接使用 grid 数组记录当前值是否已经加入过
时间复杂度为 O(MNlogMN + KlogK), 空间复杂度为 O(MN + K), k 为查询数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> maxPoints(vector<vector<int>> &grid, vector<int> &queries) 
    {
        int m = grid.size(), n = grid.front().size(), cur = 0, nx = 0, ny = 0, k = queries.size(), idx[k];
        iota(idx, idx + k, 0);
        sort(idx, idx + k, [&](int i, int j) { return queries[i] < queries[j]; });
        vector<int> result(k);
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        pq.emplace(grid.front().front(), 0, 0);
        grid.front().front() = 0;
        for (int i : idx) 
        {
            int q = queries[i];
            while (!pq.empty() and get<0>(pq.top()) < q) 
            {
                ++cur;
                auto [_, x, y] = pq.top();
                pq.pop();
                for (auto &d : dir) 
                {
                    if (-1 < (nx = x + d[0]) and nx < m and -1 < (ny = y + d[1]) and ny < n and grid[nx][ny]) 
                    {
                        pq.emplace(grid[nx][ny], nx, ny);
                        grid[nx][ny] = 0;
                    }
                }
            }
            result[i] = cur;
        }
        return result;
    }
private:
    const int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
};
```

__Java__:

```Java
class Solution {
    private static final int[][] dir = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    public int[] maxPoints(int[][] grid, int[] queries) {
        int k = queries.length, result[] = new int[k], m = grid.length, n = grid[0].length, cur = 0, nx = 0, ny = 0;
        var idx = IntStream.range(0, k).boxed().sorted((i, j) -> queries[i] - queries[j]).toArray(Integer[]::new);
        var pq = new PriorityQueue<int[]>((a, b) -> a[0] - b[0]);
        pq.add(new int[]{grid[0][0], 0, 0});
        grid[0][0] = 0;
        for (var i : idx) {
            var q = queries[i];
            while (!pq.isEmpty() && pq.peek()[0] < q) {
                ++cur;
                var p = pq.poll();
                for (var d : dir) {
                    if (-1 < (nx = p[1] + d[0]) && nx < m && -1 < (ny = p[2] + d[1]) && ny < n && grid[nx][ny] > 0) {
                        pq.add(new int[]{grid[nx][ny], nx, ny});
                        grid[nx][ny] = 0;
                    }
                }
            }
            result[i] = cur;
        }
        return result;
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
    def maxPoints(self, grid: List[List[int]], queries: List[int]) -> List[int]:
        uf, d, a, j, result = UF(total := (m := len(grid)) * (n := len(grid[0]))), [(1, 0), (-1, 0), (0, 1), (0, -1)], sorted((val, i, j) for i, row in enumerate(grid) for j, val in enumerate(row)), 0,[0] * len(queries)

        for i, q in sorted(enumerate(queries), key=lambda x: x[1]):
            while j < total and a[j][0] < q:
                _, x, y = a[j]
                for dx, dy in d:
                    if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and grid[nx][ny] < q:
                        uf.union(nx * n + ny, x * n + y)
                j += 1
            if grid[0][0] < q:
                result[i] = uf.weight[uf.find(0)]
        return result
```
