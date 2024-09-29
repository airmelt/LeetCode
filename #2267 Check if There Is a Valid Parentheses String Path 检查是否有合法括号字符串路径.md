# 2267 Check if There Is a Valid Parentheses String Path 检查是否有合法括号字符串路径

__Description:__

A parentheses string is a __non-empty__ string consisting only of `'('` and `')'`. It is __valid__ if __any__ of the following conditions is __true__:

- It is `()`.
- It can be written as `AB` ( `A` concatenated with `B`), where `A` and `B` are valid parentheses strings.
- It can be written as `(A)`, where `A` is a valid parentheses string.

You are given an `m x n` matrix of parentheses `grid`. A __valid parentheses string path__ in the grid is a path satisfying __all__ of the following conditions:

- The path starts from the upper left cell `(0, 0)`.
- The path ends at the bottom-right cell `(m - 1, n - 1)`.
- The path only ever moves __down__ or __right__.
- The resulting parentheses string formed by the path is __valid__.

Return `true` _if there exists a __valid parentheses string path__ in the grid._ Otherwise, return `false`.

__Example:__

Example 1:

![2267-1](https://assets.leetcode.com/uploads/2022/03/15/example1drawio.png)

```text
Input: grid = [["(","(","("],[")","(",")"],["(","(",")"],["(","(",")"]]
Output: true
Explanation: The above diagram shows two possible paths that form valid parentheses strings.
The first path shown results in the valid parentheses string "()(())".
The second path shown results in the valid parentheses string "((()))".
Note that there may be other valid parentheses string paths.
```

Example 2:

![2267-2](https://assets.leetcode.com/uploads/2022/03/15/example2drawio.png)

```text
Input: grid = [[")",")"],["(","("]]
Output: false
Explanation: The two possible paths form the parentheses strings "))(" and ")((". Since neither of them are valid parentheses strings, we return false.
```

__Constraints:__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `grid[i][j]` is either `'('` or `')'`.

__题目描述:__

一个括号字符串是一个 __非空__ 且只包含 `'('` 和 `')'` 的字符串。如果下面 __任意__ 条件为 __真__ ，那么这个括号字符串就是 __合法的__ 。

- 字符串是 `()` 。
- 字符串可以表示为 `AB`（ `A` 连接 `B`）， `A` 和 `B` 都是合法括号序列。
- 字符串可以表示为 `(A)` ，其中 `A` 是合法括号序列。

给你一个 `m x n` 的括号网格图矩阵 `grid` 。网格图中一个 __合法括号路径__ 是满足以下所有条件的一条路径:

- 路径开始于左上角格子 `(0, 0)` 。
- 路径结束于右下角格子 `(m - 1, n - 1)` 。
- 路径每次只会向 __下__ 或者向 __右__ 移动。
- 路径经过的格子组成的括号字符串是 __合法__ 的。

如果网格图中存在一条 __合法括号路径__ ，请返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

![2267-3](https://assets.leetcode.com/uploads/2022/03/15/example1drawio.png)

```text
输入：grid = [["(","(","("],[")","(",")"],["(","(",")"],["(","(",")"]]
输出：true
解释：上图展示了两条路径，它们都是合法括号字符串路径。
第一条路径得到的合法字符串是 "()(())" 。
第二条路径得到的合法字符串是 "((()))" 。
注意可能有其他的合法括号字符串路径。
```

示例 2：

![2267-4](https://assets.leetcode.com/uploads/2022/03/15/example2drawio.png)

```text
输入：grid = [[")",")"],["(","("]]
输出：false
解释：两条可行路径分别得到 "))(" 和 ")((" 。由于它们都不是合法括号字符串，我们返回 false 。
```

__提示：__

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `grid[i][j]` 要么是 `'('` ，要么是 `')'` 。

__思路:__

```text
1. DFS ➕ 剪枝
需要走的步数为 m + n - 1, 所以如果 m + n 为偶数, 或者左上角和右下角不是 '(' 和 ')' 则返回 false
使用 visited 数组记录当前位置和当前括号数是否访问过, 访问过的不需要再次访问
对于同一个格子, 最多的左括号累计数为 m + n + 1 >> 1, 当左括号累计数 > m - x + n - y - 1 时, 返回 false, 这是因为剩下的格子不足以补齐右括号
如果走到右下角, 则判断左括号累计数是否为 1, 如果是则返回 true, 因为右下角的格子一定是 ')', 否则之前已经被剪枝
每次尝试往右或者往下直到搜索完所有路径
时间复杂度为 O(MN(M + N)), 空间复杂度为 O(MN(M + N))
2. 位运算 ➕ 动态规划
设 '(' 为 1, ')' 为 -1, 从左上角到右下角的路径和为 0
记录每个位置可能的前缀和
合法路径上的前缀和始终非负
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool hasValidPath(vector<vector<char>>& grid) 
    {
        int m = grid.front().size();
        vector<__uint128_t> dp(m);
        dp.front() = 1;
        for (const auto& row : grid)
        {
            for (int i = 0; i < m; i++) 
            {
                if (i) dp[i] |= dp[i - 1];
                if (row[i] == '(') dp[i] <<= 1;
                else dp[i] >>= 1;
            }
        }
        return dp.back() & 1;
    }
};
```

__Java__:

```Java
class Solution {
    private int m, n;
    private char[][] grid;
    private boolean[][][] visited;

    public boolean hasValidPath(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        if (((m + n) & 1) == 0 || grid[0][0] == ')' || grid[m - 1][n - 1] == '(') return false;
        this.grid = grid;
        visited = new boolean[m][n][(m + n + 1) >>> 1];
        return dfs(0, 0, 0);
    }

    boolean dfs(int x, int y, int c) {
        if (c > m - x + n - y - 1) return false;
        if (x == m - 1 && y == n - 1) return c == 1;
        if (visited[x][y][c]) return false;
        visited[x][y][c] = true;
        c += grid[x][y] == '(' ? 1 : -1;
        return c > -1 && (x < m - 1 && dfs(x + 1, y, c) || y < n - 1 && dfs(x, y + 1, c));
    }
}
```

__Python__:

```Python
class Solution:
    def hasValidPath(self, grid: List[List[str]]) -> bool:
        m, n = len(grid), len(grid[0])
        if not (m + n) & 1 or grid[0][0] == ')' or grid[-1][-1] == '(': 
            return False

        @lru_cache(None)
        def dfs(x: int, y: int, c: int) -> bool:
            if c > m - x + n - y - 1: 
                return False
            if (x, y) == (m - 1, n - 1):
                return c == 1
            c += 1 if grid[x][y] == '(' else -1
            return c > -1 and (x < m - 1 and dfs(x + 1, y, c) or y < n - 1 and dfs(x, y + 1, c))
        return dfs(0, 0, 0)
```
