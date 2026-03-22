# 74 Search a 2D Matrix 搜索二维矩阵

__Description__:
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.

__Example:__

Example 1:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true

Example 2:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false

__题目描述__:
编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。

__示例 :__

示例 1:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true

示例 2:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false

__思路__:

可以将二维矩阵看作一行数组
元素用 [index / matrix[0].size()][index % matrix[0].size()]定位
用二分查找即可
时间复杂度O(lg(mn)), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) 
    {
        if (matrix.empty() or matrix[0].empty()) return false;
        int left = 0, right = matrix.size() * matrix[0].size() - 1, n = matrix[0].size();
        while (left <= right)
        {
            int mid = left + ((right - left) >> 1);
            int cur = matrix[mid / n][mid % n];
            if (target == cur) return true;
            else if (target > cur) left = mid + 1;
            else right = mid - 1;
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) return false;
        int left = 0, right = matrix.length * matrix[0].length - 1, n = matrix[0].length;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            int cur = matrix[mid / n][mid % n];
            if (target == cur) return true;
            else if (target > cur) left = mid + 1;
            else right = mid - 1;
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        return target in set(chain(*matrix))
```
