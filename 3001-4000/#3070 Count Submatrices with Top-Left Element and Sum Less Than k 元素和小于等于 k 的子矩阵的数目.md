# 3070 Count Submatrices with Top-Left Element and Sum Less Than k 元素和小于等于 k 的子矩阵的数目

__Description:__

You are given a __0-indexed__ integer matrix `grid` and an integer `k`.

Return _the __number__ of submatrics that contain the top-left element of the_ `grid`, _and have a sum less than or equal to_ `k`.

__Example:__

Example 1:

![3070-1](https://assets.leetcode.com/uploads/2024/01/01/example1.png)

```text
Input: grid = [[7,6,3],[6,6,1]], k = 18
Output: 4
Explanation: There are only 4 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 18.
```

Example 2:

![3070-2](https://assets.leetcode.com/uploads/2024/01/01/example21.png)

```text
Input: grid = [[7,2,9],[1,5,0],[2,6,6]], k = 20
Output: 6
Explanation: There are only 6 submatrices, shown in the image above, that contain the top-left element of grid, and have a sum less than or equal to 20.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= n, m <= 1000`
- `0 <= grid[i][j] <= 1000`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数矩阵 `grid` 和一个整数 `k`。

返回包含 `grid` 左上角元素、元素和小于或等于 `k` 的 __子矩阵__ 的数目。

__示例:__

示例 1：

![3070-3](https://assets.leetcode.com/uploads/2024/01/01/example1.png)

```text
输入：grid = [[7,6,3],[6,6,1]], k = 18
输出：4
解释：如上图所示，只有 4 个子矩阵满足：包含 grid 的左上角元素，并且元素和小于或等于 18 。
```

示例 2：

![3070-4](https://assets.leetcode.com/uploads/2024/01/01/example21.png)

```text
输入：grid = [[7,2,9],[1,5,0],[2,6,6]], k = 20
输出：6
解释：如上图所示，只有 6 个子矩阵满足：包含 grid 的左上角元素，并且元素和小于或等于 20 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= n, m <= 1000`
- `0 <= grid[i][j] <= 1000`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
前缀和
求前缀和中有多少个不大于 k 的元素即可
时间复杂度为 O(MN), 空间复杂度为 O(1), 可在 grid 原地修改
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSubmatrices(vector<vector<int>>& grid, int k) 
    {
        int m = grid.size(), n = grid.front().size(), result = 0;
        vector<vector<int>> pre(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if ((pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j]) <= k) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countSubmatrices(int[][] grid, int k) {
        int m = grid.length, n = grid[0].length, result = 0, pre[][] = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) if ((pre[i + 1][j + 1] = pre[i + 1][j] + pre[i][j + 1] - pre[i][j] + grid[i][j]) <= k) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubmatrices(self, grid: List[List[int]], k: int) -> int:
        result = sum(num <= k for num in accumulate(grid[0]))
        for row in grid[1:]:
            s = 0
            for i, num in enumerate(row):
                grid[0][i] += num
                s += grid[0][i]
                if s > k:
                    break
                result += 1
        return result
```
