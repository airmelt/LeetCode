# 1559 Detect Cycles in 2D Grid 二维网格图中探测环

__Description:__

Given a 2D array of characters `grid` of size `m x n`, you need to find if there exists any cycle consisting of the __same value__ in `grid`.

A cycle is a path of length 4 or more in the grid that starts and ends at the same cell. From a given cell, you can move to one of the cells adjacent to it - in one of the four directions (up, down, left, or right), if it has the same value of the current cell.

Also, you cannot move to the cell that you visited in your last move. For example, the cycle `(1, 1) -> (1, 2) -> (1, 1)` is invalid because from `(1, 2)` we visited `(1, 1)` which was the last visited cell.

Return `true` if any cycle of the same value exists in `grid`, otherwise, return `false`.

__Example:__

Example 1:

![1559-1](https://assets.leetcode.com/uploads/2020/07/15/1.png)

```text
Input: grid = [["a","a","a","a"],["a","b","b","a"],["a","b","b","a"],["a","a","a","a"]]
Output: true
Explanation: There are two valid cycles shown in different colors in the image below:
```

![1559-2](https://assets.leetcode.com/uploads/2020/07/15/11.png)

Example 2:

![1559-3](https://assets.leetcode.com/uploads/2020/07/15/22.png)

```text
Input: grid = [["c","c","c","a"],["c","d","c","c"],["c","c","e","c"],["f","c","c","c"]]
Output: true
Explanation: There is only one valid cycle highlighted in the image below:
```

![1559-4](https://assets.leetcode.com/uploads/2020/07/15/2.png)

Example 3:

![1559-5](https://assets.leetcode.com/uploads/2020/07/15/3.png)

```text
Input: grid = [["a","b","b"],["b","z","b"],["b","b","a"]]
Output: false
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 500`
- `grid` consists only of lowercase English letters.

__题目描述:__

给你一个二维字符网格数组 `grid` ，大小为 `m x n` ，你需要检查 `grid` 中是否存在 __相同值__ 形成的环。

一个环是一条开始和结束于同一个格子的长度 大于等于 4 的路径。对于一个给定的格子，你可以移动到它上、下、左、右四个方向相邻的格子之一，可以移动的前提是这两个格子有 相同的值 。

同时，你也不能回到上一次移动时所在的格子。比方说，环  `(1, 1) -> (1, 2) -> (1, 1)` 是不合法的，因为从 `(1, 2)` 移动到 `(1, 1)` 回到了上一次移动时的格子。

如果 `grid` 中有相同值形成的环，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![1559-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5482e1.png)

```text
输入：grid = [["a","a","a","a"],["a","b","b","a"],["a","b","b","a"],["a","a","a","a"]]
输出：true
解释：如下图所示，有 2 个用不同颜色标出来的环：
```

![1559-7](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5482e11.png)

示例 2：

![1559-8](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5482e2.png)

```text
输入：grid = [["c","c","c","a"],["c","d","c","c"],["c","c","e","c"],["f","c","c","c"]]
输出：true
解释：如下图所示，只有高亮所示的一个合法环：
```

![1559-9](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5482e22.png)

示例 3：

![1559-10](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/08/22/5482e3.png)

```text
输入：grid = [["a","b","b"],["b","z","b"],["b","b","a"]]
输出：false
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m <= 500`
- `1 <= n <= 500`
- `grid` 只包含小写英文字母。

__思路:__

```text
1. 并查集
将左边和上边相同的值加入并查集
如果已经加入并查集说明已经遇到环
为了优化二维数组可以用一维数组记录并查集的内容
时间复杂度为 O(MN), 空间复杂度为 O(MN)
2. DFS
可以使用原地修改将经过的值改成大写
遇到相同的值就继续搜索
当遇到来的坐标和当前坐标不相同的时候就可以返回 true
时间复杂度为 O(MN), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class UnionFind 
{
public:
    vector<int> parent;
    vector<int> size;
    int n;
public:
    UnionFind(int _n): n(_n), parent(_n), size(_n, 1) 
    {
        iota(parent.begin(), parent.end(), 0);
    }
    
    int find(int x) 
    {
        return parent[x] == x ? x : parent[x] = find(parent[x]);
    }
    
    void combine(int x, int y) 
    {
        if (size[x] < size[y]) swap(x, y);
        parent[y] = x;
        size[x] += size[y];
    }
    
    bool find_combine(int x, int y) 
    {
        x = find(x);
        y = find(y);
        if (x != y) 
        {
            combine(x, y);
            return true;
        }
        return false;
    }
};

class Solution 
{
public:
    bool containsCycle(vector<vector<char>>& grid) 
    {
        int m = grid.size(), n = grid.front().size();
        UnionFind uf(m * n);
        for (int i = 0; i < m; i++) 
        {
            for (int j = 0; j < n; j++) 
            {
                if (i and grid[i][j] == grid[i - 1][j]) if (!uf.find_combine(i * n + j, (i - 1) * n + j)) return true;
                if (j and grid[i][j] == grid[i][j - 1]) if (!uf.find_combine(i * n + j, i * n + j - 1)) return true;
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean containsCycle(char[][] grid) {
        for (int i = 0; i < grid.length; i++) for (int j = 0; j < grid[0].length; j++) if (Character.isLowerCase(grid[i][j]) && dfs(grid, 0, 0, i, j)) return true;
        return false;
    }

    private int[][] directions = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

    public boolean dfs(char[][] grid, int fromX, int fromY, int x, int y) {
        grid[x][y] = Character.toUpperCase(grid[x][y]);
        for (int[] direction : directions) {
            int toX = x + direction[0], toY = y + direction[1];
            if (0 <= toX && toX < grid.length && 0 <= toY && toY < grid[0].length && grid[x][y] == Character.toUpperCase(grid[toX][toY])) {
                if (Character.isUpperCase(grid[toX][toY]) && (toX != fromX || toY != fromY)) return true;
                else if (Character.isLowerCase(grid[toX][toY]) && dfs(grid, x, y, toX, toY)) return true;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def containsCycle(self, grid: List[List[str]]) -> bool:
        m, n, d = len(grid), len(grid[0]), ((0, 1), (0, -1), (1, 0), (-1, 0))
        
        def dfs(from_x: int, from_y: int, x: int, y: int) -> bool:
            grid[x][y] = grid[x][y].upper()
            for dx, dy in d:
                to_x, to_y = x + dx, y + dy
                if -1 < to_x < m and -1 < to_y < n and grid[x][y] == grid[to_x][to_y].upper():
                    if grid[to_x][to_y].isupper() and (to_x, to_y) != (from_x, from_y):
                        return True
                    elif grid[to_x][y + dy].islower() and dfs(x, y, to_x, to_y):
                        return True
            return False
        return any(grid[i][j].islower() and dfs(0, 0, i, j) for i in range(m) for j in range(n))
```
