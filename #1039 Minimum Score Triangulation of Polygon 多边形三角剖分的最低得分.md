# 1039 Minimum Score Triangulation of Polygon 多边形三角剖分的最低得分

__Description__:
You have a convex n-sided polygon where each vertex has an integer value. You are given an integer array values where values[i] is the value of the ith vertex (i.e., clockwise order).

You will triangulate the polygon into n - 2 triangles. For each triangle, the value of that triangle is the product of the values of its vertices, and the total score of the triangulation is the sum of these values over all n - 2 triangles in the triangulation.

Return the smallest possible total score that you can achieve with some triangulation of the polygon.

__Example:__

Example 1:

![shape 1](https://assets.leetcode.com/uploads/2021/02/25/shape1.jpg)

Input: values = [1,2,3]
Output: 6
Explanation: The polygon is already triangulated, and the score of the only triangle is 6.

Example 2:

![shape 2](https://assets.leetcode.com/uploads/2021/02/25/shape2.jpg)

Input: values = [3,7,4,5]
Output: 144
Explanation: There are two triangulations, with possible scores: 3*7*5 + 4*5*7 = 245, or 3*4*5 + 3*4*7 = 144.
The minimum score is 144.

Example 3:

![shape 3](https://assets.leetcode.com/uploads/2021/02/25/shape3.jpg)

Input: values = [1,3,1,4,1,5]
Output: 13
Explanation: The minimum score triangulation has score 1*1*3 + 1*1*4 + 1*1*5 + 1*1*1 = 13.

__Constraints:__

n == values.length
3 <= n <= 50
1 <= values[i] <= 100

__题目描述__:
你有一个凸的 n 边形，其每个顶点都有一个整数值。给定一个整数数组 values ，其中 values[i] 是第 i 个顶点的值（即 顺时针顺序 ）。

假设将多边形 剖分 为 n - 2 个三角形。对于每个三角形，该三角形的值是顶点标记的乘积，三角剖分的分数是进行三角剖分后所有 n - 2 个三角形的值之和。

返回 多边形进行三角剖分后可以得到的最低分 。

__示例 :__

示例 1：

![多边形 1](https://assets.leetcode.com/uploads/2021/02/25/shape1.jpg)

输入：values = [1,2,3]
输出：6
解释：多边形已经三角化，唯一三角形的分数为 6。

示例 2：

![多边形 2](https://assets.leetcode.com/uploads/2021/02/25/shape2.jpg)

输入：values = [3,7,4,5]
输出：144
解释：有两种三角剖分，可能得分分别为：3*7*5 + 4*5*7 = 245，或 3*4*5 + 3*4*7 = 144。最低分数为 144。
示例 3：

![多边形 3](https://assets.leetcode.com/uploads/2021/02/25/shape3.jpg)

输入：values = [1,3,1,4,1,5]
输出：13
解释：最低分数三角剖分的得分情况为 1*1*3 + 1*1*4 + 1*1*5 + 1*1*1 = 13。

__提示:__

n == values.length
3 <= n <= 50
1 <= values[i] <= 100

__思路__:

动态规划
令 dp[i][j] 表示 values[i:j + 1] 构成的三角形最小得分
则 dp[i][j] = min(dp[i][k] + dp[k][j] + values[i] *values[k]* values[j]), i < k < j
dp[i][j] 初始化为 0, 因为两个邻边是不能划分三角形的
最后返回 dp[0][n - 1] 即可
时间复杂度O(n ^ 3), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minScoreTriangulation(vector<int>& values) 
    {
        int n = values.size();
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 2; i < n; i++) 
        {
            for (int j = 0; j < n - i; j++) 
            {
                dp[j][i + j] = INT_MAX;
                for (int k = j + 1; k < i + j; k++) dp[j][i + j] = min(dp[j][i + j], dp[j][k] + dp[k][i + j] + values[k] * values[j] * values[i + j]);
            }
        }
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minScoreTriangulation(int[] values) {
        int n = values.length, dp[][] = new int[n][n];
        for (int i = 2; i < n; i++) {
            for (int j = 0; j < n - i; j++) {
                dp[j][i + j] = Integer.MAX_VALUE;
                for (int k = j + 1; k < i + j; k++) dp[j][i + j] = Math.min(dp[j][i + j], dp[j][k] + dp[k][i + j] + values[k] * values[j] * values[i + j]);
            }
        }
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def minScoreTriangulation(self, values: List[int]) -> int:
        n = len(values)
        dp = [[0] * n for _ in range(n)]
        for i in range(2, n):
            for j in range(n - i):
                dp[j][i + j] = float('inf')
                for k in range(j + 1, i + j + 1):
                    dp[j][i + j] = min(dp[j][i + j], dp[j][k] + dp[k][i + j] + values[j] * values[k] * values[i + j])
        return dp[0][-1]
```
