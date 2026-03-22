# 764 Largest Plus Sign 最大加号标志

__Description__:
You are given an integer n. You have an n x n binary grid grid with all values initially 1's except for some indices given in the array mines. The ith element of the array mines is defined as mines[i] = [xi, yi] where grid[xi][yi] == 0.

Return the order of the largest axis-aligned plus sign of 1's contained in grid. If there is none, return 0.

An axis-aligned plus sign of 1's of order k has some center grid[r][c] == 1 along with four arms of length k - 1 going up, down, left, and right, and made of 1's. Note that there could be 0's or 1's beyond the arms of the plus sign, only the relevant area of the plus sign is checked for 1's.

__Example:__

Example 1:

![plus grid 1](https://assets.leetcode.com/uploads/2021/06/13/plus1-grid.jpg)

Input: n = 5, mines = [[4,2]]
Output: 2
Explanation: In the above grid, the largest plus sign can only be of order 2. One of them is shown.

Example 2:

![plus grid 2](https://assets.leetcode.com/uploads/2021/06/13/plus2-grid.jpg)

Input: n = 1, mines = [[0,0]]
Output: 0
Explanation: There is no plus sign, so return 0.

__Constraints:__

1 <= n <= 500
1 <= mines.length <= 5000
0 <= xi, yi < n
All the pairs (xi, yi) are unique.

__题目描述__:
在一个大小在 (0, 0) 到 (N-1, N-1) 的2D网格 grid 中，除了在 mines 中给出的单元为 0，其他每个单元都是 1。网格中包含 1 的最大的轴对齐加号标志是多少阶？返回加号标志的阶数。如果未找到加号标志，则返回 0。

一个 k" 阶由 1 组成的“轴对称”加号标志具有中心网格  grid[x][y] = 1 ，以及4个从中心向上、向下、向左、向右延伸，长度为 k-1，由 1 组成的臂。下面给出 k" 阶“轴对称”加号标志的示例。注意，只有加号标志的所有网格要求为 1，别的网格可能为 0 也可能为 1。

k 阶轴对称加号标志示例:

阶 1:

```text
000
010
000
```

阶 2:

```text
00000
00100
01110
00100
00000
```

阶 3:

```text
0000000
0001000
0001000
0111110
0001000
0001000
0000000
```

__示例 :__

示例 1：

输入: N = 5, mines = [[4, 2]]
输出: 2
解释:

```text
11111
11111
11111
11111
11011
```

在上面的网格中，最大加号标志的阶只能是2。一个标志已在图中标出。

示例 2：

输入: N = 2, mines = []
输出: 1
解释:

```text
11
11
```

没有 2 阶加号标志，有 1 阶加号标志。

示例 3：

输入: N = 1, mines = [[0, 0]]
输出: 0
解释:

```text
0
```

没有加号标志，返回 0 。

__提示:__

整数N 的范围： [1, 500].
mines 的最大长度为 5000.
mines[i] 是长度为2的由2个 [0, N-1] 中的数组成.
(另外,使用 C, C++, 或者 C# 编程将以稍小的时间限制进行​​判断.)

__思路__:

DFS
建立 n * n 的二维数组, 初始化所有元素为 n
对每一个元素, 分别向左, 右, 上, 下搜索并更新加号的臂的长度
同时用该长度对二维数组的值进行更新
最后找到二维数组中的最大值即为加号的长度
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int orderOfLargestPlusSign(int n, vector<vector<int>>& mines) 
    {
        vector<int> grid(n * n, n);
        for (const auto& mine : mines) grid[mine.front() * n + mine.back()] = 0;
        for (int i = 0; i < n; i++) 
        {
            int left = 0, right = 0, up = 0, down = 0;
            for (int j = 0, s = n - 1; j < n; j++, s--) 
            {
                left = grid[i * n + j] == 0 ? 0 : left + 1;
                grid[i * n + j] = min(grid[i * n + j], left);
                right = grid[i * n + s] == 0 ? 0 : right + 1;
                grid[i * n + s] = min(grid[i * n + s], right);
                up = grid[j * n + i] == 0 ? 0 : up + 1;
                grid[j * n + i] = min(grid[j * n + i], up);
                down = grid[s * n + i] == 0 ? 0 : down + 1;
                grid[s * n + i] = min(grid[s * n + i], down);
            }
        }
        return *max_element(grid.begin(), grid.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int orderOfLargestPlusSign(int n, int[][] mines) {
        int grid[][] = new int[n][n], result = 0;
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) grid[i][j] = n;
        for (int mine[] : mines) grid[mine[0]][mine[1]] = 0;
        for (int i = 0; i < n; i++) {
            int left = 0, right = 0, up = 0, down = 0;
            for (int j = 0, s = n - 1; j < n; j++, s--) {
                left = grid[i][j] == 0 ? 0 : left + 1;
                grid[i][j] = Math.min(grid[i][j], left);
                right = grid[i][s] == 0 ? 0 : right + 1;
                grid[i][s] = Math.min(grid[i][s], right);
                up = grid[j][i] == 0 ? 0 : up + 1;
                grid[j][i] = Math.min(grid[j][i], up);
                down = grid[s][i] == 0 ? 0 : down + 1;
                grid[s][i] = Math.min(grid[s][i], down);
            }
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) result = Math.max(result, grid[i][j]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def orderOfLargestPlusSign(self, n: int, mines: List[List[int]]) -> int:
        grid = [[n] * n for _ in range(n)]
        for mine in mines:
            grid[mine[0]][mine[1]] = 0
        for i in range(n):
            left = right = up = down = 0
            for j in range(n):
                left = left + 1 if grid[i][j] != 0 else 0
                grid[i][j] = min(grid[i][j], left)
                right = right + 1 if grid[i][(s := n - j - 1)] != 0 else 0
                grid[i][s] = min(grid[i][s], right)
                up = up + 1 if grid[j][i] != 0 else 0
                grid[j][i] = min(grid[j][i], up)
                down = down + 1 if grid[s][i] != 0 else 0
                grid[s][i] = min(grid[s][i], down)
        return max(max(row) for row in grid)
```
