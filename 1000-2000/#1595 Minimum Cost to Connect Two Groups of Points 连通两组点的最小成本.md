# 1595 Minimum Cost to Connect Two Groups of Points 连通两组点的最小成本

__Description:__

You are given two groups of points where the first group has `size1` points, the second group has `size2` points, and `size1 >= size2`.

The `cost` of the connection between any two points are given in an `size1 x size2` matrix where `cost[i][j]` is the cost of connecting point `i` of the first group and point `j` of the second group. The groups are connected if __each point in both groups is connected to one or more points in the opposite group__. In other words, each point in the first group must be connected to at least one point in the second group, and each point in the second group must be connected to at least one point in the first group.

Return the minimum cost it takes to connect the two groups.

__Example:__

Example 1:

![1595-1](https://assets.leetcode.com/uploads/2020/09/03/ex1.jpg)

```text
Input: cost = [[15, 96], [36, 2]]
Output: 17
Explanation: The optimal way of connecting the groups is:
1--A
2--B
This results in a total cost of 17.
```

Example 2:

![1595-2](https://assets.leetcode.com/uploads/2020/09/03/ex2.jpg)

```text
Input: cost = [[1, 3, 5], [4, 1, 1], [1, 5, 3]]
Output: 4
Explanation: The optimal way of connecting the groups is:
1--A
2--B
2--C
3--A
This results in a total cost of 4.
Note that there are multiple points connected to point 2 in the first group and point A in the second group. This does not matter as there is no limit to the number of points that can be connected. We only care about the minimum total cost.
```

Example 3:

```text
Input: cost = [[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]
Output: 10
```

__Constraints:__

- `size1 == cost.length`
- `size2 == cost[i].length`
- `1 <= size1, size2 <= 12`
- `size1 >= size2`
- `0 <= cost[i][j] <= 100`

__题目描述:__

给你两组点，其中第一组中有 `size1` 个点，第二组中有 `size2` 个点，且 `size1 >= size2` 。

任意两点间的连接成本 `cost` 由大小为 `size1 x size2` 矩阵给出，其中 `cost[i][j]` 是第一组中的点 `i` 和第二组中的点 `j` 的连接成本。__如果两个组中的每个点都与另一组中的一个或多个点连接，则称这两组点是连通的。__换言之，第一组中的每个点必须至少与第二组中的一个点连接，且第二组中的每个点必须至少与第一组中的一个点连接。

返回连通两组点所需的最小成本。

__示例:__

示例 1：

![1595-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/20/ex1.jpg)

```text
输入：cost = [[15, 96], [36, 2]]
输出：17
解释：连通两组点的最佳方法是：
1--A
2--B
总成本为 17 。
```

示例 2：

![1595-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/09/20/ex2.jpg)

```text
输入：cost = [[1, 3, 5], [4, 1, 1], [1, 5, 3]]
输出：4
解释：连通两组点的最佳方法是：
1--A
2--B
2--C
3--A
最小成本为 4 。
请注意，虽然有多个点连接到第一组中的点 2 和第二组中的点 A ，但由于题目并不限制连接点的数目，所以只需要关心最低总成本。
```

示例 3：

```text
输入：cost = [[2, 5, 1], [3, 4, 7], [8, 1, 2], [6, 2, 4], [3, 8, 8]]
输出：10
```

__提示：__

- `size1 == cost.length`
- `size2 == cost[i].length`
- `1 <= size1, size2 <= 12`
- `size1 >= size2`
- `0 <= cost[i][j] <= 100`

__思路:__

```text
动态规划 ➕ 状态压缩
在矩阵中每一行每一列都要选出至少一个点
dp[i][j] 表示选取第 i 行, 选取的情况为 j
最后在最后一行需要每一个元素都选中
dp 有 m + 1 行, 1 << n 列
初始化 dp[i][j] = INF, dp[0][0] = 0
转移方程为 dp[i][mask] = min(dp[i][mask], min(min(dp[i - 1][mask], dp[i - 1][mask ^ (1 << j)]), min(dp[i][mask], dp[i][mask ^ (1 << j)])) + cost[i - 1][j])
注意到 dp[i] 只和前一行有关, 可以用滚动数组降低空间复杂度
时间复杂度为 O(MN2 ^ N), 空间复杂度为 O(M2 ^ N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int connectTwoGroups(vector<vector<int>>& cost) 
    {
        int m = cost.size(), n = cost[0].size(), INF = 0x3f3f3f3f;
        vector<vector<int>>dp(m + 1, vector<int>(1 << n, INF));
        dp.front().front() = 0;
        for (int i = 1; i <= m; i++) for (int mask = 0; mask < (1 << n); mask++) for (int j = 0; j < n; j++) if (mask & (1 << j)) dp[i][mask] = min(dp[i][mask], min(min(dp[i - 1][mask], dp[i - 1][mask ^ (1 << j)]), min(dp[i][mask], dp[i][mask ^ (1 << j)])) + cost[i - 1][j]);
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int connectTwoGroups(List<List<Integer>> cost) {
        int m = cost.size(), n = cost.get(0).size(), INF = 0x3f3f3f3f, dp[][] = new int[m + 1][1 << n];
        for (int i = 0; i <= m; i++) Arrays.fill(dp[i], INF);
        dp[0][0] = 0;
        for (int i = 1; i <= m; i++) for (int mask = 0; mask < (1 << n); mask++) for (int j = 0; j < n; j++) if ((mask & (1 << j)) != 0) dp[i][mask] = Math.min(dp[i][mask], Math.min(Math.min(dp[i - 1][mask], dp[i - 1][mask ^ (1 << j)]), Math.min(dp[i][mask], dp[i][mask ^ (1 << j)])) + cost.get(i - 1).get(j));
        return dp[m][(1 << n)- 1];
    }
}
```

__Python__:

```Python
class Solution:
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        m, n = len(cost), len(cost[0])
        dp = [0] + [float('inf')] * ((1 << n) - 1)
        for i in range(1, m + 1):
            cur = [float('inf')] * (1 << n)
            for mask in range(1 << n):
                for j in range(n):
                    if mask & (1 << j):
                        cur[mask] = min(cur[mask], min(min(dp[mask], dp[mask ^ (1 << j)]), min(cur[mask], cur[mask ^ (1 << j)])) + cost[i - 1][j])
            dp = cur
        return dp[-1]
```
