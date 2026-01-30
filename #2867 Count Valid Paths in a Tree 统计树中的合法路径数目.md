# 2867 Count Valid Paths in a Tree 统计树中的合法路径数目

__Description:__

There is an undirected tree with `n` nodes labeled from `1` to `n`. You are given the integer `n` and a 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree.

Return the number of valid paths in the tree.

A path `(a, b)` is __valid__ if there exists __exactly one__ prime number among the node labels in the path from `a` to `b`.

Note that:

- The path `(a, b)` is a sequence of __distinct__ nodes starting with node `a` and ending with node `b` such that every two adjacent nodes in the sequence share an edge in the tree.
- Path `(a, b)` and path `(b, a)` are considered the __same__ and counted only __once__.

__Example:__

Example 1:

![2867-1](https://assets.leetcode.com/uploads/2023/08/27/example1.png)

```text
Input: n = 5, edges = [[1,2],[1,3],[2,4],[2,5]]
Output: 4
Explanation: The pairs with exactly one prime number on the path between them are: 
```

- (1, 2) since the path from 1 to 2 contains prime number 2.
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (2, 4) since the path from 2 to 4 contains prime number 2.

It can be shown that there are only 4 valid paths.

Example 2:

![2867-2](https://assets.leetcode.com/uploads/2023/08/27/example2.png)

```text
Input: n = 6, edges = [[1,2],[1,3],[2,4],[3,5],[3,6]]
Output: 6
Explanation: The pairs with exactly one prime number on the path between them are: 
```

- (1, 2) since the path from 1 to 2 contains prime number 2.
- (1, 3) since the path from 1 to 3 contains prime number 3.
- (1, 4) since the path from 1 to 4 contains prime number 2.
- (1, 6) since the path from 1 to 6 contains prime number 3.
- (2, 4) since the path from 2 to 4 contains prime number 2.
- (3, 6) since the path from 3 to 6 contains prime number 3.

It can be shown that there are only 6 valid paths.

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- The input is generated such that `edges` represent a valid tree.

__题目描述:__

给你一棵 `n` 个节点的无向树，节点编号为 `1` 到 `n` 。给你一个整数 `n` 和一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ui, vi]` 表示节点 `ui` 和 `vi` 在树中有一条边。

请你返回树中的 合法路径数目 。

如果在节点 `a` 到节点 `b` 之间 __恰好有一个__ 节点的编号是质数，那么我们称路径 `(a, b)` 是 __合法的__ 。

注意：

- 路径 `(a, b)` 指的是一条从节点 `a` 开始到节点 `b` 结束的一个节点序列，序列中的节点 __互不相同__ ，且相邻节点之间在树上有一条边。
- 路径 `(a, b)` 和路径 `(b, a)` 视为 __同一条__ 路径，且只计入答案 __一次__ 。

__示例:__

示例 1：

![2867-3](https://assets.leetcode.com/uploads/2023/08/27/example1.png)

```text
输入：n = 5, edges = [[1,2],[1,3],[2,4],[2,5]]
输出：4
解释：恰好有一个质数编号的节点路径有：
```

- (1, 2) 因为路径 1 到 2 只包含一个质数 2 。
- (1, 3) 因为路径 1 到 3 只包含一个质数 3 。
- (1, 4) 因为路径 1 到 4 只包含一个质数 2 。
- (2, 4) 因为路径 2 到 4 只包含一个质数 2 。

只有 4 条合法路径。

示例 2：

![2867-4](https://assets.leetcode.com/uploads/2023/08/27/example2.png)

```text
输入：n = 6, edges = [[1,2],[1,3],[2,4],[3,5],[3,6]]
输出：6
解释：恰好有一个质数编号的节点路径有：
```

- (1, 2) 因为路径 1 到 2 只包含一个质数 2 。
- (1, 3) 因为路径 1 到 3 只包含一个质数 3 。
- (1, 4) 因为路径 1 到 4 只包含一个质数 2 。
- (1, 6) 因为路径 1 到 6 只包含一个质数 3 。
- (2, 4) 因为路径 2 到 4 只包含一个质数 2 。
- (3, 6) 因为路径 3 到 6 只包含一个质数 3 。

只有 6 条合法路径。

__提示：__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- 输入保证 `edges` 形成一棵合法的树。

__思路:__

```text
DFS
先用筛法选出 100000 以内的质数和非质数
然后遍历质数节点的非质数节点邻居
计算并记录每一块非质数节点邻居的大小
那么每一块都能和当前遍历的质数节点以及其他节点组成一条路径
最后从质数节点自身出发也能构造一条路径
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
constexpr int MX = 1e5 + 3;
vector<int> primes;
bool is_not_prime[MX + 1];

int init = []() 
{
    for (int i = 2; i < MX; i++) 
    {
        if (!is_not_prime[i]) 
        {
            primes.emplace_back(i);
            for (int j = i; j <= MX / i; j++) is_not_prime[i * j] = true;
        }
    }
    is_not_prime[1] = true;
    return 0;
}();
class Solution 
{
public:
    long long countPaths(int n, vector<vector<int>>& edges) 
    {
        long long result = 0LL;
        vector<long long> size(n + 1);
        graph.resize(n + 1);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        } 
        for (int u = 1; u <= n; u++)
        {
            if (!is_not_prime[u])
            {
                long long cur = 0LL;
                for (int v : graph[u])
                {
                    if (is_not_prime[v])
                    {
                        nodes.clear();
                        if (!size[v]) dfs(v, -1);
                        for (const auto& node : nodes) size[node] = nodes.size();
                    }
                    result += cur * size[v];
                    cur += size[v];
                }
                result += cur;
            }
        }
        return result;
    }
private:
    vector<vector<int>> graph;
    vector<int> nodes;

    void dfs(int u, int fa)
    {
        nodes.emplace_back(u);
        for (const auto& v : graph[u]) if (v != fa and is_not_prime[v]) dfs(v, u);
    }
};
```

__Java__:

```Java
class Solution {
    private final static int MX = 100_003;
    private final static List<Integer> primes = new ArrayList<>();
    private final static boolean[] isNotPrime = new boolean[MX + 1];

    static {
        for (int i = 2; i < MX; i++) {
            if (!isNotPrime[i]) {
                primes.add(i);
                for (int j = i; j <= MX / i; j++) isNotPrime[i * j] = true;
            }
        }
        isNotPrime[1] = true;
    }

    private List<Integer> nodes = new ArrayList<>();
    private List<Integer>[] graph;

    public long countPaths(int n, int[][] edges) {
        long result = 0L, size[] = new long[n + 1];
        graph = new ArrayList[n + 1];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (var edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        for (int u = 1; u <= n; u++) {
            if (!isNotPrime[u]) {
                long cur = 0L;
                for (int v : graph[u]) {
                    if (isNotPrime[v]) {
                        if (size[v] == 0L) {
                            nodes.clear();
                            dfs(v, -1);
                            for (int node : nodes) size[node] = nodes.size();
                        }
                        result += size[v] * cur;
                        cur += size[v];
                    }
                }
                result += cur;
            }
        }
        return result;
    }

    private void dfs(int u, int fa) {
        nodes.add(u);
        for (int v : graph[u]) if (v != fa && isNotPrime[v]) dfs(v, u);
    }
}
```

__Python__:

```Python
MX = 10 ** 5 + 3
primes = set()
is_prime = [True] * MX
for i in range(2, MX):
    if is_prime[i]:
        primes.add(i)
        for j in range(i * i, MX, i):
            is_prime[j] = False
is_prime[1] = False

class Solution:
    def countPaths(self, n: int, edges: List[List[int]]) -> int:
        graph, nodes, result, size = defaultdict(set), [], 0, [0] * (n + 1)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        def dfs(u: int, fa: int) -> None:
            nodes.append(u)
            for v in graph[u]:
                if v != fa and not is_prime[v]:
                    dfs(v, u)
        for u in range(1, n + 1):
            if is_prime[u]:
                cur = 0
                for v in graph[u]:
                    if not is_prime[v]:
                        if not size[v]:
                            nodes.clear()
                            dfs(v, -1)
                            for node in nodes:
                                size[node] = len(nodes)
                    result += size[v] * cur
                    cur += size[v]
                result += cur
        return result
```
