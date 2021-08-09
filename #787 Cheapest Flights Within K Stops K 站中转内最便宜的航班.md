# 787 Cheapest Flights Within K Stops K 站中转内最便宜的航班

__Description__:
There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

__Example:__

Example 1:

![flights](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
Output: 200
Explanation: The graph is shown.
The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.

Example 2:

![flights](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
Output: 500
Explanation: The graph is shown.
The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.

__Constraints:__

1 <= n <= 100
0 <= flights.length <= (n \* (n - 1) / 2)
flights[i].length == 3
0 <= fromi, toi < n
fromi != toi
1 <= pricei <= 104
There will not be any multiple flights between two cities.
0 <= src, dst, k < n
src != dst

__题目描述__:
有 n 个城市通过 m 个航班连接。每个航班都从城市 u 开始，以价格 w 抵达 v。

现在给定所有的城市和航班，以及出发城市 src 和目的地 dst，你的任务是找到从 src 到 dst 最多经过 k 站中转的最便宜的价格。 如果没有这样的路线，则输出 -1。

__示例 :__

示例 1：

输入:
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
输出: 200
解释:
城市航班图如下

![城市航班图](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

从城市 0 到城市 2 在 1 站中转以内的最便宜价格是 200，如图中红色所示。

示例 2：

输入:
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
输出: 500
解释:
城市航班图如下

![城市航班图](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

从城市 0 到城市 2 在 0 站中转以内的最便宜价格是 500，如图中蓝色所示。

__提示:__

n 范围是 [1, 100]，城市标签从 0 到 n - 1
航班数量范围是 [0, n \* (n - 1) / 2]
每个航班的格式 (src, dst, price)
每个航班的价格范围是 [1, 10000]
k 范围是 [0, n - 1]
航班没有重复，且不存在自环

__思路__:

动态规划
设 dp[i][k] 为转 k 次到达 i 站的最小花费
dp[i][k] = min(dp[i][k], dp[flight[0]][k - 1] + flight[2] for flight in flights)
注意到每次只受前一个状态影响, 可以将空间复杂度压缩至 n
用 pre 记录上一次的花费
dp[flight[1]] = min(dp[flight[1]], pre[flight[0]] + flight[2] for flight in flights)
时间复杂度为 O(nmk), 空间复杂度为 O(n), 其中 m 为 flights 的大小

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) 
    {
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;
        for (int i = 0; i < k + 1; i++)
        {
            vector<int> pre(dist);
            for (const auto& flight : flights) if (pre[flight.front()] != INT_MAX) dist[flight[1]] = min(dist[flight[1]], pre[flight.front()] + flight.back());
        }
        return dist[dst] == INT_MAX ? -1 : dist[dst];
    }
};
```

__Java__:

```Java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[][] dp = new int[n][k + 2];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], Integer.MAX_VALUE);
        for (int i = 0; i <= k + 1; i++) dp[src][i] = 0;
        for (int i = 1; i <= k + 1; i++) for (int[] flight : flights) if (dp[flight[0]][i - 1] != Integer.MAX_VALUE) dp[flight[1]][i] = Math.min(dp[flight[1]][i], dp[flight[0]][i - 1] + flight[2]);
        return dp[dst][k + 1] == Integer.MAX_VALUE ? -1 : dp[dst][k + 1];
    }
}
```

__Python__:

```Python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        dist = [float('inf') if i != src else 0 for i in range(n)]
        for i in range(k + 1):
            pre = dist[:]
            for u, v, w in flights:
                dist[v] = min(dist[v], pre[u] + w)
        return dist[dst] if dist[dst] != float('inf') else -1
```
