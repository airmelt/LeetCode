# 1292 Maximum Side Length of a Square with Sum Less than or Equal to Threshold 元素和小于等于阈值的正方形的最大边长

__Description:__

Given a m x n matrix mat and an integer threshold, return the maximum side-length of a square with a sum less than or equal to threshold or return 0 if there is no such square.

__Example:__

Example 1:

![Mat](https://assets.leetcode.com/uploads/2019/12/05/e1.png)

Input: mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
Output: 2
Explanation: The maximum side length of square with sum less than 4 is 2 as shown.

Example 2:

Input: mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
Output: 0

__Constraints:__

m == mat.length
n == mat[i].length
1 <= m, n <= 300
0 <= mat\[i][j] <= 10^4
0 <= threshold <= 10^5

__题目描述：__

给你一个大小为 m x n 的矩阵 mat 和一个整数阈值 threshold。

请你返回元素总和小于或等于阈值的正方形区域的最大边长；如果没有这样的正方形区域，则返回 0 。

__示例：__

示例 1：

![矩阵](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/15/e1.png)

输入：mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
输出：2
解释：总和小于或等于 4 的正方形的最大边长为 2，如图所示。

示例 2：

输入：mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
输出：0

__提示：__

m == mat.length
n == mat[i].length
1 <= m, n <= 300
0 <= mat\[i][j] <= 10^4
0 <= threshold <= 10^5

__思路：__

前缀和
用 pre 数组记录下二位前缀和
从结果开始到边长较小的边为止搜索前缀和是否小于阈值
如果大于直接剪枝, 可以加快执行速度
时间复杂度为 O(mn \* min(m, n)), 空间复杂度为 O(mn)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxSideLength(vector<vector<int>>& mat, int threshold) 
    {
        int m = mat.size(), n = mat.front().size(), result = 0;
        vector<vector<int>> pre(m + 1, vector<int>(n + 1));
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) pre[i][j] = mat[i - 1][j - 1] + pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1];
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) for (int l = result; i > l and j > l; l++) if (pre[i][j] - pre[i - l - 1][j] - pre[i][j - 1 - l] + pre[i - l - 1][j - l - 1] <= threshold) result = max(result, l + 1); else break;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {
        int m = mat.length, n = mat[0].length, pre[][] = new int[m + 1][n + 1], result = 0;
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) pre[i][j] = mat[i - 1][j - 1] + pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1];
        for (int i = 1; i <= m; i++) for (int j = 1; j <= n; j++) for (int l = result; i > l && j > l; l++) if (pre[i][j] - pre[i - l - 1][j] - pre[i][j - 1 - l] + pre[i - l - 1][j - l - 1] <= threshold) result = Math.max(result, l + 1); else break;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSideLength(self, mat: List[List[int]], threshold: int) -> int:
        m, n, result = len(mat), len(mat[0]), 0
        pre = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                pre[i][j] = mat[i - 1][j - 1] + pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1]
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                for l in range(result, min(i, j)):
                    if pre[i][j] - pre[i - 1 - l][j] - pre[i][j - 1 - l] + pre[i - 1 - l][j - 1 - l] <= threshold:
                        result = max(result, l + 1)
                    else:
                        break
        return result
```
