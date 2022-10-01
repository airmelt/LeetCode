# 1314 Matrix Block Sum 矩阵区域和

__Description:__

Given a m x n matrix mat and an integer k, return a matrix answer where each answer\[i][j] is the sum of all elements mat\[r][c] for:

i - k <= r <= i + k,
j - k <= c <= j + k, and
(r, c) is a valid position in the matrix.

__Example:__

Example 1:

Input: mat = [[1,2,3],[4,5,6],[7,8,9]], k = 1
Output: [[12,21,16],[27,45,33],[24,39,28]]

Example 2:

Input: mat = [[1,2,3],[4,5,6],[7,8,9]], k = 2
Output: [[45,45,45],[45,45,45],[45,45,45]]

__Constraints:__

m == mat.length
n == mat\[i].length
1 <= m, n, k <= 100
1 <= mat\[i][j] <= 100

__题目描述：__

给你一个 m x n 的矩阵 mat 和一个整数 k ，请你返回一个矩阵 answer ，其中每个 answer\[i][j] 是所有满足下述条件的元素 mat\[r][c] 的和：

i - k <= r <= i + k,
j - k <= c <= j + k 且
(r, c) 在矩阵内。

__示例：__

示例 1：

输入：mat = [[1,2,3],[4,5,6],[7,8,9]], k = 1
输出：[[12,21,16],[27,45,33],[24,39,28]]

示例 2：

输入：mat = [[1,2,3],[4,5,6],[7,8,9]], k = 2
输出：[[45,45,45],[45,45,45],[45,45,45]]

__提示：__

m == mat.length
n == mat\[i].length
1 <= m, n, k <= 100
1 <= mat\[i][j] <= 100

__思路：__

前缀和
二位前缀和 pre\[i + 1][j + 1] = pre\[i][j + 1] + pre\[i + 1][j] - pre\[i][j] + mat\[i][j];
然后直接在原地求 mat 的前缀和, 只需要求到边界位置即可
mat\[i][j] = pre\[min(i + k + 1, m)][min(j + k + 1, n)] - pre\[max(min(i - k, m), 0)][min(j + k + 1, n)] - pre\[min(i + k + 1, m)][max(min(j - k, n), 0)] + pre\[max(min(i - k, m), 0)][max(min(j - k, n), 0)]
时间复杂度为 O(mn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> matrixBlockSum(vector<vector<int>>& mat, int k) 
    {
        int m = mat.size(), n = mat.front().size();
        vector<vector<int>> pre(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] - pre[i][j] + mat[i][j];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) mat[i][j] = pre[min(i + k + 1, m)][min(j + k + 1, n)] - pre[max(min(i - k, m), 0)][min(j + k + 1, n)] - pre[min(i + k + 1, m)][max(min(j - k, n), 0)] + pre[max(min(i - k, m), 0)][max(min(j - k, n), 0)];
        return mat;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] matrixBlockSum(int[][] mat, int k) {
        int m = mat.length, n = mat[0].length, pre[][] = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] - pre[i][j] + mat[i][j];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) mat[i][j] = pre[Math.min(i + k + 1, m)][Math.min(j + k + 1, n)] - pre[Math.max(Math.min(i - k, m), 0)][Math.min(j + k + 1, n)] - pre[Math.min(i + k + 1, m)][Math.max(Math.min(j - k, n), 0)] + pre[Math.max(Math.min(i - k, m), 0)][Math.max(Math.min(j - k, n), 0)];
        return mat;
    }   
}
```

__Python__:

```Python
class Solution:
    def matrixBlockSum(self, mat: List[List[int]], k: int) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        pre = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + mat[i - 1][j - 1]
        for i in range(m):
            for j in range(n):
                mat[i][j] = pre[min(i + k + 1, m)][min(j + k + 1, n)] - pre[max(min(i - k, m), 0)][min(j + k + 1, n)] - pre[min(i + k + 1, m)][max(min(j - k, n), 0)] + pre[max(min(i - k, m), 0)][max(min(j - k, n), 0)]
        return mat
```
