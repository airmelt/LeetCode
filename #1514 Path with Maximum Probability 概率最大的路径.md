# 1514 Path with Maximum Probability 概率最大的路径

__Description:__

You are given an undirected weighted graph of `n` nodes (0-indexed), represented by an edge list where `edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b` with a probability of success of traversing that edge `succProb[i]`.

Given two nodes `start` and `end`, find the path with the maximum probability of success to go from `start` to `end` and return its success probability.

If there is no path from `start` to `end`, __return 0__. Your answer will be accepted if it differs from the correct answer by at most __1e-5__.

__Example:__

Example 1:

![1514-1](https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png)

```text
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
Output: 0.25000
Explanation: There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.
```

Example 2:

![1514-2](https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png)

```text
Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
Output: 0.30000
```

Example 3:

![1514-3](https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png)

```text
Input: n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
Output: 0.00000
Explanation: There is no path between 0 and 2.
```

__Constraints:__

- `2 <= n <= 10 ^ 4`
- `0 <= start, end < n`
- `start != end`
- `0 <= a, b < n`
- `a != b`
- `0 <= succProb.length == edges.length <= 2*10 ^ 4`
- `0 <= succProb[i] <= 1`
- There is at most one edge between every two nodes.

__题目描述:__

给你一个由 `n` 个节点（下标从 0 开始）组成的无向加权图，该图由一个描述边的列表组成，其中 `edges[i] = [a, b]` 表示连接节点 a 和 b 的一条无向边，且该边遍历成功的概率为 `succProb[i]` 。

指定两个节点分别作为起点 `start` 和终点 `end` ，请你找出从起点到终点成功概率最大的路径，并返回其成功概率。

如果不存在从 `start` 到 `end` 的路径，请 __返回 0__ 。只要答案与标准答案的误差不超过 __1e-5__ ，就会被视作正确答案。

__示例:__

示例 1：

![1514-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex1.png)

```text
输入：n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
输出：0.25000
解释：从起点到终点有两条路径，其中一条的成功概率为 0.2 ，而另一条为 0.5 * 0.5 = 0.25
```

示例 2：

![1514-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex2.png)

```text
输入：n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
输出：0.30000
```

示例 3：

![1514-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/12/1558_ex3.png)

```text
输入：n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
输出：0.00000
解释：节点 0 和 节点 2 之间不存在路径
```

__提示：__

- `2 <= n <= 10 ^ 4`
- `0 <= start, end < n`
- `start != end`
- `0 <= a, b < n`
- `a != b`
- `0 <= succProb.length == edges.length <= 2*10 ^ 4`
- `0 <= succProb[i] <= 1`
- 每两个节点之间最多有一条边

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        vector<vector<pair<int, double>>> graph(n);
        for (int i = 0; i<edges.size(); i++) 
        {
            graph[edges[i].front()].push_back({edges[i].back(), succProb[i]});
            graph[edges[i].back()].push_back({edges[i].front(), succProb[i]});
        }
        vector<double> p(n, 0.0);
        priority_queue<pair<double, int>> q;
        q.push({1.0, start});
        while (!q.empty()) 
        {
            auto node = q.top();
            q.pop();
            if (node.first <= p[node.second]) continue;
            p[node.second] = node.first;
            for (const auto& e: graph[node.second]) q.push({node.first * e.second, e.first});
        }
        return p[end];
    }
};
```

__Java__:

```Java
class Solution {
    public double maxProbability(int n, int[][] edges, double[] succProb, int start, int end) {
        List<List<Pair>> graph = new ArrayList<List<Pair>>();
        for (int i = 0; i < n; i++) graph.add(new ArrayList<Pair>());
        for (int i = 0; i < edges.length; i++) {
            graph.get(edges[i][0]).add(new Pair(edges[i][1], succProb[i]));
            graph.get(edges[i][1]).add(new Pair(edges[i][0], succProb[i]));
        }
        PriorityQueue<Pair> pq = new PriorityQueue<Pair>();
        double[] p = new double[n];
        pq.offer(new Pair(start, 1));
        p[start] = 1;
        while (!pq.isEmpty()) {
            Pair pair = pq.poll();
            if (pair.probability < p[pair.node]) continue;
            for (Pair neighbor : graph.get(pair.node)) {
                if (p[neighbor.node] < p[pair.node] * neighbor.probability) {
                    p[neighbor.node] = p[pair.node] * neighbor.probability;
                    pq.offer(new Pair(neighbor.node, p[neighbor.node]));
                }
            }
        }
        return p[end];
    }
}

class Pair implements Comparable<Pair> {
    double probability;
    int node;

    public Pair(int node, double probability) {
        this.probability = probability;
        this.node = node;
    }

    public int compareTo(Pair pair2) {
        if (this.probability == pair2.probability) {
            return this.node - pair2.node;
        } else {
            return this.probability - pair2.probability > 0 ? -1 : 1;
        }
    }
}
```

__Python__:

```Python
class Solution:
    def maxProbability(self, n: int, edges: List[List[int]], succProb: List[float], start: int, end: int) -> float:
        graph, q, p = [{} for _ in range(n)], [start], [0] * n
        p[start] = 1
        for (u, v), k in zip(edges, succProb):
            graph[u][v] = graph[v][u] = k
        for i in q:
            for j in graph[i]:
                if (cur := p[i] * graph[i][j]) > p[j]:
                    q.append(j)
                    p[j] = cur
        return p[end]
```
