# 766 Toeplitz Matrix 托普利茨矩阵

__Description__:
A matrix is Toeplitz if every diagonal from top-left to bottom-right has the same element.

Now given an M x N matrix, return True if and only if the matrix is Toeplitz.

__Example:__

Example 1:

Input:

```text
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
```

Output: True
Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.

Example 2:

Input:

```text
matrix = [
  [1,2],
  [2,2]
]
```

Output: False
Explanation:
The diagonal "[1, 2]" has different elements.

__Note:__

matrix will be a 2D array of integers.
matrix will have a number of rows and columns in range [1, 20].
matrix[i][j] will be integers in range [0, 99].

__Follow up:__

What if the matrix is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
What if the matrix is so large that you can only load up a partial row into the memory at once?

__题目描述__:
如果一个矩阵的每一方向由左上到右下的对角线上具有相同元素，那么这个矩阵是托普利茨矩阵。

给定一个 M x N 的矩阵，当且仅当它是托普利茨矩阵时返回 True。

__示例 :__

示例 1:

输入:

```text
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
```

输出: True
解释:
在上述矩阵中, 其对角线为:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]"。
各条对角线上的所有元素均相同, 因此答案是True。

示例 2:

输入:

```text
matrix = [
  [1,2],
  [2,2]
]
```

输出: False
解释:
对角线"[1, 2]"上的元素不同。

__说明:__

 matrix 是一个包含整数的二维数组。
matrix 的行数和列数均在 [1, 20]范围内。
matrix[i][j] 包含的整数在 [0, 99]范围内。

__进阶:__

如果矩阵存储在磁盘上，并且磁盘内存是有限的，因此一次最多只能将一行矩阵加载到内存中，该怎么办？
如果矩阵太大以至于只能一次将部分行加载到内存中，该怎么办？

__思路__:

题意是沿着对角线扫描, 得到的数组里面应该都是相同的元素

1. 只要比较 matrix[i][j] != matrix[i + 1][j + 1], 利用传递性, 即可
2. 比较每一行, 上一行去掉最后一个元素和下一行去掉第一个元素, 如果不相等就返回 false
时间复杂度O(n ^ 2), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) 
    {
        for (int i = 0; i < matrix.size() - 1; i++) for (int j = 0; j < matrix[0].size() - 1; j++) if (matrix[i][j] != matrix[i + 1][j + 1]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        for (int i = 0; i < matrix.length - 1; i++) for (int j = 0; j < matrix[0].length - 1; j++) if (matrix[i][j] != matrix[i + 1][j + 1]) return false;
        return true; 
    }
}
```

__Python__:

```Python
class Solution:
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        return all([matrix[i][:-1] == matrix[i + 1][1:] for i in range(len(matrix) - 1)])
```
