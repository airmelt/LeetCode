# 1914 Cyclically Rotating a Grid 循环轮转矩阵

__Description:__

You are given an `m x n` integer matrix `grid`​​​, where `m` and `n` are both __even__ integers, and an integer `k`.

The matrix is composed of several layers, which is shown in the below image, where each color is its own layer:

![1914-1](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid.png)

A cyclic rotation of the matrix is done by cyclically rotating each layer in the matrix. To cyclically rotate a layer once, each element in the layer will take the place of the adjacent element in the counter-clockwise direction. An example rotation is shown below:

![1914-2](https://assets.leetcode.com/uploads/2021/06/22/explanation_grid.jpg)

Return _the matrix after applying_ `k` _cyclic rotations to it_.

__Example:__

Example 1:

![1914-3](https://assets.leetcode.com/uploads/2021/06/19/rod2.png)

```text
Input: grid = [[40,10],[30,20]], k = 1
Output: [[10,20],[40,30]]
Explanation: The figures above represent the grid at every state.
```

Example 2:

![1914-4](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid5.png)

![1914-5](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid6.png)

![1914-6](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid7.png)

```text
Input: grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2
Output: [[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]
Explanation: The figures above represent the grid at every state.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 50`
- Both `m` and `n` are __even__ integers.
- `1 <= grid[i][j] <= ^ 5000`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个大小为 `m x n` 的整数矩阵 `grid`​​​ ，其中 `m` 和 `n` 都是 __偶数__ ；另给你一个整数 `k` 。

矩阵由若干层组成，如下图所示，每种颜色代表一层：

![1914-7](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid.png)

__示例:__

矩阵的循环轮转是通过分别循环轮转矩阵中的每一层完成的。在对某一层进行一次循环旋转操作时，层中的每一个元素将会取代其 逆时针 方向的相邻元素。轮转示例如下：

![1914-8](https://assets.leetcode.com/uploads/2021/06/22/explanation_grid.jpg)

```text
返回执行 `k` 次循环轮转操作后的矩阵。
```

示例 1：

![1914-9](https://assets.leetcode.com/uploads/2021/06/19/rod2.png)

```text
输入：grid = [[40,10],[30,20]], k = 1
输出：[[10,20],[40,30]]
解释：上图展示了矩阵在执行循环轮转操作时每一步的状态。
```

示例 2：

![1914-10](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid5.png)

![1914-11](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid6.png)

![1914-12](https://assets.leetcode.com/uploads/2021/06/10/ringofgrid7.png)

```text
输入：grid = [[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]], k = 2
输出：[[3,4,8,12],[2,11,10,16],[1,7,6,15],[5,9,13,14]]
解释：上图展示了矩阵在执行循环轮转操作时每一步的状态。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 50`
- `m` 和 `n` 都是 __偶数__
- `1 <= grid[i][j] <= ^ 5000`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
模拟
层数为 min(m, n) / 2
从外向内遍历每一层, 将每一层的元素放入队列中
轮转每一层的队列
最后将队列中的元素放回矩阵中
时间复杂度为 O(MN), 空间复杂度为 O(1), 使用原地旋转可以将空间复杂度降为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> rotateGrid(vector<vector<int>>& grid, int k) 
    {
        int m = grid.size(), n = grid.front().size(), mid = min(m, n) >> 1;
        vector<queue<int>> list(mid);
        for (int i = 0; i < mid; i++) 
        {
            for (int j = i; j < n - i - 1; j++) list[i].push(grid[i][j]);
            for (int j = i; j < m - i - 1; j++) list[i].push(grid[j][n - i - 1]);
            for (int j = n - 1 - i; j > i; j--) list[i].push(grid[m - i - 1][j]);
            for (int j = m - 1 - i; j > i; j--) list[i].push(grid[j][i]);
        }
        for (int i = 0; i < mid; i++) for (int j = 0; j < k % list[i].size(); j++) list[i].push(list[i].front()), list[i].pop();
        for (int i = 0; i < mid; i++) 
        {
            for (int j = i; j < n - i - 1; j++) grid[i][j] = list[i].front(), list[i].pop();
            for (int j = i; j < m - i - 1; j++) grid[j][n - i - 1] = list[i].front(), list[i].pop();
            for (int j = n - 1 - i; j > i; j--) grid[m - i - 1][j] = list[i].front(), list[i].pop();
            for (int j = m - 1 - i; j > i; j--) grid[j][i] = list[i].front(), list[i].pop();
        }
        return grid;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] rotateGrid(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length, mid = Math.min(m, n) >> 1;
        List<Queue<Integer>> list = new ArrayList<>();
        for (int i = 0; i < mid; i++) list.add(new LinkedList<>());
        for (int i = 0; i < mid; i++) {
            for (int j = i; j < n - i - 1; j++) list.get(i).offer(grid[i][j]);
            for (int j = i; j < m - i - 1; j++) list.get(i).offer(grid[j][n - i - 1]);
            for (int j = n - 1 - i; j > i; j--) list.get(i).offer(grid[m - i - 1][j]);
            for (int j = m - 1 - i; j > i; j--) list.get(i).offer(grid[j][i]);
        }
        for (int i = 0; i < mid; i++) for (int j = 0; j < k % list.get(i).size(); j++) list.get(i).offer(list.get(i).poll());
        for (int i = 0; i < mid; i++) {
            for (int j = i; j < n - i - 1; j++) grid[i][j] = list.get(i).poll();
            for (int j = i; j < m - i - 1; j++) grid[j][n - i - 1] = list.get(i).poll();
            for (int j = n - 1 - i; j > i; j--) grid[m - i - 1][j] = list.get(i).poll();
            for (int j = m - 1 - i; j > i; j--) grid[j][i] = list.get(i).poll();
        }
        return grid;
    }
}
```

__Python__:

```Python
class Solution:
    def rotateGrid(self, grid: List[List[int]], k: int) -> List[List[int]]:
        mid = min(m := len(grid), n := len(grid[0])) >> 1
        a = [deque() for _ in range(mid)]
        for i in range(mid):
            for j in range(i, n - i - 1):
                a[i].append(grid[i][j])
            for j in range(i, m - i - 1):
                a[i].append(grid[j][n - i - 1])
            for j in range(n - 1 - i, i, -1):
                a[i].append(grid[m - i - 1][j])
            for j in range(m - 1 - i, i, -1):
                a[i].append(grid[j][i])
        for i in range(mid):
            for j in range(k % len(a[i])):
                a[i].append(a[i].popleft())
        for i in range(mid):
            for j in range(i, n - i - 1):
                grid[i][j] = a[i].popleft()
            for j in range(i, m - i - 1):
                grid[j][n - i - 1] = a[i].popleft()
            for j in range(n - 1 - i, i, -1):
                grid[m - i - 1][j] = a[i].popleft()
            for j in range(m - 1 - i, i, -1):
                grid[j][i] = a[i].popleft()
        return grid
```
