# 2492 Minimum Score of a Path Between Two Cities 两个城市间路径的最小分数

__Description:__

You are given a positive integer `n` representing `n` cities numbered from `1` to `n`. You are also given a __2D__ array `roads` where `roads[i] = [ai, bi, distance_i]` indicates that there is a __bidirectional__ road between cities `ai` and `bi` with a distance equal to `distance_i`. The cities graph is not necessarily connected.

The score of a path between two cities is defined as the minimum distance of a road in this path.

Return _the __minimum__ possible score of a path between cities_ `1` _and_ `n`.

Note:

- A path is a sequence of roads between two cities.
- It is allowed for a path to contain the same road __multiple__ times, and you can visit cities `1` and `n` multiple times along the path.
- The test cases are generated such that there is __at least__ one path between `1` and `n`.

__Example:__

Example 1:

![2492-1](https://assets.leetcode.com/uploads/2022/10/12/graph11.png)

```text
Input: n = 4, roads = [[1,2,9],[2,3,6],[2,4,5],[1,4,7]]
Output: 5
Explanation: The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 4. The score of this path is min(9,5) = 5.
It can be shown that no other path has less score.
```

Example 2:

![2492-2](https://assets.leetcode.com/uploads/2022/10/12/graph22.png)

```text
Input: n = 4, roads = [[1,2,2],[1,3,4],[3,4,7]]
Output: 2
Explanation: The path from city 1 to 4 with the minimum score is: 1 -> 2 -> 1 -> 3 -> 4. The score of this path is min(2,2,4,7) = 2.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `1 <= roads.length <= 10 ^ 5`
- `roads[i].length == 3`
- `1 <= ai, bi <= n`
- `ai != bi`
- `1 <= distance_i <= 10 ^ 4`
- There are no repeated edges.
- There is at least one path between `1` and `n`.

__题目描述:__

给你一个正整数 `n` ，表示总共有 `n` 个城市，城市从 `1` 到 `n` 编号。给你一个二维数组 `roads` ，其中 `roads[i] = [ai, bi, distance_i]` 表示城市 `ai` 和 `bi` 之间有一条 __双向__ 道路，道路距离为 `distance_i` 。城市构成的图不一定是连通的。

两个城市之间一条路径的 分数 定义为这条路径中道路的 __最小__ 距离。

城市 `1` 和城市 `n` 之间的所有路径的 最小 分数。

注意：

- 一条路径指的是两个城市之间的道路序列。
- 一条路径可以 __多次__ 包含同一条道路，你也可以沿着路径多次到达城市 `1` 和城市 `n` 。
- 测试数据保证城市 `1` 和城市 `n` 之间 __至少__ 有一条路径。

__示例:__

示例 1：

![2492-3](https://assets.leetcode.com/uploads/2022/10/12/graph11.png)

```text
输入：n = 4, roads = [[1,2,9],[2,3,6],[2,4,5],[1,4,7]]
输出：5
解释：城市 1 到城市 4 的路径中，分数最小的一条为：1 -> 2 -> 4 。这条路径的分数是 min(9,5) = 5 。
不存在分数更小的路径。
```

示例 2：

![2492-4](https://assets.leetcode.com/uploads/2022/10/12/graph22.png)

```text
输入：n = 4, roads = [[1,2,2],[1,3,4],[3,4,7]]
输出：2
解释：城市 1 到城市 4 分数最小的路径是：1 -> 2 -> 1 -> 3 -> 4 。这条路径的分数是 min(2,2,4,7) = 2 。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `1 <= roads.length <= 10 ^ 5`
- `roads[i].length == 3`
- `1 <= ai, bi <= n`
- `ai != bi`
- `1 <= distance_i <= 10 ^ 4`
- 不会有重复的边。
- 城市 `1` 和城市 `n` 之间至少有一条路径。

__思路:__

```text
1. 并查集
将所有联通的城市合并到一个并查集中
记录每个城市的最小距离
返回 0 所在的并查集的最小距离
时间复杂度为 O(N), 空间复杂度为 O(N)
2. DFS
从城市 1 开始 DFS 遍历所有城市
每次遍历到一个城市, 更新最小距离
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minScore(int n, vector<vector<int>>& roads) 
    {
        vector<bool> visited(n);
        vector<vector<pair<int ,int>>> graph(n);
        for (const auto& road : roads)
        {
            graph[road[0] - 1].emplace_back(road[1] - 1, road[2]);
            graph[road[1] - 1].emplace_back(road[0] - 1, road[2]);
        }
        auto dfs = [&](this auto&& dfs, int u) -> int
        {
            int result = INT_MAX;
            visited[u] = true;
            for (const auto& [v, w]: graph[u])
            {
                result = min(result, w);
                if (!visited[v]) result = min(dfs(v), result);
            }
            return result;
        };
        return dfs(0);
    }
};
```

__Java__:

```Java
class Solution {
    private boolean[] visited;

    public int minScore(int n, int[][] roads) {
        visited = new boolean[n];
        List<int[]>[] graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int[] road : roads) {
            graph[road[0] - 1].add(new int[]{road[1] - 1, road[2]});
            graph[road[1] - 1].add(new int[]{road[0] - 1, road[2]});
        }
        return dfs(graph, 0);
    }

    private int dfs(List<int[]>[] graph, int u) {
        int result = Integer.MAX_VALUE;
        visited[u] = true;
        for (int[] neighbor : graph[u]) {
            result = Math.min(result, neighbor[1]);
            if (!visited[neighbor[0]]) result = Math.min(result, dfs(graph, neighbor[0]));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minScore(self, n: int, roads: List[List[int]]) -> int:
        parent, min_value = list(range(n)), [inf] * n

        def merge(p: int, q: int, w: int) -> NoReturn:
            if (p := find(p)) > (q := find(q)):
                p, q = q, p
            parent[q] = p
            min_value[q] = min(min_value[p], min_value[q], w)
        
        def find(p: int) -> int:
            while p != parent[p]:
                parent[p] = parent[parent[p]]
                p = parent[p]
            return parent[p]

        for u, v, w in roads:
            merge(u - 1, v - 1, w)
        return min(v for i, v in enumerate(min_value) if not find(i))
```
