# 1444 Number of Ways of Cutting a Pizza 切披萨的方案数

__Description:__

Given a rectangular pizza represented as a `rows x cols` matrix containing the following characters: `'A'` (an apple) and `'.'` (empty cell) and given the integer `k`. You have to cut the pizza into `k` pieces using `k-1` cuts.

For each cut you choose the direction: vertical or horizontal, then you choose a cut position at the cell boundary and cut the pizza into two pieces. If you cut the pizza vertically, give the left part of the pizza to a person. If you cut the pizza horizontally, give the upper part of the pizza to a person. Give the last piece of pizza to the last person.

Return the number of ways of cutting the pizza such that each piece contains at least one apple. Since the answer can be a huge number, return this modulo 10 ^ 9 + 7.

__Example:__

Example 1:

![1444-1](https://assets.leetcode.com/uploads/2020/04/23/ways_to_cut_apple_1.png)

```text
Input: pizza = ["A..","AAA","..."], k = 3
Output: 3 
Explanation: The figure above shows the three ways to cut the pizza. Note that pieces must contain at least one apple.
```

Example 2:

```text
Input: pizza = ["A..","AA.","..."], k = 3
Output: 1
```

Example 3:

```text
Input: pizza = ["A..","A..","..."], k = 1
Output: 1
```

__Constraints:__

- `1 <= rows, cols <= 50`
- `rows == pizza.length`
- `cols == pizza[i].length`
- `1 <= k <= 10`
- `pizza` consists of characters `'A'` and `'.'` only.

__题目描述:__

给你一个 `rows x cols` 大小的矩形披萨和一个整数 `k` ，矩形包含两种字符： `'A'` （表示苹果）和 `'.'` （表示空白格子）。你需要切披萨 `k-1` 次，得到 `k` 块披萨并送给别人。

切披萨的每一刀，先要选择是向垂直还是水平方向切，再在矩形的边界上选一个切的位置，将披萨一分为二。如果垂直地切披萨，那么需要把左边的部分送给一个人，如果水平地切，那么需要把上面的部分送给一个人。在切完最后一刀后，需要把剩下来的一块送给最后一个人。

请你返回确保每一块披萨包含 至少 一个苹果的切披萨方案数。由于答案可能是个很大的数字，请你返回它对 10 ^ 9 + 7 取余的结果。

__示例:__

示例 1：

![1444-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/10/ways_to_cut_apple_1.png)

```text
输入：pizza = ["A..","AAA","..."], k = 3
输出：3 
解释：上图展示了三种切披萨的方案。注意每一块披萨都至少包含一个苹果。
```

示例 2：

```text
输入：pizza = ["A..","AA.","..."], k = 3
输出：1
```

示例 3：

```text
输入：pizza = ["A..","A..","..."], k = 1
输出：1
```

__提示：__

- `1 <= rows, cols <= 50`
- `rows == pizza.length`
- `cols == pizza[i].length`
- `1 <= k <= 10`
- `pizza` 只包含字符 `'A'` 和 `'.'` 。

__思路:__

```text
动态规划 ➕ 前缀和
首先可以从 10 ^ 9 + 7 推测使用动态规划
因为每次分割披萨需要把左边或者上边的给别人也就是说会留下右下角的块
那么可以记录留下块的左上角的坐标 (i, j)
另外还需要记录 k
令 dp[i][j][p] 表示左上角坐标为 (i, j) 时, 披萨此时为 p 块的方案数
每切一刀披萨就多一块, 初始状态为 dp[0][0][1] = 1, 左上角坐标为 (0, 0) 时, 披萨被切成 1 块的方案数为 1
如果新切割的位置为 (x, y), 需要保证切割前后两遍都要有苹果, dp[x][y][p + 1] += dp[i][j][k], 注意这里 x 和 y 肯定有一个与 i 和 j 相等, 另一个不等
所以其实转移方程是 dp[x][y][k + 1] = sum(dp[x][j][k] + dp[i][y][k]), 对于所有的 (i, j), 0 <= i < x < m, 0 <= j < y < n
那么如何保证切割的两边都有苹果呢, 可以使用前缀和的方式记录
这样查询苹果是否存在只需要 O(1) 的时间复杂度
注意到只使用了上一个 k 更新当前的 k, 可以用滚动数组的方式优化空间复杂度
时间复杂度为 O(KMN(M + N)), 空间复杂度为 O(MN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int ways(vector<string>& pizza, int k) 
    {
        int m = pizza.size(), n = pizza.front().length(), mod = 1e9 + 7;
        vector<vector<int>> pre(m, vector<int>(n)), dp(m, vector<int>(n));
        for (int i = m - 1; i > -1; i--) for (int j = n - 1; j > -1; j--) pre[i][j] = (pizza[i][j] == 'A') + ((i == m - 1 and j == n - 1) ? 0 : (i == m - 1 ? pre[i][j + 1] : (j == n - 1 ? pre[i + 1][j] : (pre[i][j + 1] + pre[i + 1][j] - pre[i + 1][j + 1]))));
        for (int p = 0; p < k; p++) 
        {
            vector<vector<int>> cur(m, vector<int>(n));
            for (int i = m - 1; i > -1; i--) 
            {
                for (int j = n - 1; j > -1; j--) 
                {
                    if (!p) cur[i][j] = (pre[i][j] > 0);
                    else 
                    {
                        for (int x = i; x < m - 1; x++) cur[i][j] = (cur[i][j] + (pre[i][j] - pre[x + 1][j] > 0 ? dp[x + 1][j] : 0)) % mod;
                        for (int y = j; y < n - 1; y++) cur[i][j] = (cur[i][j] + (pre[i][j] - pre[i][y + 1] > 0 ? dp[i][y + 1] : 0)) % mod;
                    }
                }
            }
            dp = cur;
        }
        return dp.front().front();
    }
};
```

__Java__:

```Java
class Solution {
    public int ways(String[] pizza, int k) {
        int m = pizza.length, n = pizza[0].length(), mod = 1_000_000_007, pre[][] = new int[m][n], dp[][][] = new int[m][n][k];
        for (int i = m - 1; i > -1; i--) for (int j = n - 1; j > -1; j--) pre[i][j] = (pizza[i].charAt(j) == 'A' ? 1 : 0) + ((i == m - 1 && j == n - 1) ? 0 : (i == m - 1 ? pre[i][j + 1] : (j == n - 1 ? pre[i + 1][j] : (pre[i][j + 1] + pre[i + 1][j] - pre[i + 1][j + 1]))));
        for (int p = 0; p < k; p++) {
            for (int i = m - 1; i > -1; i--) {
                for (int j = n - 1; j > -1; j--) {
                    if (p == 0) dp[i][j][p] = (pre[i][j] > 0 ? 1 : 0);
                    else {
                        for (int x = i; x < m - 1; x++) dp[i][j][p] = (dp[i][j][p] + (pre[i][j] - pre[x + 1][j] > 0 ? dp[x + 1][j][p - 1] : 0)) % mod;
                        for (int y = j; y < n - 1; y++) dp[i][j][p] = (dp[i][j][p] + (pre[i][j] - pre[i][y + 1] > 0 ? dp[i][y + 1][p - 1] : 0)) % mod;
                    }
                }
            }
        }
        return dp[0][0][k - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def ways(self, pizza: List[str], k: int) -> int:
        m, n, mod = len(pizza), len(pizza[0]), 10 ** 9 + 7
        @lru_cache(None)
        def f(x: int, y: int, k: int) -> int:
            if not k:
                return any('A' in p[y:] for p in pizza[x:])
            result = 0
            for i in range(x + 1, m):
                if any('A' in p[y:n] for p in pizza[x:i]):
                    result += f(i, y, k - 1)
            for j in range(y + 1, n):
                if any('A' in p[y:j] for p in pizza[x:]):
                    result += f(x, j, k - 1)
            return result
        return f(0, 0, k - 1) % mod
```
