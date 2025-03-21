# 2477 Minimum Fuel Cost to Report to the Capital 到达首都的最少油耗

__Description:__

There is a tree (i.e., a connected, undirected graph with no cycles) structure country network consisting of `n` cities numbered from `0` to `n - 1` and exactly `n - 1` roads. The capital city is city `0`. You are given a 2D integer array `roads` where `roads[i] = [ai, bi]` denotes that there exists a __bidirectional road__ connecting cities `ai` and `bi`.

There is a meeting for the representatives of each city. The meeting is in the capital city.

There is a car in each city. You are given an integer `seats` that indicates the number of seats in each car.

A representative can use the car in their city to travel or change the car and ride with another representative. The cost of traveling between two cities is one liter of fuel.

Return the minimum number of liters of fuel to reach the capital city.

__Example:__

Example 1:

![2477-1](https://assets.leetcode.com/uploads/2022/09/22/a4c380025e3ff0c379525e96a7d63a3.png)

```text
Input: roads = [[0,1],[0,2],[0,3]], seats = 5
Output: 3
Explanation: 
```

- Representative1 goes directly to the capital with 1 liter of fuel.
- Representative2 goes directly to the capital with 1 liter of fuel.
- Representative3 goes directly to the capital with 1 liter of fuel.

It costs 3 liters of fuel at minimum.

It can be proven that 3 is the minimum number of liters of fuel needed.

Example 2:

![2477-2](https://assets.leetcode.com/uploads/2022/11/16/2.png)

```text
Input: roads = [[3,1],[3,2],[1,0],[0,4],[0,5],[4,6]], seats = 2
Output: 7
Explanation: 
```

- Representative2 goes directly to city 3 with 1 liter of fuel.
- Representative2 and representative3 go together to city 1 with 1 liter of fuel.
- Representative2 and representative3 go together to the capital with 1 liter of fuel.
- Representative1 goes directly to the capital with 1 liter of fuel.
- Representative5 goes directly to the capital with 1 liter of fuel.
- Representative6 goes directly to city 4 with 1 liter of fuel.
- Representative4 and representative6 go together to the capital with 1 liter of fuel.

It costs 7 liters of fuel at minimum.

It can be proven that 7 is the minimum number of liters of fuel needed.

Example 3:

![2477-3](https://assets.leetcode.com/uploads/2022/09/27/efcf7f7be6830b8763639cfd01b690a.png)

```text
Input: roads = [], seats = 1
Output: 0
Explanation: No representatives need to travel to the capital city.
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `roads.length == n - 1`
- `roads[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `roads` represents a valid tree.
- `1 <= seats <= 10 ^ 5`

__题目描述:__

给你一棵 `n` 个节点的树（一个无向、连通、无环图），每个节点表示一个城市，编号从 `0` 到 `n - 1` ，且恰好有 `n - 1` 条路。 `0` 是首都。给你一个二维整数数组 `roads` ，其中 `roads[i] = [ai, bi]` ，表示城市 `ai` 和 `bi` 之间有一条 __双向路__ 。

每个城市里有一个代表，他们都要去首都参加一个会议。

每座城市里有一辆车。给你一个整数 `seats` 表示每辆车里面座位的数目。

城市里的代表可以选择乘坐所在城市的车，或者乘坐其他城市的车。相邻城市之间一辆车的油耗是一升汽油。

请你返回到达首都最少需要多少升汽油。

__示例:__

示例 1：

![2477-4](https://assets.leetcode.com/uploads/2022/09/22/a4c380025e3ff0c379525e96a7d63a3.png)

```text
输入：roads = [[0,1],[0,2],[0,3]], seats = 5
输出：3
解释：
```

- 代表 1 直接到达首都，消耗 1 升汽油。
- 代表 2 直接到达首都，消耗 1 升汽油。
- 代表 3 直接到达首都，消耗 1 升汽油。

最少消耗 3 升汽油。

示例 2：

![2477-5](https://assets.leetcode.com/uploads/2022/11/16/2.png)

```text
输入：roads = [[3,1],[3,2],[1,0],[0,4],[0,5],[4,6]], seats = 2
输出：7
解释：
```

- 代表 2 到达城市 3 ，消耗 1 升汽油。
- 代表 2 和代表 3 一起到达城市 1 ，消耗 1 升汽油。
- 代表 2 和代表 3 一起到达首都，消耗 1 升汽油。
- 代表 1 直接到达首都，消耗 1 升汽油。
- 代表 5 直接到达首都，消耗 1 升汽油。
- 代表 6 到达城市 4 ，消耗 1 升汽油。
- 代表 4 和代表 6 一起到达首都，消耗 1 升汽油。

最少消耗 7 升汽油。

示例 3：

![2477-6](https://assets.leetcode.com/uploads/2022/09/27/efcf7f7be6830b8763639cfd01b690a.png)

```text
输入：roads = [], seats = 1
输出：0
解释：没有代表需要从别的城市到达首都。
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `roads.length == n - 1`
- `roads[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `roads` 表示一棵合法的树。
- `1 <= seats <= 10 ^ 5`

__思路:__

```text
DFS
从首都开始 DFS 遍历
每次遍历到一个城市, 累计这个城市的子树大小
如果是首都, 直接返回
如果不是首都, 计算这个城市的油耗
油耗 = (子树大小 - 1) / 座位数 + 1, 即子树大小除以座位数向上取整
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    long long minimumFuelCost(vector<vector<int>>& roads, int seats) 
    {
        long long result = 0LL;
        vector<vector<int>> graph(roads.size() + 1);
        for (const auto& road : roads) 
        {
            graph[road.front()].emplace_back(road.back());
            graph[road.back()].emplace_back(road.front());
        }
        function<int(int, int)> dfs = [&](int u, int fa) -> int
        {
            int size = 1;
            for (const auto& v : graph[u]) if (v != fa) size += dfs(v, u);
            if (u) result += (size - 1) / seats + 1;
            return size;
        };
        dfs(0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private long result = 0L, seats = 0L;
    private List<Integer>[] graph;

    public long minimumFuelCost(int[][] roads, int seats) {
        this.seats = seats;
        this.graph = new ArrayList[roads.length + 1];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int[] road : roads) {
            graph[road[0]].add(road[1]);
            graph[road[1]].add(road[0]);
        }
        dfs(0, -1);
        return result;
    }

    private int dfs(int u, int fa) {
        int size = 1;
        for (int v : graph[u]) if (v != fa) size += dfs(v, u);
        if (u != 0) result += (size - 1) / seats + 1;
        return size;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumFuelCost(self, roads: List[List[int]], seats: int) -> int:
        graph, result = defaultdict(set), 0
        for u, v in roads:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(u: int, fa: int) -> int:
            size = 1
            for v in graph[u]:
                if v != fa:
                    size += dfs(v, u)
            if u:
                nonlocal result
                result += (size - 1) // seats + 1
            return size
        dfs(0, -1)
        return result
```
