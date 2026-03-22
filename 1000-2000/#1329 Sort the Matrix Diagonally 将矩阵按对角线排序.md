# 1329 Sort the Matrix Diagonally 将矩阵按对角线排序

__Description:__

A matrix diagonal is a diagonal line of cells starting from some cell in either the topmost row or leftmost column and going in the bottom-right direction until reaching the matrix's end. For example, the matrix diagonal starting from mat\[2][0], where mat is a 6 x 3 matrix, includes cells mat\[2][0], mat\[3][1], and mat\[4][2].

Given an m x n matrix mat of integers, sort each matrix diagonal in ascending order and return the resulting matrix.

__Example:__

Example 1:

![Matrix](https://assets.leetcode.com/uploads/2020/01/21/1482_example_1_2.png)

Input: mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
Output: [[1,1,1,1],[1,2,2,2],[1,2,3,3]]

Example 2:

Input: mat = [[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]
Output: [[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]

__Constraints:__

m == mat.length
n == mat\[i].length
1 <= m, n <= 100
1 <= mat\[i][j] <= 100

__题目描述：__

矩阵对角线 是一条从矩阵最上面行或者最左侧列中的某个元素开始的对角线，沿右下方向一直到矩阵末尾的元素。例如，矩阵 mat 有 6 行 3 列，从 mat\[2][0] 开始的 矩阵对角线 将会经过 mat\[2][0]、mat\[3][1] 和 mat\[4][2] 。

给你一个 m * n 的整数矩阵 mat ，请你将同一条 矩阵对角线 上的元素按升序排序后，返回排好序的矩阵。

__示例：__

示例 1：

![矩阵](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/25/1482_example_1_2.png)

输入：mat = [[3,3,1,1],[2,2,1,2],[1,1,1,2]]
输出：[[1,1,1,1],[1,2,2,2],[1,2,3,3]]

示例 2：

输入：mat = [[11,25,66,1,69,7],[23,55,17,45,15,52],[75,31,36,44,58,8],[22,27,33,25,68,4],[84,28,14,11,5,50]]
输出：[[5,17,4,1,52,7],[11,11,25,45,8,69],[14,23,25,44,58,15],[22,27,31,36,50,66],[84,28,75,33,55,68]]

__提示：__

m == mat.length
n == mat\[i].length
1 <= m, n <= 100
1 <= mat\[i][j] <= 100

__思路：__

模拟
按照对角线取出值放入优先队列中, 然后重新放回原矩阵
时间复杂度为 O(mnlgn), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) 
    {
        for (int k = 0, m = mat.size(), n = mat.front().size(), r = min(m, n); k < r; k++) for (int i = 1; i < m; i++) for (int j = 1; j < n; j++) if (mat[i][j] < mat[i - 1][j - 1]) swap(mat[i][j], mat[i - 1][j - 1]);
        return mat;
    }
};
```

__Java__:

```Java
class Solution {
    public int[][] diagonalSort(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        HashMap<Integer, PriorityQueue<Integer>> map = new HashMap<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                map.putIfAbsent(i - j, new PriorityQueue<>());
                map.get(i - j).offer(mat[i][j]);
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) mat[i][j] = map.get(i - j).poll();
        return mat;
    }
}
```

__Python__:

```Python
class Solution:
    def diagonalSort(self, mat: List[List[int]]) -> List[List[int]]:
        m, n, nums = len(mat), len(mat[0]), defaultdict(list)
        for i in range(m):
            for j in range(n):
                heappush(nums[i - j], mat[i][j])
        for i in range(m):
            for j in range(n):
                mat[i][j] = heappop(nums[i - j])
        return mat
```
