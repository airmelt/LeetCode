# 2906 Construct Product Matrix 构造乘积矩阵

__Description:__

Given a __0-indexed__ 2D integer matrix `grid` of size `n * m`, we define a __0-indexed__ 2D matrix `p` of size `n * m` as the __product__ matrix of `grid` if the following condition is met:

- Each element `p[i][j]` is calculated as the product of all elements in `grid` except for the element `grid[i][j]`. This product is then taken modulo `12345`.

Return _the product matrix of_ `grid`.

__Example:__

Example 1:

```text
Input: grid = [[1,2],[3,4]]
Output: [[24,12],[8,6]]
Explanation: p[0][0] = grid[0][1] * grid[1][0] * grid[1][1] = 2 * 3 * 4 = 24
p[0][1] = grid[0][0] * grid[1][0] * grid[1][1] = 1 * 3 * 4 = 12
p[1][0] = grid[0][0] * grid[0][1] * grid[1][1] = 1 * 2 * 4 = 8
p[1][1] = grid[0][0] * grid[0][1] * grid[1][0] = 1 * 2 * 3 = 6
So the answer is [[24,12],[8,6]].
```

Example 2:

```text
Input: grid = [[12345],[2],[1]]
Output: [[2],[0],[0]]
Explanation: p[0][0] = grid[0][1] * grid[0][2] = 2 * 1 = 2.
p[0][1] = grid[0][0] * grid[0][2] = 12345 * 1 = 12345. 12345 % 12345 = 0. So p[0][1] = 0.
p[0][2] = grid[0][0] * grid[0][1] = 12345 * 2 = 24690. 24690 % 12345 = 0. So p[0][2] = 0.
So the answer is [[2],[0],[0]].
```

__Constraints:__

- `1 <= n == grid.length <= 10 ^ 5`
- `1 <= m == grid[i].length <= 10 ^ 5`
- `2 <= n * m <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `n * m` 的二维整数矩阵 `grid` ，定义一个下标从 __0__ 开始、大小为 `n * m` 的的二维矩阵 `p`。如果满足以下条件，则称 `p` 为 `grid` 的 __乘积矩阵__ :

- 对于每个元素 `p[i][j]` ，它的值等于除了 `grid[i][j]` 外所有元素的乘积。乘积对 `12345` 取余数。

返回 `grid` 的乘积矩阵。

__示例:__

示例 1：

```text
输入：grid = [[1,2],[3,4]]
输出：[[24,12],[8,6]]
解释：p[0][0] = grid[0][1] * grid[1][0] * grid[1][1] = 2 * 3 * 4 = 24
p[0][1] = grid[0][0] * grid[1][0] * grid[1][1] = 1 * 3 * 4 = 12
p[1][0] = grid[0][0] * grid[0][1] * grid[1][1] = 1 * 2 * 4 = 8
p[1][1] = grid[0][0] * grid[0][1] * grid[1][0] = 1 * 2 * 3 = 6
所以答案是 [[24,12],[8,6]] 。
```

示例 2：

```text
输入：grid = [[12345],[2],[1]]
输出：[[2],[0],[0]]
解释：p[0][0] = grid[0][1] * grid[0][2] = 2 * 1 = 2
p[0][1] = grid[0][0] * grid[0][2] = 12345 * 1 = 12345. 12345 % 12345 = 0 ，所以 p[0][1] = 0
p[0][2] = grid[0][0] * grid[0][1] = 12345 * 2 = 24690. 24690 % 12345 = 0 ，所以 p[0][2] = 0
所以答案是 [[2],[0],[0]] 。
```

__提示：__

- `1 <= n == grid.length <= 10 ^ 5`
- `1 <= m == grid[i].length <= 10 ^ 5`
- `2 <= n * m <= 10 ^ 5`
- `1 <= grid[i][j] <= 10 ^ 9`

__思路:__

```text
前缀和
如果是一维数组
简单的从前后分别遍历一次数组就能得到
所有元素的乘积除以自身的值
和这个类似
只是使用了二维数组遍历
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> constructProductMatrix(vector<vector<int>>& grid) 
    {
        int MOD = 12345, n = grid.size(), m = grid.front().size();
        vector<vector<int>> result(n, vector<int>(m));
        long long suf = 1LL, pre = 1LL;
        for (int i = n - 1; i > -1; i--) for (int j = m - 1; j > -1; suf = suf * grid[i][j--] % MOD) result[i][j] = suf;
        for (int i = 0; i < n; i++) for (int j = 0; j < m; pre = pre * grid[i][j++] % MOD) result[i][j] = result[i][j] * pre % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] constructProductMatrix(int[][] grid) {
        int MOD = 12345, n = grid.length, m = grid[0].length, result[][] = new int[n][m];
        long suf = 1L, pre = 1L;
        for (int i = n - 1; i > -1; i--) for (int j = m - 1; j > -1; suf = suf * grid[i][j--] % MOD) result[i][j] = (int)suf;
        for (int i = 0; i < n; i++) for (int j = 0; j < m; pre = pre * grid[i][j++] % MOD) result[i][j] = (int)(result[i][j] * pre % MOD);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def constructProductMatrix(self, grid: List[List[int]]) -> List[List[int]]:
        MOD, n, m = 12345, len(grid), len(grid[0])
        result, suf, pre = [[0] * m for _ in range(n)], 1, 1
        for i in range(n - 1, -1, -1):
            for j in range(m - 1, -1, -1):
                result[i][j] = suf
                suf = suf * grid[i][j] % MOD
        for i, row in enumerate(grid):
            for j, v in enumerate(row):
                result[i][j] = result[i][j] * pre % MOD 
                pre = pre * v % MOD
        return result
```
