__Description__:
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

__Example:__
Example 1:

Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]

Example 2:

Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]

__题目描述__:
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

__示例 :__
示例 1:

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]

示例 2:

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

__思路__:
按照螺旋的方法遍历整个矩阵
每一次遍历就更换方向
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) 
    {
        vector<int> result;
        if (matrix.empty() || matrix[0].empty()) return result;
        int up = 0, down = matrix.size() - 1, left = 0, right = matrix[0].size() - 1;
        while (true)
        {
            for (int i = left; i <= right; i++) result.push_back(matrix[up][i]);
            if (++up > down) break;
            for (int i = up; i <= down; i++) result.push_back(matrix[i][right]);
            if (--right < left) break;
            for (int i = right; i >= left; i--) result.push_back(matrix[down][i]);
            if (--down < up) break;
            for (int i = down; i >= up; i--) result.push_back(matrix[i][left]);
            if (++left > right) break;
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return new LinkedList<>();
        List<Integer> result = new LinkedList<>();
        int up = 0, down = matrix.length - 1, left = 0, right = matrix[0].length - 1;
        while (true) {
            for (int i = left; i <= right; i++) result.add(matrix[up][i]);
            if (++up > down) break;
            for (int i = up; i <= down; i++) result.add(matrix[i][right]);
            if (--right < left) break;
            for (int i = right; i >= left; i--) result.add(matrix[down][i]);
            if (--down < up) break;
            for (int i = down; i >= up; i--) result.add(matrix[i][left]);
            if (++left > right) break;
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]:
            return []
        result, up, down, left, right = [], 0, len(matrix) - 1, 0, len(matrix[0]) - 1
        while True:
            for i in range(left, right + 1):
                result.append(matrix[up][i]);
            up += 1
            if up > down:
                break
            for i in range(up, down + 1):
                result.append(matrix[i][right]);
            right -= 1
            if left > right:
                break
            for i in range(right, left - 1, - 1):
                result.append(matrix[down][i]);
            down -= 1
            if up > down:
                break
            for i in range(down, up - 1, -1):
                result.append(matrix[i][left]);
            left += 1
            if left > right:
                break
        return result
```