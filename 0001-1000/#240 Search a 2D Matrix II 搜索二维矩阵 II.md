# 240 Search a 2D Matrix II 搜索二维矩阵 II

__Description__:
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.

__Example:__

Consider the following matrix:

```text
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = 5, return true.

Given target = 20, return false.

__题目描述__:
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

__示例 :__

现有矩阵 matrix 如下：

```text
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = 5，返回 true。

给定 target = 20，返回 false。

__思路__:

1. 展开成一维数组, 用二分法查找
时间复杂度O(lgmn), 空间复杂度O(mn)
2. 每一行使用二分查找
时间复杂度O(mlgn), 空间复杂度O(1)
3. 从左下角(或者右上角)开始查找
如果大, 指针向上移动;
如果小, 指针向右移动;
如果相等, 输出 true
最后找不到输出 false
时间复杂度O(m + n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) 
    {
        int row = matrix.size() - 1, col = 0;
        while (row > -1 and col < matrix[0].size())
        {
            if (matrix[row][col] < target) ++col;
            else if (matrix[row][col] > target) --row;
            else if (matrix[row][col] == target) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length - 1, col = 0;
        while (row >= 0 && col < matrix[0].length) {
            if (matrix[row][col] > target) row--;
            else if (matrix[row][col] < target) col++;
            else if (matrix[row][col] == target) return true;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        return target in itertools.chain.from_iterable(matrix)
```
