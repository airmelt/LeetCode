# 2850 Minimum Moves to Spread Stones Over Grid 将石头分散到网格图的最少移动次数

__Description:__

You are given a __0-indexed__ 2D integer matrix `grid` of size `3 * 3`, representing the number of stones in each cell. The grid contains exactly `9` stones, and there can be __multiple__ stones in a single cell.

In one move, you can move a single stone from its current cell to any other cell if the two cells share a side.

Return the minimum number of moves required to place one stone in each cell.

__Example:__

Example 1:

![2850-1](https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg)

```text
Input: grid = [[1,1,0],[1,1,1],[1,2,1]]
Output: 3
Explanation: One possible sequence of moves to place one stone in each cell is: 
1- Move one stone from cell (2,1) to cell (2,2).
2- Move one stone from cell (2,2) to cell (1,2).
3- Move one stone from cell (1,2) to cell (0,2).
In total, it takes 3 moves to place one stone in each cell of the grid.
It can be shown that 3 is the minimum number of moves required to place one stone in each cell.
```

Example 2:

![2850-2](https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg)

```text
Input: grid = [[1,3,0],[1,0,0],[1,0,3]]
Output: 4
Explanation: One possible sequence of moves to place one stone in each cell is:
1- Move one stone from cell (0,1) to cell (0,2).
2- Move one stone from cell (0,1) to cell (1,1).
3- Move one stone from cell (2,2) to cell (1,2).
4- Move one stone from cell (2,2) to cell (2,1).
In total, it takes 4 moves to place one stone in each cell of the grid.
It can be shown that 4 is the minimum number of moves required to place one stone in each cell.
```

__Constraints:__

- `grid.length == grid[i].length == 3`
- `0 <= grid[i][j] <= 9`
- Sum of `grid` is equal to `9`.

__题目描述:__

给你一个大小为 `3 * 3` ，下标从 __0__ 开始的二维整数矩阵 `grid` ，分别表示每一个格子里石头的数目。网格图中总共恰好有 `9` 个石头，一个格子里可能会有 __多个__ 石头。

每一次操作中，你可以将一个石头从它当前所在格子移动到一个至少有一条公共边的相邻格子。

请你返回每个格子恰好有一个石头的 最少移动次数 。

__示例:__

示例 1：

![2850-3](https://assets.leetcode.com/uploads/2023/08/23/example1-3.svg)

```text
输入：grid = [[1,1,0],[1,1,1],[1,2,1]]
输出：3
解释：让每个格子都有一个石头的一个操作序列为：
1 - 将一个石头从格子 (2,1) 移动到 (2,2) 。
2 - 将一个石头从格子 (2,2) 移动到 (1,2) 。
3 - 将一个石头从格子 (1,2) 移动到 (0,2) 。
总共需要 3 次操作让每个格子都有一个石头。
让每个格子都有一个石头的最少操作次数为 3 。
```

示例 2：

![2850-4](https://assets.leetcode.com/uploads/2023/08/23/example2-2.svg)

```text
输入：grid = [[1,3,0],[1,0,0],[1,0,3]]
输出：4
解释：让每个格子都有一个石头的一个操作序列为：
1 - 将一个石头从格子 (0,1) 移动到 (0,2) 。
2 - 将一个石头从格子 (0,1) 移动到 (1,1) 。
3 - 将一个石头从格子 (2,2) 移动到 (1,2) 。
4 - 将一个石头从格子 (2,2) 移动到 (2,1) 。
总共需要 4 次操作让每个格子都有一个石头。
让每个格子都有一个石头的最少操作次数为 4 。
```

__提示：__

- `grid.length == grid[i].length == 3`
- `0 <= grid[i][j] <= 9`
- `grid` 中元素之和为 `9` 。

__思路:__

```text
暴力法
先计算出所有可以拿出的格子 a
同时计算出需要放入的格子 b
那么对 a 进行全排列
尝试将 a 的格子移动到每一个 b 中的格子
计算曼哈顿距离
更新距离的最小值
时间复杂度为 O(MN(MN)!), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumMoves(vector<vector<int>>& grid) 
    {
        vector<pair<int, int>> a, b;
        int m = grid.size(), n = grid.front().size(), result = INT_MAX;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j]) for (int k = 1; k < grid[i][j]; k++) a.emplace_back(i, j);
                else b.emplace_back(i, j);
            }
        }
        do {
            int total = 0;
            for (int i = 0; i < a.size(); i++) total += abs(a[i].first - b[i].first) + abs(a[i].second - b[i].second);
            result = min(result, total);
        } while (next_permutation(a.begin(), a.end()));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumMoves(int[][] grid) {
        List<int[]> a = new ArrayList<>(), b = new ArrayList<>();
        int m = grid.length, n = grid[0].length, result = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] > 1) for (int k = 1; k < grid[i][j]; k++) a.add(new int[]{i, j});
                else if (grid[i][j] == 0) b.add(new int[]{i, j});
            }
        }
        for (var a1 : permutations(a)) {
            int total = 0;
            for (int i = 0; i < a1.size(); i++) total += Math.abs(a1.get(i)[0] - b.get(i)[0]) + Math.abs(a1.get(i)[1] - b.get(i)[1]);
            result = Math.min(result, total);
        }
        return result;
    }

    private List<List<int[]>> permutations(List<int[]> arr) {
        List<List<int[]>> result = new ArrayList<>();
        permute(arr, 0, result);
        return result;
    }

    private void permute(List<int[]> arr, int start, List<List<int[]>> result) {
        if (start == arr.size()) result.add(new ArrayList<>(arr));
        for (int i = start; i < arr.size(); i++) {
            swap(arr, start, i);
            permute(arr, start + 1, result);
            swap(arr, start, i);
        }
    }

    private void swap(List<int[]> arr, int i, int j) {
        int[] temp = arr.get(i);
        arr.set(i, arr.get(j));
        arr.set(j, temp);
    }
}
```

__Python__:

```Python
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        a, b, result = [], [], inf
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                if v > 1:
                    a.extend([(i, j)] * (v - 1))
                elif not v:
                    b.append((i, j))
        return min(sum(abs(x1 - x2) + abs(y1 - y2) for (x1, y1), (x2, y2) in zip(a1, b)) for a1 in permutations(a))
```
