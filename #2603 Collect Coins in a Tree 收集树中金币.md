# 2603 Collect Coins in a Tree 收集树中金币

__Description:__

There exists an undirected and unrooted tree with `n` nodes indexed from `0` to `n - 1`. You are given an integer `n` and a 2D integer array edges of length `n - 1`, where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree. You are also given an array `coins` of size `n` where `coins[i]` can be either `0` or `1`, where `1` indicates the presence of a coin in the vertex `i`.

Initially, you choose to start at any vertex in the tree. Then, you can perform the following operations any number of times:

- Collect all the coins that are at a distance of at most `2` from the current vertex, or
- Move to any adjacent vertex in the tree.

Find the minimum number of edges you need to go through to collect all the coins and go back to the initial vertex.

Note that if you pass an edge several times, you need to count it into the answer several times.

__Example:__

Example 1:

![2603-1](https://assets.leetcode.com/uploads/2023/03/01/graph-2.png)

```text
Input: coins = [1,0,0,0,0,1], edges = [[0,1],[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: Start at vertex 2, collect the coin at vertex 0, move to vertex 3, collect the coin at vertex 5 then move back to vertex 2.
```

Example 2:

![2603-2](https://assets.leetcode.com/uploads/2023/03/02/graph-4.png)

```text
Input: coins = [0,0,0,1,1,0,0,1], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[5,6],[5,7]]
Output: 2
Explanation: Start at vertex 0, collect the coins at vertices 4 and 3, move to vertex 2,  collect the coin at vertex 7, then move back to vertex 0.
```

__Constraints:__

- `n == coins.length`
- `1 <= n <= 3 * 10 ^ 4`
- `0 <= coins[i] <= 1`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.

__题目描述:__

给你一个 `n` 个节点的无向无根树，节点编号从 `0` 到 `n - 1` 。给你整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示树中节点 `ai` 和 `bi` 之间有一条边。再给你一个长度为 `n` 的数组 `coins` ，其中 `coins[i]` 可能为 `0` 也可能为 `1` ， `1` 表示节点 `i` 处有一个金币。

一开始，你需要选择树中任意一个节点出发。你可以执行下述操作任意次：

- 收集距离当前节点距离为 `2` 以内的所有金币，或者
- 移动到树中一个相邻节点。

你需要收集树中所有的金币，并且回到出发节点，请你返回最少经过的边数。

如果你多次经过一条边，每一次经过都会给答案加一。

__示例:__

示例 1：

![2603-3](https://assets.leetcode.com/uploads/2023/03/01/graph-2.png)

```text
输入：coins = [1,0,0,0,0,1], edges = [[0,1],[1,2],[2,3],[3,4],[4,5]]
输出：2
解释：从节点 2 出发，收集节点 0 处的金币，移动到节点 3 ，收集节点 5 处的金币，然后移动回节点 2 。
```

示例 2：

![2603-4](https://assets.leetcode.com/uploads/2023/03/02/graph-4.png)

```text
输入：coins = [0,0,0,1,1,0,0,1], edges = [[0,1],[0,2],[1,3],[1,4],[2,5],[5,6],[5,7]]
输出：2
解释：从节点 0 出发，收集节点 4 和 3 处的金币，移动到节点 2 处，收集节点 7 处的金币，移动回节点 0 。
```

__提示：__

- `n == coins.length`
- `1 <= n <= 3 * 10 ^ 4`
- `0 <= coins[i] <= 1`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵合法的树。

__思路:__

```text
拓扑排序
先移除所有的没有金币的叶子节点, 并将其与父节点的边数减一
然后找到所有有金币的叶子节点, 由于不需要访问这些节点, 可以将其和父节点的边数减一
遍历所有有金币的叶子节点, 如果其父节点在删除这个叶子节点之后是叶子节点, 则将其和父节点的边数减一, 即这两个节点都不需要访问
剩下的节点都需要访问, 边需要经过两次
最后返回边数的两倍, 即为最少经过的边数, 最少为 0
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int collectTheCoins(vector<int>& coins, vector<vector<int>>& edges) 
    {
        int n = coins.size(), edge = n - 1;
        vector<vector<int>> graph(n);
        vector<int> degree(n);
        for (const auto& e : edges) 
        {
            graph[e.front()].emplace_back(e.back());
            graph[e.back()].emplace_back(e.front());
            ++degree[e.front()];
            ++degree[e.back()];
        }
        vector<int> q;
        for (int i = 0; i < n; i++) if (degree[i] == 1 and !coins[i]) q.emplace_back(i);
        while (!q.empty()) 
        {
            --edge;
            int u = q.back();
            q.pop_back();
            for (const auto& v : graph[u]) if (--degree[v] == 1 and !coins[v]) q.emplace_back(v);
        }
        for (int i = 0; i < n; i++) if (degree[i] == 1 and coins[i]) q.emplace_back(i);
        edge -= q.size();
        for (const auto& u : q) for (const auto& v : graph[u]) if (--degree[v] == 1) --edge;
        return max(edge << 1, 0);
    }
};
```

__Java__:

```Java
class Solution {
    public int collectTheCoins(int[] coins, int[][] edges) {
        int n = coins.length, degree[] = new int[n], edge = n - 1;
        List<Integer>[] graph = new ArrayList[n];
        Arrays.setAll(graph, a -> new ArrayList<>());
        for (int[] e : edges) {
            graph[e[0]].add(e[1]);
            graph[e[1]].add(e[0]);
            ++degree[e[0]];
            ++degree[e[1]];
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < n; i++) if (degree[i] == 1 && coins[i] == 0) queue.add(i);
        while (!queue.isEmpty()) {
            --edge;
            for (int v : graph[queue.poll()]) if (--degree[v] == 1 && coins[v] == 0) queue.add(v);
        }
        for (int i = 0; i < n; i++) if (degree[i] == 1 && coins[i] == 1) queue.add(i);
        edge -= queue.size();
        for (int u : queue) for (int v : graph[u]) if (--degree[v] == 1) --edge;
        return Math.max(edge << 1, 0);
    }
}
```

__Python__:

```Python
class Solution:
    def collectTheCoins(self, coins: List[int], edges: List[List[int]]) -> int:
        graph, n = defaultdict(set), len(coins)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        degree, edge = list(map(len, (graph[i] for i in range(n)))), n - 1
        q = [i for i, (d, c) in enumerate(zip(degree, coins)) if (d, c) == (1, 0)]
        while q:
            edge -= 1
            for v in graph[q.pop()]:
                degree[v] -= 1
                if degree[v] == 1 and not coins[v]:
                    q.append(v)
        edge -= len(q := [i for i, (d, c) in enumerate(zip(degree, coins)) if d == 1 and c])
        for u in q:
            for v in graph[u]:
                degree[v] -= 1
                if degree[v] == 1:
                    edge -= 1
        return max(edge << 1, 0)
```
