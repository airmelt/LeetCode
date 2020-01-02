__Description__:
Given n and m which are the dimensions of a matrix initialized by zeros and given an array indices where indices[i] = [ri, ci]. For each pair of [ri, ci] you have to increment all cells in row ri and column ci by 1.

Return the number of cells with odd values in the matrix after applying the increment to all indices.

__Example:__
Example 1:
![Matrix 1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/06/e1.png)
Input: n = 2, m = 3, indices = [[0,1],[1,1]]
Output: 6
Explanation: Initial matrix = [[0,0,0],[0,0,0]].
After applying first increment it becomes [[1,2,1],[0,1,0]].
The final matrix will be [[1,3,1],[1,3,1]] which contains 6 odd numbers.

Example 2:
![Matrix 2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/06/e2.png)
Input: n = 2, m = 2, indices = [[1,1],[0,0]]
Output: 0
Explanation: Final matrix = [[2,2],[2,2]]. There is no odd number in the final matrix.
 
__Constraints:__

1 <= n <= 50
1 <= m <= 50
1 <= indices.length <= 100
0 <= indices[i][0] < n
0 <= indices[i][1] < m

__题目描述__:
给你一个 n 行 m 列的矩阵，最开始的时候，每个单元格中的值都是 0。

另有一个索引数组 indices，indices[i] = [ri, ci] 中的 ri 和 ci 分别表示指定的行和列（从 0 开始编号）。

你需要将每对 [ri, ci] 指定的行和列上的所有单元格的值加 1。

请你在执行完所有 indices 指定的增量操作后，返回矩阵中 「奇数值单元格」 的数目。

__示例 :__
示例 1：
![矩阵1](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/06/e1.png)
输入：n = 2, m = 3, indices = [[0,1],[1,1]]
输出：6
解释：最开始的矩阵是 [[0,0,0],[0,0,0]]。
第一次增量操作后得到 [[1,2,1],[0,1,0]]。
最后的矩阵是 [[1,3,1],[1,3,1]]，里面有 6 个奇数。

示例 2：
![矩阵2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/06/e2.png)
输入：n = 2, m = 2, indices = [[1,1],[0,0]]
输出：0
解释：最后的矩阵是 [[2,2],[2,2]]，里面没有奇数。
 
__提示：__

1 <= n <= 50
1 <= m <= 50
1 <= indices.length <= 100
0 <= indices[i][0] < n
0 <= indices[i][1] < m

__思路__:
1. 按照题目要求, 设置一个二维数组, 找到里面的奇数输出即可
时间复杂度O(pq), 空间复杂度O(mn), 其中 p为 indices的长度, q为 max(m ,n)
2. 因为每次只用考虑行或者列, 可以只用一维数组记录行和列的变化, 然后最后去掉交叉的部分(不清楚可以画 venn图)
时间复杂度O(p), 空间复杂度O(q), 其中 p为 indices的长度, q为 max(m ,n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int oddCells(int n, int m, vector<vector<int>>& indices) 
    {
        vector<int> row(n), col(m);
        for (auto indice : indices) row[indice[0]] ^= 1, col[indice[1]] ^= 1;
        int r = accumulate(row.begin(), row.end(), 0), c = accumulate(col.begin(), col.end(), 0);
        return r * m + c * n - 2 * r * c;
    }
};
```

__Java__:
```Java
class Solution {
    public int oddCells(int n, int m, int[][] indices) {
        int row[] = new int[n], col[] = new int[m], result = 0;
        for (int[] indice : indices) {
            row[indice[0]] ^= 1; 
            col[indice[1]] ^= 1;
        }
        for (int i = 0; i < n; i++) for (int j = 0; j < m; j++) if (((row[i] + col[j]) & 1) == 1) ++result;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def oddCells(self, n: int, m: int, indices: List[List[int]]) -> int:
        matrix = [[0 for _ in range(m)] for _ in range(n)]
        for i, j in indices:
            for col in range(m):
                matrix[i][col] += 1
            for row in range(n):
                matrix[row][j] += 1
        return sum(i & 1 for row in matrix for i in row)
```