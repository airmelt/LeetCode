# 1074 Number of Submatrices That Sum to Target 元素和为目标值的子矩阵数量

__Description__:
Given a matrix and a target, return the number of non-empty submatrices that sum to target.

A submatrix x1, y1, x2, y2 is the set of all cells matrix[x][y] with x1 <= x <= x2 and y1 <= y <= y2.

Two submatrices (x1, y1, x2, y2) and (x1', y1', x2', y2') are different if they have some coordinate that is different: for example, if x1 != x1'.

__Example:__

Example 1:

![matrix](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

Input: matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
Output: 4
Explanation: The four 1x1 submatrices that only contain 0.

Example 2:

Input: matrix = [[1,-1],[-1,1]], target = 0
Output: 5
Explanation: The two 1x2 submatrices, plus the two 2x1 submatrices, plus the 2x2 submatrix.

Example 3:

Input: matrix = [[904]], target = 0
Output: 0

__Constraints:__

1 <= matrix.length <= 100
1 <= matrix[0].length <= 100
-1000 <= matrix[i] <= 1000
-10^8 <= target <= 10^8

__题目描述__:
给出矩阵 matrix 和目标值 target，返回元素总和等于目标值的非空子矩阵的数量。

子矩阵 x1, y1, x2, y2 是满足 x1 <= x <= x2 且 y1 <= y <= y2 的所有单元 matrix[x][y] 的集合。

如果 (x1, y1, x2, y2) 和 (x1', y1', x2', y2') 两个子矩阵中部分坐标不同（如：x1 != x1'），那么这两个子矩阵也不同。

__示例 :__

示例 1：

![矩阵](https://assets.leetcode.com/uploads/2020/09/02/mate1.jpg)

输入：matrix = [[0,1,0],[1,1,1],[0,1,0]], target = 0
输出：4
解释：四个只含 0 的 1x1 子矩阵。

示例 2：

输入：matrix = [[1,-1],[-1,1]], target = 0
输出：5
解释：两个 1x2 子矩阵，加上两个 2x1 子矩阵，再加上一个 2x2 子矩阵。

示例 3：

输入：matrix = [[904]], target = 0
输出：0

__提示:__

1 <= matrix.length <= 100
1 <= matrix[0].length <= 100
-1000 <= matrix[i] <= 1000
-10^8 <= target <= 10^8

__思路__:

1. 前缀和
预处理二维数组前缀和
pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] - pre[i][j] + matrix[i][j]
那么任意一个区间[k, l], [i, j]的前缀和可由
pre[i][j] - pre[i][l - 1] - pre[k - 1][j] + pre[k - 1][l - 1]
得到, 其中[k, l] 表示矩形的左上角的点的坐标, [i, j] 表示矩形的右下角的点的坐标
时间复杂度O(m ^ 2 * n ^ 2), 空间复杂度O(mn)
2. 前缀和 ➕ 哈希
预处理二维数组前缀和
固定上下边界和右边界
整个上下边界为 cur = pre[buttom][right] - pre[top - 1][right], 如果等于 target, 结果加 1
否则在哈希表中查找 cur - target, 结果加上该值
哈希表存入 cur, 表示前缀和出现的次数
时间复杂度O(mn ^ 2), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSubmatrixSumTarget(vector<vector<int>>& matrix, int target) 
    {
        int m = matrix.size(), n = matrix.front().size(), result = 0;
        vector<vector<int>> pre(m + 1, vector<int>(n + 1));
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] - pre[i][j] + matrix[i][j];
        for (int i = 1; i <= m; i++)
        {
            for (int j = i; j <= m; j++)
            {
                int cur = 0;
                unordered_map<int, int> m;
                for (int k = 1; k <= n; k++)
                {
                    cur = pre[j][k] - pre[i - 1][k];
                    result += (cur == target) + m[cur - target];
                    ++m[cur];
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubmatrixSumTarget(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length, result = 0, pre[][] = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) pre[i + 1][j + 1] = pre[i][j + 1] + pre[i + 1][j] - pre[i][j] + matrix[i][j];
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) for (int k = 1; k <= i; k++) for (int l = 1; l <= j; l++) result += (pre[i][j] - pre[i][l - 1] - pre[k - 1][j] + pre[k - 1][l - 1] == target ? 1 : 0);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubmatrixSumTarget(self, matrix: List[List[int]], target: int) -> int:
        m, n, pre, result = len(matrix), len(matrix[0]), [list(accumulate(i)) for i in matrix], 0
        for i in range(1, m):
            for j in range(n):
                pre[i][j] += pre[i - 1][j]
        for top in range(m):
            for buttom in range(top, m):
                d = defaultdict(int)
                for right in range(n):
                    result += ((cur := pre[buttom][right] - (pre[top - 1][right] if top else 0)) == target) + d[cur - target]
                    d[cur] += 1
        return result
```
