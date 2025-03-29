# 305 Number of Islands II 岛屿数量 II

__Description:__

You are given an empty 2D binary `grid` grid of size `m x n`. The grid represents a map where `0`'s represent water and `1`'s represent land. Initially, all the cells of `grid` are water cells (i.e., all the cells are `0`'s).

We may perform an add land operation which turns the water at position into a land. You are given an array `positions` where `positions[i] = [ri, ci]` is the position `(ri, ci)` at which we should operate the `ith` operation.

Return _an array of integers `answer` where `answer[i]` is the number of islands after turning the cell `(ri, ci)` into a land._

An __island__ is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

__Example:__

Example 1:

![302-1](https://assets.leetcode.com/uploads/2021/03/10/tmp-grid.jpg)

```text
Input: m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
Output: [1,1,2,3]
Explanation:
Initially, the 2d grid is filled with water.
```

- Operation #1: addLand(0, 0) turns the water at grid\[0][0] into a land. We have 1 island.
- Operation #2: addLand(0, 1) turns the water at grid\[0][1] into a land. We still have 1 island.
- Operation #3: addLand(1, 2) turns the water at grid\[1][2] into a land. We have 2 islands.
- Operation #4: addLand(2, 1) turns the water at grid\[2][1] into a land. We have 3 islands.

Example 2:

```text
Input: m = 1, n = 1, positions = [[0,0]]
Output: [1]
```

__Constraints:__

- `1 <= m, n, positions.length <= 10 ^ 4`
- `1 <= m * n <= 10 ^ 4`
- `positions[i].length == 2`
- `0 <= ri < m`
- `0 <= ci < n`

__Follow up:__

Could you solve it in time complexity `O(k log(mn))`, where `k == positions.length`?

__题目描述:__

给你一个大小为 `m x n` 的二维二进制网格 `grid` 。网格表示一个地图，其中，`0` 表示水，`1` 表示陆地。最初，`grid` 中的所有单元格都是水单元格（即，所有单元格都是 `0`）。

可以通过执行 `addLand` 操作，将某个位置的水转换成陆地。给你一个数组 `positions` ，其中 `positions[i] = [ri, ci]` 是要执行第 `i` 次操作的位置 `(ri, ci)` 。

返回一个整数数组 `answer` ，其中 `answer[i]` 是将单元格 `(ri, ci)` 转换为陆地后，地图中岛屿的数量。

__岛屿__ 的定义是被「水」包围的「陆地」，通过水平方向或者垂直方向上相邻的陆地连接而成。你可以假设地图网格的四边均被无边无际的「水」所包围。

__示例:__

示例 1：

![302-2](https://assets.leetcode.com/uploads/2021/03/10/tmp-grid.jpg)

```text
输入：m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]
输出：[1,1,2,3]
解释：
起初，二维网格 grid 被全部注入「水」。（0 代表「水」，1 代表「陆地」）
```

- 操作 #1：addLand(0, 0) 将 grid\[0][0] 的水变为陆地。此时存在 1 个岛屿。
- 操作 #2：addLand(0, 1) 将 grid\[0][1] 的水变为陆地。此时存在 1 个岛屿。
- 操作 #3：addLand(1, 2) 将 grid\[1][2] 的水变为陆地。此时存在 2 个岛屿。
- 操作 #4：addLand(2, 1) 将 grid\[2][1] 的水变为陆地。此时存在 3 个岛屿。

示例 2：

```text
输入：m = 1, n = 1, positions = [[0,0]]
输出：[1]
```

__提示：__

- `1 <= m, n, positions.length <= 10 ^ 4`
- `1 <= m * n <= 10 ^ 4`
- `positions[i].length == 2`
- `0 <= ri < m`
- `0 <= ci < n`

__Follow up:__

你可以设计一个时间复杂度 `O(k log(mn))` 的算法解决此问题吗？（其中 `k == positions.length`）

__思路:__

```text
并查集
将二维坐标转换为一维坐标
如果当前位置未被访问, 则岛屿数量加一
遍历四个方向, 如果相邻位置已被访问, 则连接两个位置
如果连接了两个位置, 则岛屿数量减一
时间复杂度为 O(MN + QlogMN), 空间复杂度为 O(MN), 其中 Q 为 positions 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) 
    {
        int total = m * n, nx = 0, ny = 0, nindex = 0, index = 0, directions[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        visited.resize(total);
        for (int i = 0; i < total; i++) parent.emplace_back(i);
        for (const auto& position : positions) {
            if (!visited[index = position.front() * n + position.back()]) 
            {
                ++count;
                visited[index] = 1;
                for (const auto& direction : directions) if (-1 < (nx = position.front() + direction[0]) and nx < m and -1 < (ny = position.back() + direction[1]) and ny < n and visited[nindex = nx * n + ny]) connect(nindex, index);
            }
            result.emplace_back(count);
        }
        return result;
    }
private:
    vector<int> parent, visited, result;
    int count = 0;

    void connect(int p, int q) 
    {
        if ((p = find(p)) == (q = find(q))) return;
        parent[p] = q;
        --count;
    }

    int find(int p) 
    {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
    }
};
```

__Java__:

```Java
class Solution {
    private int[] parent;
    private int count = 0;
    private boolean[] visited;
    private final static int[][] directions = {{-1, 0}, {0, -1}, {1, 0}, {0, 1}};

    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        int total = m * n, nx = 0, ny = 0, nindex = 0, index = 0;
        visited = new boolean[total];
        parent = new int[total];
        for (int i = 1; i < total; i++) parent[i] = i;
        List<Integer> result = new ArrayList<>();
        for (int[] position : positions) {
            if (!visited[index = position[0] * n + position[1]]) {
                ++count;
                visited[index] = true;
                for (int[] direction : directions) if (-1 < (nx = position[0] + direction[0]) && nx < m && -1 < (ny = position[1] + direction[1]) && ny < n && visited[nindex = nx * n + ny]) union(nindex, index);
            }
            result.add(count);
        }
        return result;
    }

    private void union(int p, int q) {
        if ((p = find(p)) == (q = find(q))) return;
        parent[p] = q;
        --count;
    }

    private int find(int p) {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
    }
}
```

__Python__:

```Python
class UF:
    def __init__(self, n: int) -> None:
        self.parent = [i for i in range(n)]
        self.weight = [1] * n
        self.count = 0

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
    def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
        uf, result, d, visited = UF(total := m * n), [], [(0, 1), (0, -1), (1, 0), (-1, 0)], set()
        for x, y in positions:
            if (x, y) not in visited:
                uf.count += 1
                visited.add((x, y))
                for dx, dy in d:
                    if -1 < (nx := x + dx) < m and -1 < (ny := y + dy) < n and (nx, ny) in visited:
                        uf.union(nx * n + ny, x * n + y)
            result.append(uf.count)
        return result
```
