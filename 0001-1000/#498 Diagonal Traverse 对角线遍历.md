# 498 Diagonal Traverse 对角线遍历

__Description__:
Given a matrix of M x N elements (M rows, N columns), return all elements of the matrix in diagonal order as shown in the below image.

__Example:__

Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

Output:  [1,2,4,7,5,3,6,8,9]

Explanation:

![diagonal_traverse](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

__Note:__

The total number of elements of the given matrix will not exceed 10,000.

__题目描述__:
给定一个含有 M x N 个元素的矩阵（M 行，N 列），请以对角线遍历的顺序返回这个矩阵中的所有元素，对角线遍历如下图所示。

__示例 :__

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

输出:  [1,2,4,7,5,3,6,8,9]

解释:

![对角线遍历](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

__说明:__

给定矩阵中的元素总数不会超过 100000 。

__思路__:

观察每次遍历的路径:

1. i + j = 0, result[0] = matrix[0][0]
2. i + j = 1, result[1] = matrix[1][0], result[2] = matrix[0][1]

设 x为遍历的次数 i + j = x, 分为奇偶讨论
每次遍历的范围为 range(max(0, x + 1 - n), min(x + 1, m))
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& matrix) 
    {
        int m = matrix.size(), n = matrix.empty() ? 0 : matrix.front().size(), i = 0;
        vector<int> result(m * n);
        for (int x = 0; x < m + n - 1; x++) 
        {
            if (x & 1) for (int j = max(0, x + 1 - n); j < min(x + 1, m); j++) result[i++] = matrix[j][x - j];
            else for (int j = min(x, m - 1); j > max(-1, x - n); j--) result[i++] = matrix[j][x - j];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        int m = matrix.length, n = matrix.length == 0 ? 0 : matrix[0].length, result[] = new int[matrix.length * (matrix.length == 0 ? 0 : matrix[0].length)], i = 0;
        for (int x = 0; x < m + n - 1; x++) {
            if ((x & 1) == 0) for (int j = Math.min(x + 1, m) - 1; j >= Math.max(0, x + 1 - n); j--) result[i++] = matrix[j][x - j];
            else for (int j = Math.max(0, x + 1 - n); j < Math.min(x + 1, m); j++) result[i++] = matrix[j][x - j];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findDiagonalOrder(self, matrix: List[List[int]]) -> List[int]:
        m, n, result = len(matrix), len(matrix) and len(matrix[0]), []
        for x in range(m + n - 1):
            line = [matrix[i][x - i] for i in range(max(0, x + 1 - n), min(x + 1, m))]
            result += line if x & 1 else line[::-1]
        return result
```
