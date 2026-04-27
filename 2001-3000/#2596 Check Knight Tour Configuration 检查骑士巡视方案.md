# 2596 Check Knight Tour Configuration 检查骑士巡视方案

__Description:__

There is a knight on an `n x n` chessboard. In a valid configuration, the knight starts __at the top-left cell__ of the board and visits every cell on the board __exactly once__.

You are given an `n x n` integer matrix `grid` consisting of distinct integers from the range `[0, n * n - 1]` where `grid[row][col]` indicates that the cell `(row, col)` is the `grid[row][col] ^ th` cell that the knight visited. The moves are __0-indexed__.

Return `true` _if_ `grid` _represents a valid configuration of the knight's movements or_ `false` _otherwise_.

Note that a valid knight move consists of moving two squares vertically and one square horizontally, or two squares horizontally and one square vertically. The figure below illustrates all the possible eight moves of a knight from some cell.

![2596-1](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

__Example:__

Example 1:

![2596-2](https://assets.leetcode.com/uploads/2022/12/28/yetgriddrawio-5.png)

```text
Input: grid = [[0,11,16,5,20],[17,4,19,10,15],[12,1,8,21,6],[3,18,23,14,9],[24,13,2,7,22]]
Output: true
Explanation: The above diagram represents the grid. It can be shown that it is a valid configuration.
```

Example 2:

![2596-3](https://assets.leetcode.com/uploads/2022/12/28/yetgriddrawio-6.png)

```text
Input: grid = [[0,3,6],[5,8,1],[2,7,4]]
Output: false
Explanation: The above diagram represents the grid. The 8th move of the knight is not valid considering its position after the 7th move.
```

__Constraints:__

- `n == grid.length == grid[i].length`
- `3 <= n <= 7`
- `0 <= grid[row][col] < n * n`
- All integers in `grid` are __unique__.

__题目描述:__

骑士在一张 `n x n` 的棋盘上巡视。在 __有效__ 的巡视方案中，骑士会从棋盘的 __左上角__ 出发，并且访问棋盘上的每个格子 __恰好一次__ 。

给你一个 `n x n` 的整数矩阵 `grid` ，由范围 `[0, n * n - 1]` 内的不同整数组成，其中 `grid[row][col]` 表示单元格 `(row, col)` 是骑士访问的第 `grid[row][col]` 个单元格。骑士的行动是从下标 __0__ 开始的。

如果 `grid` 表示了骑士的有效巡视方案，返回 `true`；否则返回 `false`。

注意，骑士行动时可以垂直移动两个格子且水平移动一个格子，或水平移动两个格子且垂直移动一个格子。下图展示了骑士从某个格子出发可能的八种行动路线。

![2596-4](https://pic.leetcode.cn/1694590028-CTMBQL-image.png)

__示例:__

示例 1：

![2596-5](https://pic.leetcode.cn/1694590044-AmhkRb-image.png)

```text
输入：grid = [[0,11,16,5,20],[17,4,19,10,15],[12,1,8,21,6],[3,18,23,14,9],[24,13,2,7,22]]
输出：true
解释：grid 如上图所示，可以证明这是一个有效的巡视方案。
```

示例 2：

![2596-6](https://pic.leetcode.cn/1694590057-FIMBAG-image.png)

```text
输入：grid = [[0,3,6],[5,8,1],[2,7,4]]
输出：false
解释：grid 如上图所示，考虑到骑士第 7 次行动后的位置，第 8 次行动是无效的。
```

__提示：__

- `n == grid.length == grid[i].length`
- `3 <= n <= 7`
- `0 <= grid[row][col] < n * n`
- `grid` 中的所有整数 __互不相同__

__思路:__

```text
模拟
将 grid 中的数字转化为哈希表
从左上角出发 pos[0] = 0
从 1 到 n * n - 1 遍历每个数字, 计算当前位置与上一个位置的距离
如果距离不等于 5, 则返回 false, 因为要么 abs(dx) = 2 且 abs(dy) = 1, 要么 abs(dx) = 1 且 abs(dy) = 2, dx ** 2 + dy ** 2 = 5
如果遍历完都没有问题, 则返回 true
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkValidGrid(vector<vector<int>>& grid) 
    {
        int n = grid.size(), p = 0, q = 0;
        unordered_map<int, int> pos;
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) pos[grid[i][j]] = 10 * i + j;
        if (pos[0]) return false;
        for (int i = 1; i < n * n; i++) if (((p = pos[i] / 10 - pos[i - 1] / 10) * p + (q = pos[i] % 10 - pos[i - 1] % 10) * q) != 5) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkValidGrid(int[][] grid) {
        int n = grid.length, p = 0, q = 0;
        var pos = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) pos.put(grid[i][j], 10 * i + j);
        if (pos.get(0) != 0) return false;
        for (int i = 1; i < n * n; i++) if (((p = pos.get(i) / 10 - pos.get(i - 1) / 10) * p + (q = pos.get(i) % 10 - pos.get(i - 1) % 10) * q) != 5) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkValidGrid(self, grid: List[List[int]]) -> bool:
        return (pos := {x: (i, j) for i, row in enumerate(grid) for j, x in enumerate(row)}) and pos[0] == (0, 0) and all(abs(pos[i][0] - pos[i - 1][0]) == 2 and abs(pos[i][1] - pos[i - 1][1]) == 1 or abs(pos[i][0] - pos[i - 1][0]) == 1 and abs(pos[i][1] - pos[i - 1][1]) == 2 for i in range(1, len(grid) ** 2))
```
