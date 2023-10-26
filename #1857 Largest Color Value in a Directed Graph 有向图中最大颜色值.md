# 1857 Largest Color Value in a Directed Graph 有向图中最大颜色值

__Description:__

There is a __directed graph__ of `n` colored nodes and `m` edges. The nodes are numbered from `0` to `n - 1`.

You are given a string `colors` where `colors[i]` is a lowercase English letter representing the __color__ of the `i ^ th` node in this graph (__0-indexed__). You are also given a 2D array `edges` where `edges[j] = [aj, bj]` indicates that there is a __directed edge__ from node `aj` to node `bj`.

A valid __path__ in the graph is a sequence of nodes `x1 -> x2 -> x3 -> ... -> xk` such that there is a directed edge from `xi` to `xi+1` for every `1 <= i < k`. The __color value__ of the path is the number of nodes that are colored the __most frequently__ occurring color along that path.

Return _the __largest color value__ of any valid path in the given graph, or_ `-1` _if the graph contains a cycle_.

__Example:__

Example 1:

![1857-1](https://assets.leetcode.com/uploads/2021/04/21/leet1.png)

```text
Input:  colors = "abaca", edges = [[0,1],[0,2],[2,3],[3,4]]
Output:  3
Explanation:  The path 0 -> 2 -> 3 -> 4 contains 3 nodes that are colored `"a" (red in the above image)`.
```

Example 2:

![1857-2](https://assets.leetcode.com/uploads/2021/04/21/leet2.png)

```text
Input: colors = "a", edges = [[0,0]]
Output: -1
Explanation: There is a cycle from 0 to 0.
```

__Constraints:__

- `n == colors.length`
- `m == edges.length`
- `1 <= n <= 10 ^ 5`
- `0 <= m <= 10 ^ 5`
- `colors` consists of lowercase English letters.
- `0 <= aj, bj < n`

__题目描述:__

给你一个 __有向图__ ，它含有 `n` 个节点和 `m` 条边。节点编号从 `0` 到 `n - 1` 。

给你一个字符串 `colors` ，其中 `colors[i]` 是小写英文字母，表示图中第 `i` 个节点的 _颜色_ （下标从 __0__ 开始）。同时给你一个二维数组 `edges` ，其中 `edges[j] = [aj, bj]` 表示从节点 `aj` 到节点 `bj` 有一条 __有向边__ 。

图中一条有效 __路径__ 是一个点序列 `x1 -> x2 -> x3 -> ... -> xk` ，对于所有 `1 <= i < k` ，从 `xi` 到 `xi+1` 在图中有一条有向边。路径的 __颜色值__ 是路径中 __出现次数最多__ 颜色的节点数目。

请你返回给定图中有效路径里面的 __最大颜色值__。如果图中含有环，请返回 `-1` 。

__示例:__

示例 1：

![1857-3](https://assets.leetcode.com/uploads/2021/04/21/leet1.png)

```text
输入: colors = "abaca", edges = [[0,1],[0,2],[2,3],[3,4]]
输出: 3
解释: 路径 0 -> 2 -> 3 -> 4 含有 3 个颜色为 `"a" 的节点（上图中的红色节点）。`
```

示例 2：

![1857-4](https://assets.leetcode.com/uploads/2021/04/21/leet2.png)

```text
输入：colors = "a", edges = [[0,0]]
输出：-1
解释：从 0 到 0 有一个环。
```

__提示：__

- `n == colors.length`
- `m == edges.length`
- `1 <= n <= 10 ^ 5`
- `0 <= m <= 10 ^ 5`
- `colors` 只含有小写英文字母。
- `0 <= aj, bj < n`

__思路:__

```text
拓扑排序 ➕ 动态规划
设 dp[i][c] 表示第 i 个结点为终点的路径中颜色 c 出现的最大次数
用一个数组 degree 求出每个结点的入度
用一个数组 graph 存储每个结点的后继结点
初始化 graph 将所有结点加入, 并初始化 degree
将所有 degree 中入度为 0 的结点加入队列
遍历队列, 每次取出一个结点 u 增加其 dp 值
遍历所有邻居, 将所有后继结点修改为 u 的 dp 值
当邻居的入度为 0 时, 更新结果
每次遍历到入度为 0 的点加入队列
如果最后遍历的结点数不足 n 说明存在环, 返回 -1
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), M 为 edges 数组的长度, N 为 colors 字符串的长度, 即结点数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestPathValue(string colors, vector<vector<int>>& edges) 
    {
        int n = colors.size(), result = 0;
        vector<int> degree(n);
        vector<vector<int>> graph(n), dp(n, vector<int>(26));
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            ++degree[edge.back()];
        }
        queue<int> q;
        for (int i = 0; i < n; i++) if (!degree[i]) q.push(i);
        while (!q.empty()) 
        {
            int u = q.front();
            q.pop();
            ++dp[u][colors[u] - 'a'];
            --n;
            if (graph[u].empty()) 
            {
                for (int c = 0; c < 26; c++) result = max(result, dp[u][c]);
                continue;
            }
            for (int v : graph[u]) 
            {
                for (int c = 0; c < 26; c++) dp[v][c] = max(dp[v][c], dp[u][c]);
                if (!--degree[v]) q.push(v);
            }
        }
        return n ? -1 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public int largestPathValue(String colors, int[][] edges) {
        int n = colors.length(), result = 0, degree[] = new int[n], dp[][] = new int[n][26];
        List<List<Integer>> graph = new ArrayList<>(n);
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            ++degree[edge[1]];
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < n; i++) if (degree[i] == 0) queue.offer(i);
        while (!queue.isEmpty()) {
            int u = queue.poll();
            ++dp[u][colors.charAt(u) - 'a'];
            --n;
            if (graph.get(u).isEmpty()) {
                for (int c = 0; c < 26; c++) result = Math.max(result, dp[u][c]);
                continue;
            }
            for (int v : graph.get(u)) {
                for (int c = 0; c < 26; c++) dp[v][c] = Math.max(dp[v][c], dp[u][c]);
                if (--degree[v] == 0) queue.offer(v);
            }
        }
        return n == 0 ? result : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def largestPathValue(self, colors: str, edges: List[List[int]]) -> int:
        degree, f, graph, dp = [0] * (n := len(colors)), lambda x: ord(x) - ord('a'), defaultdict(set), [[0] * 26 for _ in range(n)]
        for u, v in edges:
            graph[u].add(v)
            degree[v] += 1
        q = [u for u in range(n) if not degree[u]]
        head, tail, result = 0, len(q), 0
        while head < tail:
            dp[u := q[head]][f(colors[u])] += 1
            if not graph[u]:
                result = max(result, *dp[u])
            for v in graph[u]:
                degree[v] -= 1
                for ch in range(26):
                    dp[v][ch] = max(dp[v][ch], dp[u][ch])
                if not degree[v]:
                    q.append(v)
                    tail += 1
            head += 1
        return result if tail == n else -1
```
