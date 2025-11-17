# 2768 Number of Black Blocks 黑格子的数目

__Description:__

You are given two integers `m` and `n` representing the dimensions of a __0-indexed__ `m x n` grid.

You are also given a __0-indexed__ 2D integer matrix `coordinates`, where `coordinates[i] = [x, y]` indicates that the cell with coordinates `[x, y]` is colored __black__. All cells in the grid that do not appear in `coordinates` are __white__.

A block is defined as a `2 x 2` submatrix of the grid. More formally, a block with cell `[x, y]` as its top-left corner where `0 <= x < m - 1` and `0 <= y < n - 1` contains the coordinates `[x, y]`, `[x + 1, y]`, `[x, y + 1]`, and `[x + 1, y + 1]`.

Return _a __0-indexed__ integer array_ `arr` _of size_ `5` _such that_ `arr[i]` _is the number of blocks that contains exactly_ `i` ___black__ cells_.

__Example:__

Example 1:

```text
Input: m = 3, n = 3, coordinates = [[0,0]]
Output: [3,1,0,0,0]
Explanation: The grid looks like this:
There is only 1 block with one black cell, and it is the block starting with cell [0,0].
The other 3 blocks start with cells [0,1], [1,0] and [1,1]. They all have zero black cells. 
Thus, we return [3,1,0,0,0].
```

![2768-1](https://assets.leetcode.com/uploads/2023/06/18/screen-shot-2023-06-18-at-44656-am.png)

Example 2:

```text
Input: m = 3, n = 3, coordinates = [[0,0],[1,1],[0,2]]
Output: [0,2,2,0,0]
Explanation: The grid looks like this:
There are 2 blocks with two black cells (the ones starting with cell coordinates [0,0] and [0,1]).
The other 2 blocks have starting cell coordinates of [1,0] and [1,1]. They both have 1 black cell.
Therefore, we return [0,2,2,0,0].
```

![2768-2](https://assets.leetcode.com/uploads/2023/06/18/screen-shot-2023-06-18-at-45018-am.png)

__Constraints:__

- `2 <= m <= 10 ^ 5`
- `2 <= n <= 10 ^ 5`
- `0 <= coordinates.length <= 10 ^ 4`
- `coordinates[i].length == 2`
- `0 <= coordinates[i][0] < m`
- `0 <= coordinates[i][1] < n`
- It is guaranteed that `coordinates` contains pairwise distinct coordinates.

__题目描述:__

给你两个整数 `m` 和 `n` ，表示一个下标从 __0__ 开始的 `m x n` 的网格图。

给你一个下标从 __0__ 开始的二维整数矩阵 `coordinates` ，其中 `coordinates[i] = [x, y]` 表示坐标为 `[x, y]` 的格子是 __黑色的__ ，所有没出现在 `coordinates` 中的格子都是 __白色的__。

一个块定义为网格图中 `2 x 2` 的一个子矩阵。更正式的，对于左上角格子为 `[x, y]` 的块，其中 `0 <= x < m - 1` 且 `0 <= y < n - 1` ，包含坐标为 `[x, y]` ， `[x + 1, y]` ， `[x, y + 1]` 和 `[x + 1, y + 1]` 的格子。

请你返回一个下标从 __0__ 开始长度为 `5` 的整数数组 `arr` ， `arr[i]` 表示恰好包含 `i` 个 __黑色__ 格子的块的数目。

__示例:__

示例 1：

```text
输入：m = 3, n = 3, coordinates = [[0,0]]
输出：[3,1,0,0,0]
解释：网格图如下：
只有 1 个块有一个黑色格子，这个块是左上角为 [0,0] 的块。
其他 3 个左上角分别为 [0,1] ，[1,0] 和 [1,1] 的块都有 0 个黑格子。
所以我们返回 [3,1,0,0,0] 。
```

![2768-3](https://assets.leetcode.com/uploads/2023/06/18/screen-shot-2023-06-18-at-44656-am.png)

示例 2：

```text
输入：m = 3, n = 3, coordinates = [[0,0],[1,1],[0,2]]
输出：[0,2,2,0,0]
解释：网格图如下：
有 2 个块有 2 个黑色格子（左上角格子分别为 [0,0] 和 [0,1]）。
左上角为 [1,0] 和 [1,1] 的两个块，都有 1 个黑格子。
所以我们返回 [0,2,2,0,0] 。
```

![2768-4](https://assets.leetcode.com/uploads/2023/06/18/screen-shot-2023-06-18-at-45018-am.png)

__提示：__

- `2 <= m <= 10 ^ 5`
- `2 <= n <= 10 ^ 5`
- `0 <= coordinates.length <= 10 ^ 4`
- `coordinates[i].length == 2`
- `0 <= coordinates[i][0] < m`
- `0 <= coordinates[i][1] < n`
- `coordinates` 中的坐标对两两互不相同。

__思路:__

```text
枚举
显然黑色格子会比矩阵稀疏
遍历 coordinates
对于每一个黑色格子
如果没有访问过
将它四周的 2X2 的格子都检查一遍
即对于黑色格子 (x, y)
我们以左上角为 2X2 格子的基点
访问 [x - 1, y - 1], [x - 1, y], [x, y - 1], [x, y] 4 个格子, 注意对边界进行判断
访问对应格子检查其中的格子是否可以染色, 染色完成之后将这个格子加入结果计数中
最后染色完成之后还未访问过的就是 0 的总数
用 (m - 1) * (n - 1) - visited.size() 即可
时间复杂度为 O(C), 空间复杂度为 O(C), C 为 coordinates 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> countBlackBlocks(int m, int n, vector<vector<int>>& coordinates) 
    {
        long long offset = 1e6;
        vector<long long> result(5);
        unordered_set<long long> visited, s;
        for (const auto& c : coordinates) s.insert(offset * c.front() + c.back());
        for (const auto& c : coordinates)
        {
            for (int x = c.front(), y = c.back(), i = max(x - 1, 0); i < min(x + 1, m - 1); i++) 
            {
                for (int j = max(y - 1, 0); j < min(y + 1, n - 1); j++) 
                {
                    if (!visited.count(offset * i + j)) 
                    {
                        visited.insert(offset * i + j);
                        ++result[s.count(offset * i + j) + s.count(offset * (i + 1) + j) + s.count(offset * i + j + 1) + s.count(offset * (i + 1) + j + 1)];
                    }
                }
            }
        }
        result.front() = (long long)(m - 1) * (n - 1) - visited.size();
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] countBlackBlocks(int m, int n, int[][] coordinates) {
        long result[] = new long[5], offset = 1_000_000;
        Set<Long> visited = new HashSet<>(), s = new HashSet<>();
        for (var c : coordinates) s.add(offset * c[0] + c[1]);
        for (var c : coordinates) {
            for (int x = c[0], y = c[1], i = Math.max(x - 1, 0); i < Math.min(x + 1, m - 1); i++) {
                for (int j = Math.max(y - 1, 0); j < Math.min(y + 1, n - 1); j++) {
                    if (!visited.contains(offset * i + j)) {
                        visited.add(offset * i + j);
                        ++result[(s.contains(offset * i + j) ? 1 : 0) + (s.contains(offset * (i + 1) + j) ? 1 : 0) + (s.contains(offset * i + j + 1) ? 1 : 0) + (s.contains(offset * (i + 1) + j + 1) ? 1 : 0)];
                    }
                }
            }
        }
        result[0] = (long)(m - 1) * (n - 1) - visited.size();
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countBlackBlocks(self, m: int, n: int, coordinates: List[List[int]]) -> List[int]:
        visited, result, s = set(), [0] * 5, set(map(tuple, coordinates))
        for x, y in coordinates:
            for i in range(max(x - 1, 0), min(x + 1, m - 1)):
                for j in range(max(y - 1, 0), min(y + 1, n - 1)):
                    if (i, j) not in visited:
                        visited.add((i, j))
                        result[((i, j) in s) + ((i, j + 1) in s) + ((i + 1, j) in s) + ((i + 1, j + 1) in s)] += 1
        return [(m - 1) * (n - 1) - len(visited)] + result[1:]
```
