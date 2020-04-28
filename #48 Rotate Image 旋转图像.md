__Description__:
You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

__Note:__

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

__Example:__
Example 1:

Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

Example 2:

Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

__题目描述__:
给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

__说明:__

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

__示例 :__
示例 1:

给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

示例 2:

给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

__思路__:
1. 先转置矩阵, 再反转每一行
时间复杂度O(n ^ 2), 空间复杂度O(1)
2. 将最外层的 4个角矩形交换元素
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void rotate(vector<vector<int>>& matrix) 
    {
        int n = matrix.size();
        for (int i = 0; i < (n + 1) / 2; i ++) for (int j = 0; j < n / 2; j++)
        {
                int temp = matrix[n - 1 - j][i];
                matrix[n - 1 - j][i] = matrix[n - 1 - i][n - j - 1];
                matrix[n - 1 - i][n - j - 1] = matrix[j][n - 1 -i];
                matrix[j][n - 1 - i] = matrix[i][j];
                matrix[i][j] = temp;
        }
    }
};
```

__Java__:
```Java
class Solution {
    public void rotate(int[][] matrix) {
        for (int i = 0; i < matrix.length; i++) for (int j = i; j < matrix.length; j++) if (i != j) swap(i, j, j, i, matrix);
        for (int i = 0; i < matrix.length; i++) for (int j = 0; j < matrix.length / 2; j++) swap(i, j, i, matrix.length - j - 1, matrix);
    }
    
    private void swap(int i, int j, int k, int l, int[][] matrix) {
        matrix[i][j] ^= matrix[k][l];
        matrix[k][l] ^= matrix[i][j];
        matrix[i][j] ^= matrix[k][l];
    }
}
```

__Python__:
```Python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        matrix[:] = map(list, zip(*matrix[::-1]))
```