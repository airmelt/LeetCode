# 3127 Make a Square with the Same Color 构造相同颜色的正方形

__Description:__

You are given a 2D matrix `grid` of size `3 x 3` consisting only of characters `'B'` and `'W'`. Character `'W'` represents the white color, and character `'B'` represents the black color.

Your task is to change the color of __at most one__ cell so that the matrix has a `2 x 2` square where all cells are of the same color.

Return `true` if it is possible to create a `2 x 2` square of the same color, otherwise, return `false`.

__Example:__

Example 1:

```text
Input: grid = [["B","W","B"],["B","W","W"],["B","W","B"]]

Output: true

Explanation:
```

It can be done by changing the color of the `grid[0][2]`.

Example 2:

```text
Input: grid = [["B","W","B"],["W","B","W"],["B","W","B"]]

Output: false

Explanation:

It cannot be done by changing at most one cell.
```

Example 3:

```text
Input: grid = [["B","W","B"],["B","W","W"],["B","W","W"]]

Output: true

Explanation:
```

The `grid` already contains a `2 x 2` square of the same color.

__Constraints:__

- `grid.length == 3`
- `grid[i].length == 3`
- `grid[i][j]` is either `'W'` or `'B'`.

__题目描述:__

给你一个二维 `3 x 3` 的矩阵 `grid` ，每个格子都是一个字符，要么是 `'B'` ，要么是 `'W'` 。字符 `'W'` 表示白色，字符 `'B'` 表示黑色。

你的任务是改变 __至多一个__ 格子的颜色，使得矩阵中存在一个 `2 x 2` 颜色完全相同的正方形。

如果可以得到一个相同颜色的 `2 x 2` 正方形，那么返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：grid = [["B","W","B"],["B","W","W"],["B","W","B"]]

输出：true

解释：
```

修改 `grid[0][2]` 的颜色，可以满足要求。

示例 2：

```text
输入：grid = [["B","W","B"],["W","B","W"],["B","W","B"]]

输出：false

解释：

只改变一个格子颜色无法满足要求。
```

示例 3：

```text
输入：grid = [["B","W","B"],["B","W","W"],["B","W","W"]]

输出：true

解释：
```

`grid` 已经包含一个 `2 x 2` 颜色相同的正方形了。

__提示：__

- `grid.length == 3`
- `grid[i].length == 3`
- `grid[i][j]` 要么是 `'W'` ，要么是 `'B'` 。

__思路:__

```text
模拟
遍历所有 2 X 2 的正方形
由于 B 和 W 刚好最后 1 位不相同
可以使用位运算计算个数
只要 2 X 2 里不是 2 个 B 和 2 个 W 就返回 true
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canMakeSquare(vector<vector<char>>& grid) 
    {
        for (int i = 0; i < 2; i++) for (int j = 0; j < 2; j++) if ((grid[i][j] & 1) + (grid[i][j + 1] & 1) + (grid[i + 1][j] & 1) + (grid[i + 1][j + 1] & 1) != 2) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canMakeSquare(char[][] grid) {
        for (int i = 0; i < 2; i++) for (int j = 0; j < 2; j++) if ((grid[i][j] & 1) + (grid[i][j + 1] & 1) + (grid[i + 1][j] & 1) + (grid[i + 1][j + 1] & 1) != 2) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def canMakeSquare(self, grid: List[List[str]]) -> bool:
        
        return any(((grid[i][j] == 'B') + (grid[i + 1][j] == 'B') + (grid[i][j + 1] == 'B') + (grid[i + 1][j + 1] == 'B')) >= 3 or ((grid[i][j] == 'W') + (grid[i + 1][j] == 'W') + (grid[i][j + 1] == 'W') + (grid[i + 1][j + 1] == 'W')) >= 3 for i, j in ((0, 0), (0, 1), (1, 0), (1, 1)))
```
