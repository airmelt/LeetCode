# 1615 Maximal Network Rank 最大网络秩

__Description:__

There is an infrastructure of `n` cities with some number of `roads` connecting these cities. Each `roads[i] = [ai, bi]` indicates that there is a bidirectional road between cities `ai` and `bi`.

The network rank of two different cities is defined as the total number of directly connected roads to either city. If a road is directly connected to both cities, it is only counted once.

The maximal network rank of the infrastructure is the maximum network rank of all pairs of different cities.

Given the integer `n` and the array `roads`, return _the __maximal network rank__ of the entire infrastructure_.

__Example:__

Example 1:

![1615-1](https://assets.leetcode.com/uploads/2020/09/21/ex1.png)

```text
Input: n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
Output: 4
Explanation: The network rank of cities 0 and 1 is 4 as there are 4 roads that are connected to either 0 or 1. The road between 0 and 1 is only counted once.
```

Example 2:

![1615-2](https://assets.leetcode.com/uploads/2020/09/21/ex2.png)

```text
Input: n = 5, roads = [[0,1],[0,3],[1,2],[1,3],[2,3],[2,4]]
Output: 5
Explanation: There are 5 roads that are connected to cities 1 or 2.
```

Example 3:

```text
Input: n = 8, roads = [[0,1],[1,2],[2,3],[2,4],[5,6],[5,7]]
Output: 5
Explanation: The network rank of 2 and 5 is 5. Notice that all the cities do not have to be connected.
```

__Constraints:__

- `2 <= n <= 100`
- `0 <= roads.length <= n * (n - 1) / 2`
- `roads[i].length == 2`
- `0 <= ai, bi <= n-1`
- `ai != bi`
- Each pair of cities has __at most one__ road connecting them.

__题目描述:__

`n` 座城市和一些连接这些城市的道路 `roads` 共同组成一个基础设施网络。每个 `roads[i] = [ai, bi]` 都表示在城市 `ai` 和 `bi` 之间有一条双向道路。

两座不同城市构成的 城市对 的 网络秩 定义为：与这两座城市 直接 相连的道路总数。如果存在一条道路直接连接这两座城市，则这条道路只计算 一次 。

整个基础设施网络的 最大网络秩 是所有不同城市对中的 最大网络秩 。

给你整数 `n` 和数组 `roads`，返回整个基础设施网络的 __最大网络秩__ 。

__示例:__

示例 1：

![1615-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/ex1.png)

```text
输入：n = 4, roads = [[0,1],[0,3],[1,2],[1,3]]
输出：4
解释：城市 0 和 1 的网络秩是 4，因为共有 4 条道路与城市 0 或 1 相连。位于 0 和 1 之间的道路只计算一次。
```

示例 2：

![1615-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/11/ex2.png)

```text
输入：n = 5, roads = [[0,1],[0,3],[1,2],[1,3],[2,3],[2,4]]
输出：5
解释：共有 5 条道路与城市 1 或 2 相连。
```

示例 3：

```text
输入：n = 8, roads = [[0,1],[1,2],[2,3],[2,4],[5,6],[5,7]]
输出：5
解释：2 和 5 的网络秩为 5，注意并非所有的城市都需要连接起来。
```

__提示：__

- `2 <= n <= 100`
- `0 <= roads.length <= n * (n - 1) / 2`
- `roads[i].length == 2`
- `0 <= ai, bi <= n-1`
- `ai != bi`
- 每对城市之间 __最多只有一条__ 道路相连

__思路:__

```text
1. 模拟
将道路转化为邻接表
统计每个城市的度数
最大网络秩即 degree[i] + degree[j] - graph[i][j]
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
2. 贪心
只需要考虑度最大的城市 first 和次大的城市 second
如果这两个城市相连, 返回 first + second - 1
否则统计 fisrt 和 second 连接的城市
如果 first 只连接一个城市, 枚举 second 连接的城市找到最大的网络秩, 返回 first + second - graph[f][s]
否则枚举 first 连接的城市, 找到最大的网络秩, 返回 fisrt * 2 - graph[f][s]
时间复杂度为 O(N + M), 空间复杂度为 O(N ^ 2), 其中 M 为 roads 数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maximalNetworkRank(int n, vector<vector<int>>& roads) 
    {
        vector<vector<bool>> graph(n, vector<bool>(n));
        vector<int> degree(n), first_road, second_road;
        for (const auto& edge : roads) 
        {
            graph[edge.front()][edge.back()] = graph[edge.back()][edge.front()] = true;
            ++degree[edge.front()];
            ++degree[edge.back()];
        }
        int first = -1, second = -2, m = roads.size();
        for (int i = 0; i < n; i++) 
        {
            if (degree[i] > first) 
            {
                second = first;
                second_road = first_road;
                first = degree[i];
                first_road.clear();
                first_road.emplace_back(i);
            } 
            else if (degree[i] == first) first_road.emplace_back(i);
            else if (degree[i] > second) 
            {
                second_road.clear();
                second = degree[i];
                second_road.emplace_back(i);
            } 
            else if (degree[i] == second) second_road.emplace_back(i);
        }
        if (first_road.size() == 1) 
        {
            for (const auto& v : second_road) if (!graph[first_road.front()][v]) return first + second;
            return first + second - 1;
        } 
        else 
        {
            if (first_road.size() * (first_road.size() - 1) >> 1 > m) return first << 1;
            for (const auto& u: first_road) for (const auto& v: first_road) if (u != v && !graph[u][v]) return first << 1;
            return (first << 1) - 1;
        }
    }
};
```

__Java__:

```Java
class Solution {
    public int maximalNetworkRank(int n, int[][] roads) {
        boolean[][] graph = new boolean[n][n];
        int degree[] = new int[n], result = 0;
        for (int[] edge : roads) {
            graph[edge[0]][edge[1]] = graph[edge[1]][edge[0]] = true;
            ++degree[edge[0]];
            ++degree[edge[1]];
        }
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) result = Math.max(result, degree[i] + degree[j] - (graph[i][j] ? 1 : 0));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximalNetworkRank(self, n: int, roads: List[List[int]]) -> int:
        graph, degree, result = [[False] * n for _ in range(n)], [0] * n, 0
        for u, v in roads:
            degree[u] += 1
            degree[v] += 1
            graph[u][v] = graph[v][u] = True
        for i in range(n):
            for j in range(i + 1, n):
                result = max(result, degree[i] + degree[j] - graph[i][j])
        return result
```
