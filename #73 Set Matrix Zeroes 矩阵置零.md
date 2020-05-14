__Description__:
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.

__Example:__
Example 1:

Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]

Example 2:

Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]

__Follow up:__

A straight forward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?

__题目描述__:
给定一个 m x n 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用原地算法。

__示例 :__
示例 1:

输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]

示例 2:

输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]

__进阶:__

一个直接的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个常数空间的解决方案吗？

__思路__:
将矩阵的首行和首列用来记录该行/列是否需要全部置 0
需要额外的两个 bool变量记录首行和首列是否需要置 0
1. 遍历首行/列, 记录是否需要置 0
2. 遍历矩阵, 如果需要置 0, 将首行/列第一个元素置 0
3. 遍历矩阵, 将需要置 0的行/列置 0
4. 将首行/列置 0
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void setZeroes(vector<vector<int>>& matrix) 
    {
        bool row = false, col = false;
        for (int i = 0; i < matrix.size(); i++) 
        {
            if (matrix[i][0] == 0) 
            {
                col = true;
                break;
            }
        }
        for (int i = 0; i < matrix[0].size(); i++) 
        {
            if (matrix[0][i] == 0) 
            {
                row = true;
                break;
            }
        }
        for (int i = 0; i < matrix.size(); i++) 
        {
            for (int j = 0; j < matrix[0].size(); j++) 
            {
                if (matrix[i][j] == 0) 
                {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < matrix.size(); i++) for (int j = 1; j < matrix[0].size(); j++) if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
        if (row) for (int i = 0; i < matrix[0].size(); i++) matrix[0][i] = 0;
        if (col) for (int i = 0; i < matrix.size(); i++) matrix[i][0] = 0;
    }
};
```

__Java__:
```Java
class Solution {
    public void setZeroes(int[][] matrix) {
        boolean row = false, col = false;
        for (int i = 0; i < matrix.length; i++) {
            if (matrix[i][0] == 0) {
                col = true;
                break;
            }
        }
        for (int i = 0; i < matrix[0].length; i++) {
            if (matrix[0][i] == 0) {
                row = true;
                break;
            }
        }
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < matrix.length; i++) for (int j = 1; j < matrix[0].length; j++) if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
        if (row) for (int i = 0; i < matrix[0].length; i++) matrix[0][i] = 0;
        if (col) for (int i = 0; i < matrix.length; i++) matrix[i][0] = 0;
    }
}
```

__Python__:
```Python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        row, col = False, False
        for i in range(len(matrix)):
            if not matrix[i][0]:
                col = True
                break
        for i in range(len(matrix[0])):
            if not matrix[0][i]:
                row = True
                break
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if not matrix[i][j]:
                    matrix[i][0] = 0
                    matrix[0][j] = 0
        for i in range(1, len(matrix)):
            for j in range(1, len(matrix[0])):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0
        if row:
            for i in range(len(matrix[0])):
                matrix[0][i] = 0
        if col:
            for i in range(len(matrix)):
                matrix[i][0] = 0
```