# 2352 Equal Row and Column Pairs 相等行列对

__Description:__

Given a __0-indexed__ `n x n` integer matrix `grid`, _return the number of pairs_ `(ri, cj)` _such that row_ `ri` _and column_ `cj` _are equal_.

A row and column pair is considered equal if they contain the same elements in the same order (i.e., an equal array).

__Example:__

Example 1:

![2352-1](https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg)

```text
Input: grid = [[3,2,1],[1,7,6],[2,7,7]]
Output: 1
Explanation: There is 1 equal row and column pair:
- (Row 2, Column 1): [2,7,7]
```

Example 2:

![2352-2](https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg)

```text
Input: grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
Output: 3
Explanation: There are 3 equal row and column pairs:
- (Row 0, Column 0): [3,1,2,2]
- (Row 2, Column 2): [2,4,2,2]
- (Row 3, Column 2): [2,4,2,2]
```

__Constraints:__

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `1 <= grid[i][j] <= 10 ^ 5`

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `n x n` 的整数矩阵 `grid` ，返回满足 `Ri` 行和 `Cj` 列相等的行列对 `(Ri, Cj)` 的数目_。_

如果行和列以相同的顺序包含相同的元素（即相等的数组），则认为二者是相等的。

__示例:__

示例 1：

![2352-3](https://assets.leetcode.com/uploads/2022/06/01/ex1.jpg)

```text
输入：grid = [[3,2,1],[1,7,6],[2,7,7]]
输出：1
解释：存在一对相等行列对：
- (第 2 行，第 1 列)：[2,7,7]
```

示例 2：

![2352-4](https://assets.leetcode.com/uploads/2022/06/01/ex2.jpg)

```text
输入：grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
输出：3
解释：存在三对相等行列对：
- (第 0 行，第 0 列)：[3,1,2,2]
- (第 2 行, 第 2 列)：[2,4,2,2]
- (第 3 行, 第 2 列)：[2,4,2,2]
```

__提示：__

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `1 <= grid[i][j] <= 10 ^ 5`

__思路:__

```text
1. 暴力法
遍历所有的行和列
检查是否相等
时间复杂度为 O(N ^ 3), 空间复杂度为 O(1)
2. 哈希表
用哈希表存储每一行的元素
遍历每一列, 检查是否存在相等的行
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int equalPairs(vector<vector<int>>& grid) 
    {
        for (int i = 0, n = grid.size(); i < n; i++) for (int j = 0; j < n; j++) if (helper(i, j, grid)) ++result;
        return result;
    }
private:
    int result = 0;
    bool helper(int row, int col, vector<vector<int>>& grid) 
    {
        for (int i = 0, n = grid.size(); i < n; i++) if (grid[row][i] != grid[i][col]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public int equalPairs(int[][] grid) {
        int n = grid.length, result = 0;
        Map<List<Integer>, Integer> map = new HashMap<>();
        for (var row : grid) map.merge(Arrays.stream(row).boxed().collect(Collectors.toList()), 1, Integer::sum);
        for (int j = 0; j < n; j++) {
            List<Integer> arr = new ArrayList<Integer>();
            for (int i = 0; i < n; i++) arr.add(grid[i][j]);
            result += map.getOrDefault(arr, 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def equalPairs(self, grid: List[List[int]]) -> int:
        return sum(c[s] * r[s] for s in c if s in r) if (r := Counter(('#'.join(map(str, row))) for row in grid)) and (c := Counter(('#'.join(map(str, col))) for col in zip(*grid))) else 0
```
