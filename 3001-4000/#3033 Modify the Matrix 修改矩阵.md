# 3033 Modify the Matrix 修改矩阵

__Description:__

Given a __0-indexed__ `m x n` integer matrix `matrix`, create a new __0-indexed__ matrix called `answer`. Make `answer` equal to `matrix`, then replace each element with the value `-1` with the __maximum__ element in its respective column.

Return _the matrix_ `answer`.

__Example:__

Example 1:

![3033-1](https://assets.leetcode.com/uploads/2023/12/24/matrix1.png)

```text
Input: matrix = [[1,2,-1],[4,-1,6],[7,8,9]]
Output: [[1,2,9],[4,8,6],[7,8,9]]
Explanation: The diagram above shows the elements that are changed (in blue).
```

- We replace the value in the cell \[1\]\[1\] with the maximum value in the column 1, that is 8.
- We replace the value in the cell \[0\]\[2\] with the maximum value in the column 2, that is 9.

Example 2:

![3033-2](https://assets.leetcode.com/uploads/2023/12/24/matrix2.png)

```text
Input: matrix = [[3,-1],[5,2]]
Output: [[3,2],[5,2]]
Explanation: The diagram above shows the elements that are changed (in blue).
```

__Constraints:__

- `m == matrix.length`
- `n == matrix[i].length`
- `2 <= m, n <= 50`
- `-1 <= matrix[i][j] <= 100`
- The input is generated such that each column contains at least one non-negative integer.

__题目描述:__

给你一个下标从 __0__ 开始、大小为 `m x n` 的整数矩阵 `matrix` ，新建一个下标从 __0__ 开始、名为 `answer` 的矩阵。使 `answer` 与 `matrix` 相等，接着将其中每个值为 `-1` 的元素替换为所在列的 __最大__ 元素。

返回矩阵 `answer` 。

__示例:__

示例 1：

![3033-3](https://assets.leetcode.com/uploads/2023/12/24/matrix1.png)

```text
输入：matrix = [[1,2,-1],[4,-1,6],[7,8,9]]
输出：[[1,2,9],[4,8,6],[7,8,9]]
解释：上图显示了发生替换的元素（蓝色区域）。
```

- 将单元格 \[1\]\[1\] 中的值替换为列 1 中的最大值 8 。
- 将单元格 \[0\]\[2\] 中的值替换为列 2 中的最大值 9 。

示例 2：

![3033-4](https://assets.leetcode.com/uploads/2023/12/24/matrix2.png)

```text
输入：matrix = [[3,-1],[5,2]]
输出：[[3,2],[5,2]]
解释：上图显示了发生替换的元素（蓝色区域）。
```

__提示：__

- `m == matrix.length`
- `n == matrix[i].length`
- `2 <= m, n <= 50`
- `-1 <= matrix[i][j] <= 100`
- 测试用例中生成的输入满足每列至少包含一个非负整数。

__思路:__

```text
1. 暴力法
遍历所有元素
当遇到 -1 时
遍历所有的同列元素取最大值
时间复杂度为 O(MN ^ 2), 空间复杂度为 O(1)
2. 预处理
按照列遍历矩阵
求出每一列对应的最大值
如果当前元素为 -1
更新为最大值
时间复杂度为 O(MN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> modifiedMatrix(vector<vector<int>>& matrix) 
    {
        for (int n = matrix.front().size(), j = 0, mx = -1; j < n; j++, mx = -1) 
        {
            for (const auto& row : matrix) mx = max(row[j], mx);
            for (auto& row : matrix) if (row[j] == -1) row[j] = mx;
        }
        return matrix;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] modifiedMatrix(int[][] matrix) {
        for (int m = matrix.length, n = matrix[0].length, i = 0; i < m; i++) for (int j = 0; j < n; j++) if (matrix[i][j] == -1) for (int k = 0; k < m; k++) matrix[i][j] = Math.max(matrix[i][j], matrix[k][j]);
        return matrix;
    }
}
```

__Python__:

```Python
class Solution:
    def modifiedMatrix(self, matrix: List[List[int]]) -> List[List[int]]:
        return [original[x * n:x * n + n] for x in range(m)] if (original := [matrix[i][j] if matrix[i][j] != -1 else max(list(zip(*matrix))[j]) for i in range(len(matrix)) for j in range(len(matrix[0]))]) and (m := len(matrix)) and (n := len(matrix[0])) else []
```
