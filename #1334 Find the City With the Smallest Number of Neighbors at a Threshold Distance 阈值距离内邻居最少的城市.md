# 1334 Find the City With the Smallest Number of Neighbors at a Threshold Distance 阈值距离内邻居最少的城市

__Description:__

There are n cities numbered from 0 to n-1. Given the array edges where edges[i] = [fromi, toi, weighti] represents a bidirectional and weighted edge between cities fromi and toi, and given the integer distanceThreshold.

Return the city with the smallest number of cities that are reachable through some path and whose distance is at most distanceThreshold, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities i and j is equal to the sum of the edges' weights along that path.

__Example:__

Example 1:

![find_the_city_01](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png)

Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
Output: 3
Explanation: The figure above describes the graph.
The neighboring cities at a distanceThreshold = 4 for each city are:
City 0 -> [City 1, City 2]
City 1 -> [City 0, City 2, City 3]
City 2 -> [City 0, City 1, City 3]
City 3 -> [City 1, City 2]
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.

Example 2:

![find_the_city_02](https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png)

Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
Output: 0
Explanation: The figure above describes the graph.
The neighboring cities at a distanceThreshold = 2 for each city are:
City 0 -> [City 1]
City 1 -> [City 0, City 4]
City 2 -> [City 3, City 4]
City 3 -> [City 2, City 4]
City 4 -> [City 1, City 2, City 3]
The city 0 has 1 neighboring city at a distanceThreshold = 2.

__Constraints:__

2 <= n <= 100
1 <= edges.length <= n * (n - 1) / 2
edges[i].length == 3
0 <= fromi < toi < n
1 <= weighti, distanceThreshold <= 10^4
All pairs (fromi, toi) are distinct.

__题目描述：__

有 n 个城市，按从 0 到 n-1 编号。给你一个边数组 edges，其中 edges[i] = [fromi, toi, weighti] 代表 fromi 和 toi 两个城市之间的双向加权边，距离阈值是一个整数 distanceThreshold。

返回能通过某些路径到达其他城市数目最少、且路径距离 最大 为 distanceThreshold 的城市。如果有多个这样的城市，则返回编号最大的城市。

注意，连接城市 i 和 j 的路径的距离等于沿该路径的所有边的权重之和。

__示例：__

示例 1：

![最少的城市 01](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/26/find_the_city_01.png)

输入：n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
输出：3
解释：城市分布图如上。
每个城市阈值距离 distanceThreshold = 4 内的邻居城市分别是：
城市 0 -> [城市 1, 城市 2]
城市 1 -> [城市 0, 城市 2, 城市 3]
城市 2 -> [城市 0, 城市 1, 城市 3]
城市 3 -> [城市 1, 城市 2]
城市 0 和 3 在阈值距离 4 以内都有 2 个邻居城市，但是我们必须返回城市 3，因为它的编号最大。

示例 2：

![最少的城市 02](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/26/find_the_city_02.png)

输入：n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
输出：0
解释：城市分布图如上。
每个城市阈值距离 distanceThreshold = 2 内的邻居城市分别是：
城市 0 -> [城市 1]
城市 1 -> [城市 0, 城市 4]
城市 2 -> [城市 3, 城市 4]
城市 3 -> [城市 2, 城市 4]
城市 4 -> [城市 1, 城市 2, 城市 3]
城市 0 在阈值距离 2 以内只有 1 个邻居城市。

__提示：__

2 <= n <= 100
1 <= edges.length <= n * (n - 1) / 2
edges[i].length == 3
0 <= fromi < toi < n
1 <= weighti, distanceThreshold <= 10^4
所有 (fromi, toi) 都是不同的。

__思路：__

动态规划
Floyd 算法
设 dp\[i][j] 表示从 i 到 j 的最短路径
初始化 dp\[i][i] = 0, dp\[i][j] = float('inf')
遍历所有节点 t, 如果 dp\[i][j] < dp\[i][t] + dp\[t][j]
即从 i 直接到 j, 不如从 i 先到 t 再到 j, 则更新 dp\[i][j] 为较小值
然后统计所有节点到其他节点距离小于阈值的数量并返回相应下标
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) 
    {
        vector<vector<int>> dp(n, vector<int>(n, 0x3f3f3f3f));
        for (const auto& edge : edges) dp[edge[0]][edge[1]] = dp[edge[1]][edge[0]] = edge[2];
        for (int i = 0; i < n; i++) dp[i][i] = 0;
        for (int t = 0; t < n; t++) for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (dp[i][t] + dp[t][j] < dp[i][j]) dp[i][j] = dp[i][t] + dp[t][j];
        int result = 0, min_count = n - 1;
        for (int i = 0; i < n; i++)
        {
            int count = 0;
            for (int j = 0; j < n; j++) if (i != j and dp[i][j] <= distanceThreshold) ++count;
            if (min_count >= count)
            {
                result = i;
                min_count = count;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        int result = 0, minCount = n - 1, dp[][] = new int[n][n];
        for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (i != j) dp[i][j] = 0x3f3f3f3f;
        for (int[] edge : edges) dp[edge[0]][edge[1]] = dp[edge[1]][edge[0]] = edge[2];
        for (int t = 0; t < n; t++) for (int i = 0; i < n; i++) for (int j = 0; j < n; j++) if (dp[i][j] > dp[i][t] + dp[t][j]) dp[i][j] = dp[i][t] + dp[t][j];
        for (int i = 0; i < n; i++) {
            int count = 0;
            for (int j = 0; j < n; j++) if (i != j && dp[i][j] <= distanceThreshold) ++count;
            if (minCount >= count) {
                minCount = count;
                result = i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findTheCity(self, n: int, edges: List[List[int]], distanceThreshold: int) -> int:
        result, min_count, dp = 0, n - 1, [[float('inf')] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = 0
        for edge in edges:
            dp[edge[0]][edge[1]] = dp[edge[1]][edge[0]] = edge[2]
        for t in range(n):
            for i in range(n):
                for j in range(n):
                    dp[i][j] = min(dp[i][j], dp[i][t] + dp[t][j])
        for i in range(n):
            if min_count >= (count := sum(1 for j in range(n) if i != j and dp[i][j] <= distanceThreshold)):
                result, min_count = i, count
        return result
```
