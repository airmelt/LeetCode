# 867 Transpose Matrix 转置矩阵

__Description__:
Given a matrix A, return the transpose of A.

The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.

__Example:__

Example 1:

Input: [[1,2,3],[4,5,6],[7,8,9]]
Output: [[1,4,7],[2,5,8],[3,6,9]]

Example 2:

Input: [[1,2,3],[4,5,6]]
Output: [[1,4],[2,5],[3,6]]

__Note:__

1 <= A.length <= 1000
1 <= A[0].length <= 1000

__题目描述__:
给定一个矩阵 A， 返回 A 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

__示例 :__

示例 1：

输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：[[1,4,7],[2,5,8],[3,6,9]]

示例 2：

输入：[[1,2,3],[4,5,6]]
输出：[[1,4],[2,5],[3,6]]

__提示：__

1 <= A.length <= 1000
1 <= A[0].length <= 1000

__思路__:

复制到新的空间即可
时间复杂度O(nm), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> transpose(vector<vector<int>>& A) 
    {
        vector<vector<int>> result(A[0].size(), vector<int>(A.size(), 0));
        for (int i = 0; i < A[0].size(); i++) for (int j = 0; j < A.size(); j++) result[i][j] = A[j][i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] transpose(int[][] A) {
        int result[][] = new int[A[0].length][A.length];
        for (int i = 0; i < A[0].length; i++) for (int j = 0; j < A.length; j++) result[i][j] = A[j][i];
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def transpose(self, A: List[List[int]]) -> List[List[int]]:
        return [list(i) for i in zip(*A)]
```
