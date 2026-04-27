# 2133 Check if Every Row and Column Contains All Numbers 检查是否每一行每一列都包含全部整数

__Description:__

An `n x n` matrix is __valid__ if every row and every column contains __all__ the integers from `1` to `n` (__inclusive__).

Given an `n x n` integer matrix `matrix`, return `true` _if the matrix is __valid__._ Otherwise, return `false`.

__Example:__

Example 1:

![2133-1](https://assets.leetcode.com/uploads/2021/12/21/example1drawio.png)

```text
Input: matrix = [[1,2,3],[3,1,2],[2,3,1]]
Output: true
Explanation: In this case, n = 3, and every row and column contains the numbers 1, 2, and 3.
Hence, we return true.
```

Example 2:

![2133-2](https://assets.leetcode.com/uploads/2021/12/21/example2drawio.png)

```text
Input: matrix = [[1,1,1],[1,2,3],[1,2,3]]
Output: false
Explanation: In this case, n = 3, but the first row and the first column do not contain the numbers 2 or 3.
Hence, we return false.
```

__Constraints:__

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 100`
- `1 <= matrix[i][j] <= n`

__题目描述:__

对一个大小为 `n x n` 的矩阵而言，如果其每一行和每一列都包含从 `1` 到 `n` 的 __全部__ 整数（含 `1` 和 `n`），则认为该矩阵是一个 __有效__ 矩阵。

给你一个大小为 `n x n` 的整数矩阵 `matrix` ，请你判断矩阵是否为一个有效矩阵:如果是，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

![2133-3](https://assets.leetcode.com/uploads/2021/12/21/example1drawio.png)

```text
输入：matrix = [[1,2,3],[3,1,2],[2,3,1]]
输出：true
解释：在此例中，n = 3 ，每一行和每一列都包含数字 1、2、3 。
因此，返回 true 。
```

示例 2：

![2133-4](https://assets.leetcode.com/uploads/2021/12/21/example2drawio.png)

```text
输入：matrix = [[1,1,1],[1,2,3],[1,2,3]]
输出：false
解释：在此例中，n = 3 ，但第一行和第一列不包含数字 2 和 3 。
因此，返回 false 。
```

__提示：__

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 100`
- `1 <= matrix[i][j] <= n`

__思路:__

```text
模拟
按照行和列将同一行或者同一列的加入哈希表
如果一行或者一列的长度不等于 n, 则返回 false
或者判断是否出现过提前剪枝
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkValid(vector<vector<int>>& matrix) 
    {
        int n = matrix.size();
        unordered_set<int> cur;
        for (int i = 0; i < n; i++) 
        {
            cur.clear();
            for (int j = 0; j < n; j++) cur.insert(matrix[i][j]);
            if (cur.size() != n) return false;
        }
        for (int j = 0; j < n; j++) 
        {
            cur.clear();
            for (int i = 0; i < n; i++) cur.insert(matrix[i][j]);
            if (cur.size() != n) return false;
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkValid(int[][] matrix) {
        int n = matrix.length;
        Set<Integer> row = new HashSet<>(), col = new HashSet<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                row.add(matrix[i][j]);
                col.add(matrix[j][i]);
            }
            if (row.size() != n || col.size() != n) return false;
            row.clear();
            col.clear();
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def checkValid(self, matrix: List[List[int]]) -> bool:
        return all(len(set(row)) == len(matrix) for row in matrix) and all(len(set(col)) == len(matrix) for col in zip(*matrix))
```
