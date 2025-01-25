# 2421 Number of Good Paths 好路径的数目

__Description:__

There is a tree (i.e. a connected, undirected graph with no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` edges.

You are given a __0-indexed__ integer array `vals` of length `n` where `vals[i]` denotes the value of the `i ^ th` node. You are also given a 2D integer array `edges` where `edges[i] = [ai, bi]` denotes that there exists an __undirected__ edge connecting nodes `ai` and `bi`.

A good path is a simple path that satisfies the following conditions:

Return the number of distinct good paths.

Note that a path and its reverse are counted as the __same__ path. For example, `0 -> 1` is considered to be the same as `1 -> 0`. A single node is also considered as a valid path.

__Example:__

Example 1:

![2421-1](https://assets.leetcode.com/uploads/2022/08/04/f9caaac15b383af9115c5586779dec5.png)

```text
Input: vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
Output: 6
Explanation: There are 5 good paths consisting of a single node.
There is 1 additional good path: 1 -> 0 -> 2 -> 4.
(The reverse path 4 -> 2 -> 0 -> 1 is treated as the same as 1 -> 0 -> 2 -> 4.)
Note that 0 -> 2 -> 3 is not a good path because vals[2] > vals[0].
```

Example 2:

![2421-2](https://assets.leetcode.com/uploads/2022/08/04/149d3065ec165a71a1b9aec890776ff.png)

```text
Input: vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
Output: 7
Explanation: There are 5 good paths consisting of a single node.
There are 2 additional good paths: 0 -> 1 and 2 -> 3.
```

Example 3:

![2421-3](https://assets.leetcode.com/uploads/2022/08/04/31705e22af3d9c0a557459bc7d1b62d.png)

```text
Input: vals = [1], edges = []
Output: 1
Explanation: The tree consists of only one node, so there is one good path.
```

__Constraints:__

- `n == vals.length`
- `1 <= n <= 3 * 10 ^ 4`
- `0 <= vals[i] <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.

__题目描述:__

给你一棵 `n` 个节点的树（连通无向无环的图），节点编号从 `0` 到 `n - 1` 且恰好有 `n - 1` 条边。

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `vals` ，分别表示每个节点的值。同时给你一个二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 和 `bi` 之间有一条 __无向__ 边。

一条 好路径 需要满足以下条件：

请你返回不同好路径的数目。

注意，一条路径和它反向的路径算作 __同一__ 路径。比方说， `0 -> 1` 与 `1 -> 0` 视为同一条路径。单个节点也视为一条合法路径。

__示例:__

示例 1：

![2421-4](https://assets.leetcode.com/uploads/2022/08/04/f9caaac15b383af9115c5586779dec5.png)

```text
输入：vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
输出：6
解释：总共有 5 条单个节点的好路径。
还有 1 条好路径：1 -> 0 -> 2 -> 4 。
（反方向的路径 4 -> 2 -> 0 -> 1 视为跟 1 -> 0 -> 2 -> 4 一样的路径）
注意 0 -> 2 -> 3 不是一条好路径，因为 vals[2] > vals[0] 。
```

示例 2：

![2421-5](https://assets.leetcode.com/uploads/2022/08/04/149d3065ec165a71a1b9aec890776ff.png)

```text
输入：vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
输出：7
解释：总共有 5 条单个节点的好路径。
还有 2 条好路径：0 -> 1 和 2 -> 3 。
```

示例 3：

![2421-6](https://assets.leetcode.com/uploads/2022/08/04/31705e22af3d9c0a557459bc7d1b62d.png)

```text
输入：vals = [1], edges = []
输出：1
解释：这棵树只有一个节点，所以只有一条好路径。
```

__提示：__

- `n == vals.length`
- `1 <= n <= 3 * 10 ^ 4`
- `0 <= vals[i] <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵合法的树。

__思路:__

```text
并查集 ➕ 排序
构建无向图
使用并查集合并节点
由于起点和终点要求最大值相等, 所以可以将节点按照值从小到大排序
这里只需要将节点值对应的下标按照值从小到大排序即可
从最小的节点值开始遍历
对于节点 i, 将其与其相邻的节点 u 合并
如果 parent[u] != parent[i] and vals[parent[u]] <= vals[parent[i]], 这表示路径上 v = vals[i] 是最大值, 则合并节点 parent[u] 和 parent[i]
如果此时 vals[parent[u]] == vals[i], 则说明是一条可用的路径
根据乘法原理, 路径上一共有 weight[parent[u]] * weight[parent[i]] 条路径, 加入结果中
将路径上的节点数也进行合并
时间复杂度为 O(NlogN), 空间复杂度为 O(N), 其中 N 为 vals 的长度, 主要是排序的时间复杂度, 并查集时间复杂度为 O(aN), a 为阿克曼函数的反函数, 可以认为是 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfGoodPaths(vector<int>& vals, vector<vector<int>>& edges) 
    {
        int n = vals.size(), result = n;
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        vector<int> weight(n, 1), idx(n), parent(n);
        iota(idx.begin(), idx.end(), 0);
        iota(parent.begin(), parent.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return vals[i] < vals[j]; });
        function<int(int)> find = [&](int p) -> int { return parent[p] == p ? parent[p] : (parent[p] = find(parent[p])); };
        for (int i : idx)
        {
            int v = vals[i], p = find(i), q = 0;
            for (int u : graph[i])
            {
                if ((q = find(u)) != p and vals[q] <= vals[p])
                {
                    if (vals[q] == vals[p])
                    {
                        result += weight[p] * weight[q];
                        weight[p] += weight[q];
                    }
                    parent[q] = p;
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
    private int[] parent;

    public int numberOfGoodPaths(int[] vals, int[][] edges) {
        int n = vals.length, result = n, weight[] = new int[n];
        Arrays.fill(weight, 1);
        parent = new int[n];    
        var idx = new Integer[n];
        for (int i = 0; i < n; i++) parent[i] = idx[i] = i;
        Arrays.sort(idx, (i, j) -> vals[i] - vals[j]);
        List<Integer>[] graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        for (int i : idx) {
            int v = vals[i], p = find(i), q = 0;
            for (int u : graph[i]) {
                if ((q = find(u)) != p && vals[q] <= v) {
                    if (vals[q] == v) {
                        result += weight[p] * weight[q];
                        weight[p] += weight[q];
                    }
                    parent[q] = p;
                } 
            }
        }
        return result;
    }

    private int find(int p) {
        return parent[p] == p ? parent[p] : (parent[p] = find(parent[p]));
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfGoodPaths(self, vals: List[int], edges: List[List[int]]) -> int:
        graph, parent, weight, result = defaultdict(set), list(range(n := len(vals))), [1] * n, n
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def find(p: int) -> int:
            if parent[p] != p:
                parent[p] = find(parent[p])
            return parent[p]
        for v, i in sorted(zip(vals, range(n))):
            p = find(i)
            for u in graph[i]:
                if (q := find(u)) != p and vals[q] <= v:
                    if vals[q] == v:
                        result += weight[p] * weight[q]
                        weight[p] += weight[q]
                    parent[q] = p
        return result
```
