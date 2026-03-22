# 2536 Increment Submatrices by One 子矩阵元素加 1

__Description:__

You are given a positive integer `n`, indicating that we initially have an `n x n` __0-indexed__ integer matrix `mat` filled with zeroes.

You are also given a 2D integer array `query`. For each `query[i] = [row1i, col1i, row2i, col2i]`, you should do the following operation:

- Add `1` to __every element__ in the submatrix with the __top left__ corner `(row1i, col1i)` and the __bottom right__ corner `(row2i, col2i)`. That is, add `1` to `mat[x][y]` for all `row1i <= x <= row2i` and `col1i <= y <= col2i`.

Return _the matrix_ `mat` _after performing every query._

__Example:__

Example 1:

![2536-1](https://assets.leetcode.com/uploads/2022/11/24/p2example11.png)

```text
Input: n = 3, queries = [[1,1,2,2],[0,0,1,1]]
Output: [[1,1,0],[1,2,1],[0,1,1]]
Explanation: The diagram above shows the initial matrix, the matrix after the first query, and the matrix after the second query.
```

- In the first query, we add 1 to every element in the submatrix with the top left corner (1, 1) and bottom right corner (2, 2).
- In the second query, we add 1 to every element in the submatrix with the top left corner (0, 0) and bottom right corner (1, 1).

Example 2:

![2536-2](https://assets.leetcode.com/uploads/2022/11/24/p2example22.png)

```text
Input: n = 2, queries = [[0,0,1,1]]
Output: [[1,1],[1,1]]
Explanation: The diagram above shows the initial matrix and the matrix after the first query.
- In the first query we add 1 to every element in the matrix.
```

__Constraints:__

- `1 <= n <= 500`
- `1 <= queries.length <= 10 ^ 4`
- `0 <= row1i <= row2i < n`
- `0 <= col1i <= col2i < n`

__题目描述:__

给你一个正整数 `n` ，表示最初有一个 `n x n` 、下标从 __0__ 开始的整数矩阵 `mat` ，矩阵中填满了 0 。

另给你一个二维整数数组 `query` 。针对每个查询 `query[i] = [row1i, col1i, row2i, col2i]` ，请你执行下述操作:

- 找出 __左上角__ 为 `(row1i, col1i)` 且 __右下角__ 为 `(row2i, col2i)` 的子矩阵，将子矩阵中的 __每个元素__ 加 `1` 。也就是给所有满足 `row1i <= x <= row2i` 和 `col1i <= y <= col2i` 的 `mat[x][y]` 加 `1` 。

返回执行完所有操作后得到的矩阵 `mat` 。

__示例:__

示例 1：

![2536-3](https://assets.leetcode.com/uploads/2022/11/24/p2example11.png)

```text
输入：n = 3, queries = [[1,1,2,2],[0,0,1,1]]
输出：[[1,1,0],[1,2,1],[0,1,1]]
解释：上图所展示的分别是：初始矩阵、执行完第一个操作后的矩阵、执行完第二个操作后的矩阵。
```

- 第一个操作：将左上角为 (1, 1) 且右下角为 (2, 2) 的子矩阵中的每个元素加 1 。 
- 第二个操作：将左上角为 (0, 0) 且右下角为 (1, 1) 的子矩阵中的每个元素加 1 。

示例 2：

![2536-4](https://assets.leetcode.com/uploads/2022/11/24/p2example22.png)

```text
输入：n = 2, queries = [[0,0,1,1]]
输出：[[1,1],[1,1]]
解释：上图所展示的分别是：初始矩阵、执行完第一个操作后的矩阵。 
- 第一个操作：将矩阵中的每个元素加 1 。
```

__提示：__

- `1 <= n <= 500`
- `1 <= queries.length <= 10 ^ 4`
- `0 <= row1i <= row2i < n`
- `0 <= col1i <= col2i < n`

__思路:__

```text
差分数组 ➕ 前缀和
使用 diff 数组记录差分值
将 diff[x1 + 1][y1 + 1] 加 1
将 diff[x1 + 1][y2 + 2] 减 1
将 diff[x2 + 2][y1 + 1] 减 1
将 diff[x2 + 2][y2 + 2] 加 1
这样就可以记录下所有的 query
然后对 diff 数组进行前缀和计算
diff[i][j] = diff[i][j - 1] + diff[i - 1][j] - diff[i - 1][j - 1]
最后将 diff 数组的值赋值给 result 数组
只需要取出 diff 数组的 1 ~ n 行和 1 ~ n 列
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> rangeAddQueries(int n, vector<vector<int>>& queries) 
    {
        vector<vector<int>> diff(n + 2, vector<int>(n + 2)), result(n, vector<int>(n));
        for (const auto& query : queries) 
        {
            int x1 = query[0], y1 = query[1], x2 = query[2], y2 = query[3];
            ++diff[x1 + 1][y1 + 1];
            --diff[x1 + 1][y2 + 2];
            --diff[x2 + 2][y1 + 1];
            ++diff[x2 + 2][y2 + 2];
        }
        for (int i = 1; i <= n; i++) for (int j = 1; j <= n; j++) diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
        for (int i = 1; i <= n; i++) for (int j = 1; j <= n; j++) result[i - 1][j - 1] = diff[i][j];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] rangeAddQueries(int n, int[][] queries) {
        int[][] diff = new int[n + 2][n + 2], result = new int[n][n];
        for (var query : queries) {
            int x1 = query[0], y1 = query[1], x2 = query[2], y2 = query[3];
            ++diff[x1 + 1][y1 + 1];
            --diff[x1 + 1][y2 + 2];
            --diff[x2 + 2][y1 + 1];
            ++diff[x2 + 2][y2 + 2];
        }
        for (int i = 1; i <= n; i++) for (int j = 1; j <= n; j++) diff[i][j] += diff[i - 1][j] + diff[i][j - 1] - diff[i - 1][j - 1];
        for (int i = 1; i <= n; i++) for (int j = 1; j <= n; j++) result[i - 1][j - 1] = diff[i][j];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def rangeAddQueries(self, n: int, queries: List[List[int]]) -> List[List[int]]:
        diff = [[0] * (n + 2) for _ in range(n + 2)]
        for x1, y1, x2, y2 in queries:
            diff[x1 + 1][y1 + 1] += 1
            diff[x1 + 1][y2 + 2] -= 1
            diff[x2 + 2][y1 + 1] -= 1
            diff[x2 + 2][y2 + 2] += 1
        for i in range(1, n + 1):
            for j in range(1, n + 1):
                diff[i][j] += diff[i][j - 1] + diff[i - 1][j] - diff[i - 1][j - 1]
        return [row[1:-1] for i, row in enumerate(diff[1:-1])]
```
