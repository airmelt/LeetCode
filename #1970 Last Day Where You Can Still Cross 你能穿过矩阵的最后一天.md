# 1970 Last Day Where You Can Still Cross 你能穿过矩阵的最后一天

__Description:__

There is a __1-based__ binary matrix where `0` represents land and `1` represents water. You are given integers `row` and `col` representing the number of rows and columns in the matrix, respectively.

Initially on day `0`, the __entire__ matrix is __land__. However, each day a new cell becomes flooded with __water__. You are given a __1-based__ 2D array `cells`, where `cells[i] = [ri, ci]` represents that on the `i ^ th` day, the cell on the `ri ^ th` row and `ci ^ th` column (__1-based__ coordinates) will be covered with __water__ (i.e., changed to `1`).

You want to find the last day that it is possible to walk from the top to the bottom by only walking on land cells. You can start from any cell in the top row and end at any cell in the bottom row. You can only travel in the four cardinal directions (left, right, up, and down).

Return the last day where it is possible to walk from the top to the bottom by only walking on land cells.

__Example:__

Example 1:

![1970-1](https://assets.leetcode.com/uploads/2021/07/27/1.png)

```text
Input: row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]
Output: 2
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 2.
```

Example 2:

![1970-2](https://assets.leetcode.com/uploads/2021/07/27/2.png)

```text
Input: row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]
Output: 1
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 1.
```

Example 3:

![1970-3](https://assets.leetcode.com/uploads/2021/07/27/3.png)

```text
Input: row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]
Output: 3
Explanation: The above image depicts how the matrix changes each day starting from day 0.
The last day where it is possible to cross from top to bottom is on day 3.
```

__Constraints:__

- `2 <= row, col <= 2 * 10 ^ 4`
- `4 <= row * col <= 2 * 10 ^ 4`
- `cells.length == row * col`
- `1 <= ri <= row`
- `1 <= ci <= col`
- All the values of `cells` are __unique__.

__题目描述:__

给你一个下标从 __1__ 开始的二进制矩阵，其中 `0` 表示陆地， `1` 表示水域。同时给你 `row` 和 `col` 分别表示矩阵中行和列的数目。

一开始在第 `0` 天，__整个__ 矩阵都是 __陆地__ 。但每一天都会有一块新陆地被 __水__ 淹没变成水域。给你一个下标从 __1__ 开始的二维数组 `cells` ，其中 `cells[i] = [ri, ci]` 表示在第 `i` 天，第 `ri` 行 `ci` 列（下标都是从 __1__ 开始）的陆地会变成 __水域__ （也就是 `0` 变成 `1` ）。

你想知道从矩阵最 上面 一行走到最 下面 一行，且只经过陆地格子的 最后一天 是哪一天。你可以从最上面一行的 任意 格子出发，到达最下面一行的 任意 格子。你只能沿着 四个 基本方向移动（也就是上下左右）。

请返回只经过陆地格子能从最 上面 一行走到最 下面 一行的 最后一天 。

__示例:__

示例 1：

![1970-4](https://assets.leetcode.com/uploads/2021/07/27/1.png)

```text
输入：row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]
输出：2
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 2 天。
```

示例 2：

![1970-5](https://assets.leetcode.com/uploads/2021/07/27/2.png)

```text
输入：row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]
输出：1
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 1 天。
```

示例 3：

![1970-6](https://assets.leetcode.com/uploads/2021/07/27/3.png)

```text
输入：row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]
输出：3
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 3 天。
```

__提示：__

- `2 <= row, col <= 2 * 10 ^ 4`
- `4 <= row * col <= 2 * 10 ^ 4`
- `cells.length == row * col`
- `1 <= ri <= row`
- `1 <= ci <= col`
- `cells` 中的所有格子坐标都是 __唯一__ 的。

__思路:__

```text
并查集
正难则反
从后往前遍历 cells 数组找到第一天使得第一行和最后一行连通的天数
为了使第一行和最后一行连通, 我们可以将第一行和最后一行分别与虚拟节点相连
如果这两个虚拟结点连通, 则说明第一行和最后一行连通
对于 cells 的每一天, 我们将其对应的位置标记为 true, 然后将其上下左右已经访问的位置标记为 true, 并将其与上下左右位置连通
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class UF 
{
public:
    UF(int _n): n(_n), cnt(_n), parent(_n), weight(_n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x) 
    {
        return parent[x] == x ? x : parent[x] = find(parent[x]);
    }
    
    bool connet(int x, int y)
    {
        if ((x = find(x)) == (y = find(y))) return false;
        if (weight[x] < weight[y]) swap(x, y);
        parent[y] = x;
        weight[x] += weight[y];
        --cnt;
        return true;
    }
    
    bool connected(int x, int y) 
    {
        return find(x) == find(y);
    }
private:
    vector<int> parent;
    vector<int> weight;
    int n;
    int cnt;
};

class Solution 
{
public:
    int latestDayToCross(int row, int col, vector<vector<int>>& cells) 
    {
        int n = row * col, result = 0, x = 0, y = 0;
        auto uf = UF(n + 2);
        vector<vector<bool>> visited(row, vector<bool>(col));
        for (int i = n - 1; i > -1; i--) 
        {
            visited[x = cells[i][0] - 1][y = cells[i][1] - 1] = true;
            int cur = x * col + y;
            if (x > 0 and visited[x - 1][y]) uf.connet(cur, cur - col);
            if (x < row - 1 and visited[x + 1][y]) uf.connet(cur, cur + col);
            if (y > 0 and visited[x][y - 1]) uf.connet(cur, cur - 1);
            if (y < col - 1 and visited[x][y + 1]) uf.connet(cur, cur + 1);
            if (!x) uf.connet(cur, n);
            if (x == row - 1) uf.connet(cur, n + 1);
            if (uf.connected(n, n + 1)) return i;
        }
        return result;
    }
};
```

__Java__:

```Java
class UF 
{
    private int[] parent;
    private int[] weight;
    private int n;
    private int cnt;

    UF(int n) {
        this.n = n;
        parent = new int[n];
        for (int i = 1; i < n; i++) parent[i] = i;
        weight = new int[n];
        Arrays.fill(weight, 1);
    }
    
    int find(int x) {
        return (parent[x] == x) ? x : (parent[x] = find(parent[x]));
    }
    
    boolean connet(int x, int y) {
        if ((x = find(x)) == (y = find(y))) return false;
        if (weight[x] < weight[y]) {
            x ^= y;
            y ^= x;
            x ^= y;
        }
        parent[y] = x;
        weight[x] += weight[y];
        --cnt;
        return true;
    }
    
    boolean connected(int x, int y) {
        return find(x) == find(y);
    }
}

class Solution {
    public int latestDayToCross(int row, int col, int[][] cells) {
        int n = row * col, result = 0, x = 0, y = 0;
        UF uf = new UF(n + 2);
        boolean[][] visited = new boolean[row][col];
        for (int i = n - 1; i > -1; i--) {
            visited[x = cells[i][0] - 1][y = cells[i][1] - 1] = true;
            int cur = x * col + y;
            if (x > 0 && visited[x - 1][y]) uf.connet(cur, cur - col);
            if (x < row - 1 && visited[x + 1][y]) uf.connet(cur, cur + col);
            if (y > 0 && visited[x][y - 1]) uf.connet(cur, cur - 1);
            if (y < col - 1 && visited[x][y + 1]) uf.connet(cur, cur + 1);
            if (x == 0) uf.connet(cur, n);
            if (x == row - 1) uf.connet(cur, n + 1);
            if (uf.connected(n, n + 1)) return i;
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
    def latestDayToCross(self, row: int, col: int, cells: List[List[int]]) -> int:
        uf, result, visited = UF((n := row * col) + 2), 0, [[False] * col for _ in range(row)]
        for i in range(n - 1, -1, -1):
            visited[x][y], cur = True, (x := cells[i][0] - 1) * col + (y := cells[i][1] - 1)
            if x > 0 and visited[x - 1][y]:
                uf.union(cur, cur - col)
            if x < row - 1 and visited[x + 1][y]:
                uf.union(cur, cur + col)
            if y > 0 and visited[x][y - 1]:
                uf.union(cur, cur - 1)
            if y < col - 1 and visited[x][y + 1]:
                uf.union(cur, cur + 1)
            if not x:
                uf.union(cur, n)
            if x == row - 1:
                uf.union(cur, n + 1)
            if uf.connected(n, n + 1):
                return i
```
