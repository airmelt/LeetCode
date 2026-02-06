# 2876 Count Visited Nodes in a Directed Graph 有向图访问计数

__Description:__

There is a __directed__ graph consisting of `n` nodes numbered from `0` to `n - 1` and `n` directed edges.

You are given a __0-indexed__ array `edges` where `edges[i]` indicates that there is an edge from node `i` to node `edges[i]`.

Consider the following process on the graph:

- You start from a node `x` and keep visiting other nodes through edges until you reach a node that you have already visited before on this __same__ process.

Return _an array_ `answer` _where_ `answer[i]` _is the number of __different__ nodes that you will visit if you perform the process starting from node_ `i`.

__Example:__

Example 1:

![2876-1](https://assets.leetcode.com/uploads/2023/08/31/graaphdrawio-1.png)

```text
Input: edges = [1,2,0,0]
Output: [3,3,3,4]
Explanation: We perform the process starting from each node in the following way:
```

- Starting from node 0, we visit the nodes 0 -> 1 -> 2 -> 0. The number of different nodes we visit is 3.
- Starting from node 1, we visit the nodes 1 -> 2 -> 0 -> 1. The number of different nodes we visit is 3.
- Starting from node 2, we visit the nodes 2 -> 0 -> 1 -> 2. The number of different nodes we visit is 3.
- Starting from node 3, we visit the nodes 3 -> 0 -> 1 -> 2 -> 0. The number of different nodes we visit is 4.

Example 2:

![2876-2](https://assets.leetcode.com/uploads/2023/08/31/graaph2drawio.png)

```text
Input: edges = [1,2,3,4,0]
Output: [5,5,5,5,5]
Explanation: Starting from any node we can visit every node in the graph in the process.
```

__Constraints:__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `0 <= edges[i] <= n - 1`
- `edges[i] != i`

__题目描述:__

现有一个有向图，其中包含 `n` 个节点，节点编号从 `0` 到 `n - 1` 。此外，该图还包含了 `n` 条有向边。

给你一个下标从 __0__ 开始的数组 `edges` ，其中 `edges[i]` 表示存在一条从节点 `i` 到节点 `edges[i]` 的边。

想象在图上发生以下过程：

- 你从节点 `x` 开始，通过边访问其他节点，直到你在 __此过程__ 中再次访问到之前已经访问过的节点。

返回数组 `answer` 作为答案，其中 `answer[i]` 表示如果从节点 `i` 开始执行该过程，你可以访问到的不同节点数。

__示例:__

示例 1：

![2876-3](https://assets.leetcode.com/uploads/2023/08/31/graaphdrawio-1.png)

```text
输入：edges = [1,2,0,0]
输出：[3,3,3,4]
解释：从每个节点开始执行该过程，记录如下：
```

- 从节点 0 开始，访问节点 0 -> 1 -> 2 -> 0 。访问的不同节点数是 3 。
- 从节点 1 开始，访问节点 1 -> 2 -> 0 -> 1 。访问的不同节点数是 3 。
- 从节点 2 开始，访问节点 2 -> 0 -> 1 -> 2 。访问的不同节点数是 3 。
- 从节点 3 开始，访问节点 3 -> 0 -> 1 -> 2 -> 0 。访问的不同节点数是 4 。

示例 2：

![2876-4](https://assets.leetcode.com/uploads/2023/08/31/graaph2drawio.png)

```text
输入：edges = [1,2,3,4,0]
输出：[5,5,5,5,5]
解释：无论从哪个节点开始，在这个过程中，都可以访问到图中的每一个节点。
```

__提示：__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `0 <= edges[i] <= n - 1`
- `edges[i] != i`

__思路:__

```text
基向内环树
遍历图
将图反向存入, 并反向记录度
利用拓扑排序移除所有树枝
这样度为 1 就在环上
度为 0 的就为树枝
遍历所有节点
如果度不为 0 说明在环上
找到环上所有节点, 并修改度为 -1 防止重复访问
那么环上邻居能访问到的节点就为环的大小及它到环上最近节点的深度
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countVisitedNodes(vector<int>& edges) 
    {
        int n = edges.size(), v = 0;
        vector<vector<int>> reverse_graph(n);
        vector<int> degree(n), result(n);
        for (int u = 0; u < n; u++)
        {
            ++degree[edges[u]];
            reverse_graph[edges[u]].emplace_back(u);
        }
        queue<int> q;
        for (int u = 0; u < n; u++) if (!degree[u]) q.push(u);
        while (!q.empty())
        {
            v = edges[q.front()];
            q.pop();
            if (!(--degree[v])) q.push(v);
        }
        auto reverse_dfs = [&](this auto&& reverse_dfs, int u, int depth) -> void
        {
            result[u] = depth;
            for (const auto& v : reverse_graph[u]) if (!degree[v]) reverse_dfs(v, depth + 1);
        };
        for (int i = 0; i < n; i++)
        {
            if (degree[i] > 0)
            {
                vector<int> ring;
                for (int u = i; ; u = edges[u])
                {
                    ring.emplace_back(u);
                    degree[u] = -1;
                    if (edges[u] == i) break;
                }
                for (const auto& u : ring) reverse_dfs(u, ring.size());
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private int[] graph;
    private int n;
    private List<Integer>[] reverseGraph;
    private int[] degree;
    private int[] result;

    public int[] countVisitedNodes(List<Integer> edges) {
        graph = edges.stream().mapToInt(i -> i).toArray();
        n = graph.length;
        reverseGraph = new ArrayList[n];
        Arrays.setAll(reverseGraph, e -> new ArrayList<>());
        degree = new int[n];
        result = new int[n];
        int v = 0;
        for (int u = 0; u < n; u++) {
            ++degree[graph[u]];
            reverseGraph[graph[u]].add(u);
        }
        var q = new ArrayDeque<Integer>();
        for (int u = 0; u < n; u++) if (degree[u] == 0) q.add(u);
        while (!q.isEmpty()) if (--degree[v = graph[q.poll()]] == 0) q.add(v);
        for (int i = 0; i < n; i++) {
            if (degree[i] > 0) {
                var ring = new HashSet<Integer>();
                for (int u = i; ; u = graph[u]) {
                    degree[u] = -1;
                    ring.add(u);
                    if (graph[u] == i) break;
                }
                for (int u : ring) reverseDFS(u, ring.size());
            }
        }
        return result;
    }

    private void reverseDFS(int u, int depth) {
        result[u] = depth;
        for (int v : reverseGraph[u]) if (degree[v] == 0) reverseDFS(v, depth + 1);
    }
}
```

__Python__:

```Python
class Solution:
    def countVisitedNodes(self, edges: List[int]) -> List[int]:
        degree, reverse_graph, result = [0] * (n := len(edges)), defaultdict(set), [0] * n
        for u, v in enumerate(edges):
            reverse_graph[v].add(u)
            degree[v] += 1
        q = deque(u for u, d in enumerate(degree) if not d)
        while q:
            degree[v := edges[u := q.popleft()]] -= 1
            if not degree[v]:
                q.append(v)
        
        def reverse_dfs(u: int, depth: int) -> None:
            result[u] = depth
            for v in reverse_graph[u]:
                if not degree[v]:
                    reverse_dfs(v, depth + 1)
        for i, d in enumerate(degree):
            if d > 0:
                ring, u = set(), i
                while True:
                    ring.add(u)
                    degree[u] = -1
                    if (u := edges[u]) == i:
                        break
                for u in ring:
                    reverse_dfs(u, len(ring))
        return result
```
