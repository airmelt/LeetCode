# 2581 Count Number of Possible Root Nodes 统计可能的树根数目

__Description:__

Alice has an undirected tree with `n` nodes labeled from `0` to `n - 1`. The tree is represented as a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Alice wants Bob to find the root of the tree. She allows Bob to make several guesses about her tree. In one guess, he does the following:

- Chooses two __distinct__ integers `u` and `v` such that there exists an edge `[u, v]` in the tree.
- He tells Alice that `u` is the __parent__ of `v` in the tree.

Bob's guesses are represented by a 2D integer array `guesses` where `guesses[j] = [uj, vj]` indicates Bob guessed `uj` to be the parent of `vj`.

Alice being lazy, does not reply to each of Bob's guesses, but just says that __at least__ `k` of his guesses are `true`.

Given the 2D integer arrays `edges`, `guesses` and the integer `k`, return _the __number of possible nodes__ that can be the root of Alice's tree_. If there is no such tree, return `0`.

__Example:__

Example 1:

![2581-1](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

```text
Input: edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
Output: 3
Explanation: 
Root = 0, correct guesses = [1,3], [0,1], [2,4]
Root = 1, correct guesses = [1,3], [1,0], [2,4]
Root = 2, correct guesses = [1,3], [1,0], [2,4]
Root = 3, correct guesses = [1,0], [2,4]
Root = 4, correct guesses = [1,3], [1,0]
Considering 0, 1, or 2 as root node leads to 3 correct guesses.
```

Example 2:

![2581-2](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)

```text
Input: edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
Output: 5
Explanation: 
Root = 0, correct guesses = [3,4]
Root = 1, correct guesses = [1,0], [3,4]
Root = 2, correct guesses = [1,0], [2,1], [3,4]
Root = 3, correct guesses = [1,0], [2,1], [3,2], [3,4]
Root = 4, correct guesses = [1,0], [2,1], [3,2]
Considering any node as root will give at least 1 correct guess.
```

__Constraints:__

- `edges.length == n - 1`
- `2 <= n <= 10 ^ 5`
- `1 <= guesses.length <= 10 ^ 5`
- `0 <= ai, bi, uj, vj <= n - 1`
- `ai != bi`
- `uj != vj`
- `edges` represents a valid tree.
- `guesses[j]` is an edge of the tree.
- `guesses` is unique.
- `0 <= k <= guesses.length`

__题目描述:__

Alice 有一棵 `n` 个节点的树，节点编号为 `0` 到 `n - 1` 。树用一个长度为 `n - 1` 的二维整数数组 `edges` 表示，其中 `edges[i] = [ai, bi]` ，表示树中节点 `ai` 和 `bi` 之间有一条边。

Alice 想要 Bob 找到这棵树的根。她允许 Bob 对这棵树进行若干次 猜测 。每一次猜测，Bob 做如下事情：

- 选择两个 __不相等__ 的整数 `u` 和 `v` ，且树中必须存在边 `[u, v]` 。
- Bob 猜测树中 `u` 是 `v` 的 __父节点__ 。

Bob 的猜测用二维整数数组 `guesses` 表示，其中 `guesses[j] = [uj, vj]` 表示 Bob 猜 `uj` 是 `vj` 的父节点。

Alice 非常懒，她不想逐个回答 Bob 的猜测，只告诉 Bob 这些猜测里面 __至少__ 有 `k` 个猜测的结果为 `true` 。

给你二维整数数组 `edges` ，Bob 的所有猜测和整数 `k` ，请你返回可能成为树根的 __节点数目__ 。如果没有这样的树，则返回 `0`。

__示例:__

示例 1：

![2581-3](https://assets.leetcode.com/uploads/2022/12/19/ex-1.png)

```text
输入：edges = [[0,1],[1,2],[1,3],[4,2]], guesses = [[1,3],[0,1],[1,0],[2,4]], k = 3
输出：3
解释：
根为节点 0 ，正确的猜测为 [1,3], [0,1], [2,4]
根为节点 1 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 2 ，正确的猜测为 [1,3], [1,0], [2,4]
根为节点 3 ，正确的猜测为 [1,0], [2,4]
根为节点 4 ，正确的猜测为 [1,3], [1,0]
节点 0 ，1 或 2 为根时，可以得到 3 个正确的猜测。
```

示例 2：

![2581-4](https://assets.leetcode.com/uploads/2022/12/19/ex-2.png)

```text
输入：edges = [[0,1],[1,2],[2,3],[3,4]], guesses = [[1,0],[3,4],[2,1],[3,2]], k = 1
输出：5
解释：
根为节点 0 ，正确的猜测为 [3,4]
根为节点 1 ，正确的猜测为 [1,0], [3,4]
根为节点 2 ，正确的猜测为 [1,0], [2,1], [3,4]
根为节点 3 ，正确的猜测为 [1,0], [2,1], [3,2], [3,4]
根为节点 4 ，正确的猜测为 [1,0], [2,1], [3,2]
任何节点为根，都至少有 1 个正确的猜测。
```

__提示：__

- `edges.length == n - 1`
- `2 <= n <= 10 ^ 5`
- `1 <= guesses.length <= 10 ^ 5`
- `0 <= ai, bi, uj, vj <= n - 1`
- `ai != bi`
- `uj != vj`
- `edges` 表示一棵有效的树。
- `guesses[j]` 是树中的一条边。
- `guesses` 是唯一的。
- `0 <= k <= guesses.length`

__思路:__

```text
换根 DP
将 edges 转换为邻接表
将 guesses 转换为一个集合 s, 方便后续查询, 为了快速计算, 可以将两个节点 u, v 转换为一个 long long 数, 计算方式为 (long long)u << 32 | v
使用 DFS 遍历树, 计算当前节点的父节点为根时, 有多少个正确的猜测, 记为 cur
使用另一个 DFS 遍历树, 重新计算每个节点的 cur, 如果当前节点的父节点为根时,
则当前节点的 cur = 父节点的 cur - 当前边是否在 guesses 中 + 当前节点的边反向是否在 guesses 中
即 cur = fa_cur - (u, v) in s + (v, u) in s
即 cur = fa_cur - s.count((long long)u << 32 | v) + s.count((long long)v << 32 | u)
如果当前节点的 cur >= k, 则结果 result += 1
最后返回结果 result
时间复杂度为 O(M + N), 空间复杂度为 O(M + N), 其中 M 为 guesses 的长度, N 为 edges 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int rootCount(vector<vector<int>>& edges, vector<vector<int>>& guesses, int k) 
    {
        unordered_map<int, unordered_set<int>> graph;
        for (const auto& edge : edges)
        {
            graph[edge.front()].insert(edge.back());
            graph[edge.back()].insert(edge.front());
        }
        unordered_set<long long> s;
        for (const auto& guess : guesses) s.insert((long long)guess.front() << 32 | guess.back());
        int result = 0, cur = 0;
        auto dfs = [&](this auto&& dfs, int u, int fa) -> void
        {
            for (const auto& v : graph[u])
            {
                if (v == fa) continue;
                if (s.count((long long)u << 32 | v)) ++cur;
                dfs(v, u);
            }
        };
        dfs(0, -1);
        auto reboot = [&](this auto&& reboot, int u, int fa, int cur) -> void
        {
            result += cur >= k;
            for (const auto& v : graph[u]) if (v != fa) reboot(v, u, cur - s.count((long long)u << 32 | v) + s.count((long long)v << 32 | u));
        };
        reboot(0, -1, cur);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private Set<Long> set = new HashSet<>();
    private int k, result, cur;

    public int rootCount(int[][] edges, int[][] guesses, int k) {
        this.k = k;
        graph = new ArrayList[edges.length + 1];
        Arrays.setAll(graph, i -> new ArrayList<>());
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }    
        for (int[] guess : guesses) set.add((long)guess[0] << 32 | guess[1]);
        dfs(0, -1);
        reroot(0, -1, cur);
        return result;
    }

    private void dfs(int u, int fa) {
        for (int v : graph[u]) {
            if (v != fa) {
                if (set.contains((long)u << 32 | v)) ++cur;
                dfs(v, u);
            }
        }
    }

    private void reroot(int u, int fa, int cur) {
        if (cur >= k) ++result;
        for (int v : graph[u]) if (v != fa) reroot(v, u, cur - (set.contains((long)u << 32 | v) ? 1 : 0) + (set.contains((long)v << 32 | u) ? 1 : 0));
    }
}
```

__Python__:

```Python
class Solution:
    def rootCount(self, edges: List[List[int]], guesses: List[List[int]], k: int) -> int:
        graph, s, result, cur = defaultdict(set), {(u, v) for u, v in guesses}, 0, 0
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        def dfs(u: int, fa: int) -> None:
            nonlocal cur
            for v in graph[u]:
                if v != fa:
                    dfs(v, u)
                    cur += (u, v) in s
        dfs(0, -1)

        def reroot(u: int, fa: int, cur: int) -> None:
            nonlocal result
            result += cur >= k
            for v in graph[u]:
                if v != fa:
                    reroot(v, u, cur - ((u, v) in s) + ((v, u) in s))
        reroot(0, -1, cur)
        return result
```
