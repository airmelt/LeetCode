# 3122 Minimum Number of Operations to Satisfy Conditions 使矩阵满足条件的最少操作次数

__Description:__

You are given a 2D matrix `grid` of size `m x n`. In one __operation__, you can change the value of __any__ cell to __any__ non-negative number. You need to perform some __operations__ such that each cell `grid[i][j]` is:

- Equal to the cell below it, i.e. `grid[i][j] == grid[i + 1][j]` (if it exists).
- Different from the cell to its right, i.e. `grid[i][j] != grid[i][j + 1]` (if it exists).

Return the minimum number of operations needed.

__Example:__

Example 1:

```text
Input: grid = [[1,0,2],[1,0,2]]
```

Output: 0

Explanation:

![3122-1](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)

All the cells in the matrix already satisfy the properties.

Example 2:

```text
Input: grid = [[1,1,1],[0,0,0]]
```

Output: 3

Explanation:

![3122-2](https://assets.leetcode.com/uploads/2024/03/27/example21.png)

The matrix becomes `[[1,0,1],[1,0,1]]` which satisfies the properties, by doing these 3 operations:

- Change `grid[1][0]` to 1.
- Change `grid[0][1]` to 0.
- Change `grid[1][2]` to 1.

Example 3:

```text
Input: grid = [[1],[2],[3]]
```

Output: 2

Explanation:

![3122-3](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

There is a single column. We can change the value to 1 in each cell using 2 operations.

__Constraints:__

- `1 <= n, m <= 1000`
- `0 <= grid[i][j] <= 9`

__题目描述:__

给你一个大小为 `m x n` 的二维矩形 `grid` 。每次 __操作__ 中，你可以将 __任一__ 格子的值修改为 __任意__ 非负整数。完成所有操作后，你需要确保每个格子 `grid[i][j]` 的值满足:

- 如果下面相邻格子存在的话，它们的值相等，也就是 `grid[i][j] == grid[i + 1][j]`（如果存在）。
- 如果右边相邻格子存在的话，它们的值不相等，也就是 `grid[i][j] != grid[i][j + 1]`（如果存在）。

请你返回需要的 最少 操作数目。

__示例:__

示例 1：

```text
输入：grid = [[1,0,2],[1,0,2]]
```

输出：0

解释：

![3122-4](https://assets.leetcode.com/uploads/2024/04/15/examplechanged.png)

矩阵中所有格子已经满足要求。

示例 2：

```text
输入：grid = [[1,1,1],[0,0,0]]
```

输出：3

解释：

![3122-5](https://assets.leetcode.com/uploads/2024/03/27/example21.png)

将矩阵变成 `[[1,0,1],[1,0,1]]` ，它满足所有要求，需要 3 次操作:

- 将 `grid[1][0]` 变为 1 。
- 将 `grid[0][1]` 变为 0 。
- 将 `grid[1][2]` 变为 1 。

示例 3：

```text
输入：grid = [[1],[2],[3]]
```

输出：2

解释：

![3122-6](https://assets.leetcode.com/uploads/2024/03/31/changed.png)

这个矩阵只有一列，我们可以通过 2 次操作将所有格子里的值变为 1 。

__提示：__

- `1 <= n, m <= 1000`
- `0 <= grid[i][j] <= 9`

__思路:__

```text
模拟
统计每一列 0-9 出现的次数
记录上一列最多出现的次数 mx, 次多出现的次数 mx2, 以及选择的数字 cur
如果选择和上一列相同的数字那么使用次多的转移否则使用最多的转移
最后需要保留最多出现的次数 a
用总数 m * n 减去
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperations(vector<vector<int>>& grid) 
    {
        int a = 0, b = 0, pre = -1, m = grid.size(), n = grid.front().size();
        for (int j = 0, mx = 0, mx2 = 0, cur = -1; j < n; j++)
        {
            vector<int> col(10);
            for (const auto& row : grid) ++col[row[j]];
            for (int v = 0, nxt = 0; v < 10; v++)
            {
                if ((nxt = (v != pre ? a : b) + col[v]) > mx)
                {
                    mx2 = mx;
                    mx = nxt;
                    cur = v;
                } 
                else if (nxt > mx2) mx2 = nxt;
            }
            a = mx;
            b = mx2;
            pre = cur;
        }
        return m * n - a;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumOperations(int[][] grid) {
        int a = 0, b = 0, pre = -1, m = grid.length, n = grid[0].length;
        for (int j = 0, col[] = new int[10], mx = 0, mx2 = 0, cur = -1; j < n; j++, col = new int[10]) {
            for (var row : grid) ++col[row[j]];
            for (int v = 0, nxt = 0; v < 10; v++) {
                if ((nxt = (v != pre ? a : b) + col[v]) > mx) {
                    mx2 = mx;
                    mx = nxt;
                    cur = v;
                } else if (nxt > mx2) mx2 = nxt;
            }
            a = mx;
            b = mx2;
            pre = cur;
        }
        return m * n - a;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumOperations(self, grid: List[List[int]]) -> int:
        a, b, pre = 0, 0, -1
        for col in zip(*grid):
            mx, mx2, cur = a, 0, -1
            for v, c in Counter(col).items():
                if (nxt := (a if v != pre else b) + c) > mx:
                    mx, mx2, cur = nxt, mx, v
                elif nxt > mx2:
                    mx2 = nxt
            a, b, pre = mx, mx2, cur
        return len(grid) * len(grid[0]) - a
```
