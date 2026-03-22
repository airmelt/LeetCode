# 265 Paint House II 粉刷房子 II

__Description:__

There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by an n x k cost matrix costs.

For example, costs\[0][0] is the cost of painting house 0 with color 0; costs\[1][2] is the cost of painting house 1 with color 2, and so on...
Return the minimum cost to paint all houses.

__Example:__

Example 1:

Input: costs = [[1,5,3],[2,9,4]]
Output: 5
Explanation:
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5;
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.

Example 2:

Input: costs = [[1,3],[2,4]]
Output: 5

__Constraints:__

costs.length == n
costs[i].length == k
1 <= n <= 100
2 <= k <= 20
1 <= costs\[i][j] <= 20

__题目描述：__

假如有一排房子共有 n 幢，每个房子可以被粉刷成 k 种颜色中的一种。房子粉刷成不同颜色的花费成本也是不同的。你需要粉刷所有的房子并且使其相邻的两个房子颜色不能相同。

每个房子粉刷成不同颜色的花费以一个 n x k 的矩阵表示。

例如，costs\[0][0] 表示第 0 幢房子粉刷成 0 号颜色的成本；costs\[1][2] 表示第 1 幢房子粉刷成 2 号颜色的成本，以此类推。
返回 粉刷完所有房子的最低成本 。

__示例：__

示例 1：

输入: costs = [[1,5,3],[2,9,4]]
输出: 5
解释:
将房子 0 刷成 0 号颜色，房子 1 刷成 2 号颜色。花费: 1 + 4 = 5;
或者将 房子 0 刷成 2 号颜色，房子 1 刷成 0 号颜色。花费: 3 + 2 = 5.

示例 2:

输入: costs = [[1,3],[2,4]]
输出: 5

__提示：__

costs.length == n
costs[i].length == k
1 <= n <= 100
2 <= k <= 20
1 <= costs\[i][j] <= 20

__思路：__

1. 动态规划
设 dp\[i][j] 表示粉刷 costs\[i][j] 的最小累计成本
dp\[i][j] = min(costs\[i][j] + dp\[i - 1][k] for k in range(n) if j != k)
即取遍所有上一行到值中取不与当前行相同的最小值
注意到当前值只和上一行相关, 可以将空间复杂度优化为 O(n)
时间复杂度为 O(mn ^ 2), 空间复杂度为 O(n)
2. 动态规划优化
注意到只需要求最值
那么只要记录上一行到最小值, 以及第二小的值
当这一行的值和上一行最小值位置相同时, 使用第二小的值
否则使用最小值进行更新
同样可以使用滚动数组优化空间复杂度
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minCostII(vector<vector<int>>& costs) 
    {
        int m = costs.size(), n = costs.front().size(), first = INT_MAX, second = INT_MAX;
        vector<int> dp(costs.front());
        for (int j = 0; j < n; j++) 
        {
            if (dp[j] < first) 
            {
                second = first;
                first = dp[j];
            } 
            else if (dp[j] < second) second = dp[j];
        }
        for (int i = 1; i < m; i++) 
        {
            vector<int> cur(n);
            int first_cur = INT_MAX, second_cur = INT_MAX;
            for (int j = 0; j < n; j++) 
            {
                int pre = dp[j] != first ? first : second;
                cur[j] = pre + costs[i][j];
                if (cur[j] < first_cur) 
                {
                    second_cur = first_cur;
                    first_cur = cur[j];
                } 
                else if (cur[j] < second_cur) second_cur = cur[j];
            }
            dp = cur;
            first = first_cur;
            second = second_cur;
        }
        return *min_element(dp.begin(), dp.end());
    }
};
```

__Java__:

```Java
class Solution {
    public int minCostII(int[][] costs) {
        int m = costs.length, n = costs[0].length, dp[] = costs[0], first = Integer.MAX_VALUE, second = Integer.MAX_VALUE;
        for (int j = 0; j < n; j++) {
            if (dp[j] < first) {
                second = first;
                first = dp[j];
            } else if (dp[j] < second) second = dp[j];
        }
        for (int i = 1; i < m; i++) {
            int cur[] = new int[n], firstCur = Integer.MAX_VALUE, secondCur = Integer.MAX_VALUE;
            for (int j = 0; j < n; j++) {
                int pre = dp[j] != first ? first : second;
                cur[j] = pre + costs[i][j];
                if (cur[j] < firstCur) {
                    secondCur = firstCur;
                    firstCur = cur[j];
                } else if (cur[j] < secondCur) secondCur = cur[j];
            }
            dp = cur;
            first = firstCur;
            second = secondCur;
        }
        return Arrays.stream(dp).min().getAsInt();
    }
}
```

__Python__:

```Python
class Solution:
    def minCostII(self, costs: List[List[int]]) -> int:
        dp, m, n = costs[0], len(costs), len(costs[0])
        for i in range(1, m):
            dp = [costs[i][j] + min(dp[k] for k in range(n) if k != j) for j in range(n)]
        return min(dp)
```
