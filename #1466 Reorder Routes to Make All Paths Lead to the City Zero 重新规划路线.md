# 1466 Reorder Routes to Make All Paths Lead to the City Zero 重新规划路线

__Description:__

There are  `n` cities numbered from  `0` to  `n - 1` and  `n - 1` roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by  `connections` where  `connections[i] = [ai, bi]` represents a road from city  `ai` to city  `bi`.

This year, there will be a big event in the capital (city  `0`), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city  `0`. Return the __minimum__ number of edges changed.

It's __guaranteed__ that each city can reach city  `0` after reorder.

__Example:__

Example 1:

![1466-1](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)

```text
Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
Output: 3
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

Example 2:

![1466-2](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)

```text
Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
Output: 2
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

Example 3:

```text
Input: n = 3, connections = [[1,0],[2,0]]
Output: 0
```

__Constraints:__

- `2 <= n <= 5 * 10 ^ 4`
- `connections.length == n - 1`
- `connections[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

__题目描述:__

`n` 座城市，从  `0` 到  `n-1` 编号，其间共有  `n-1` 条路线。因此，要想在两座不同城市之间旅行只有唯一一条路线可供选择（路线网形成一颗树）。去年，交通运输部决定重新规划路线，以改变交通拥堵的状况。

路线用  `connections` 表示，其中  `connections[i] = [a, b]` 表示从城市  `a` 到  `b` 的一条有向路线。

今年，城市 0 将会举办一场大型比赛，很多游客都想前往城市 0 。

请你帮助重新规划路线方向，使每个城市都可以访问城市 0 。返回需要变更方向的最小路线数。

题目数据 保证 每个城市在重新规划路线方向后都能到达城市 0 。

__示例:__

示例 1：

![1466-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/sample_1_1819.png)

```text
输入：n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
输出：3
解释：更改以红色显示的路线的方向，使每个城市都可以到达城市 0 。
```

示例 2：

![1466-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/30/sample_2_1819.png)

```text
输入：n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
输出：2
解释：更改以红色显示的路线的方向，使每个城市都可以到达城市 0 。
```

示例 3：

```text
输入：n = 3, connections = [[1,0],[2,0]]
输出：0
```

__提示：__

- `2 <= n <= 5 * 10 ^ 4`
- `connections.length == n-1`
- `connections[i].length == 2`
- `0 <= connections[i][0], connections[i][1] <= n-1`
- `connections[i][0] != connections[i][1]`

__思路:__

```text
BFS
记录所有结点的邻接结点并记录方向
因为需要记录最后修改方向的数量, 可以记录方向时用 1 表示与需要方向相反
从 0 出发遍历所有结点即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minReorder(int n, vector<vector<int>>& connections) 
    {
        vector<vector<pair<int, int>>> graph(n);
        for (const auto& c: connections)
        {
            graph[c.front()].emplace_back(make_pair(c.back(), 1));
            graph[c.back()].emplace_back(make_pair(c.front(), 0));
        }
        queue<int> q;
        q.push(0);
        vector<bool> visited(n);
        visited.front() = true;
        int result = 0;
        while (!q.empty())
        {
            int cur = q.front();
            q.pop();
            for (const auto& v: graph[cur])
            {
                if (!visited[v.first])
                {
                    visited[v.first] = true;
                    result += v.second;
                    q.push(v.first);
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
    public int minReorder(int n, int[][] connections) {
        List<List<int[]>> graph = new ArrayList<>();
        for (int i = 0; i < n; ++i) graph.add(new ArrayList<>());
        for (int[] c: connections) {
            graph.get(c[0]).add(new int[]{c[1], 1});
            graph.get(c[1]).add(new int[]{c[0], 0});
        }
        boolean[] visited = new boolean[n];
        Deque<Integer> queue = new ArrayDeque<>();
        queue.offer(0);
        visited[0] = true;
        int result = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int[] v: graph.get(cur)) {
                if (!visited[v[0]]) {
                    visited[v[0]] = true;
                    result += v[1];
                    queue.add(v[0]);
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minReorder(self, n: int, connections: List[List[int]]) -> int:
        graph, queue, result, visited = defaultdict(deque), deque([0]), 0, [True] + [False] * (n - 1)
        for u, v in connections:
            graph[u].append((v, 1))
            graph[v].append((u, 0))
        while queue:
            cur = queue.popleft()
            for v, cost in graph[cur]:
                if not visited[v]:
                    visited[v] = True
                    result += cost
                    queue.append(v)
        return result
```
