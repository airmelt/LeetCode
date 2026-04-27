# 2440 Create Components With Same Value 创建价值相同的连通块

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`.

You are given a __0-indexed__ integer array `nums` of length `n` where `nums[i]` represents the value of the `i ^ th` node. You are also given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

You are allowed to __delete__ some edges, splitting the tree into multiple connected components. Let the __value__ of a component be the sum of __all__ `nums[i]` for which node `i` is in the component.

Return the maximum number of edges you can delete, such that every connected component in the tree has the same value.

__Example:__

Example 1:

![2440-1](https://assets.leetcode.com/uploads/2022/08/26/diagramdrawio.png)

```text
Input: nums = [6,2,2,2,6], edges = [[0,1],[1,2],[1,3],[3,4]] 
Output: 2 
Explanation: The above figure shows how we can delete the edges [0,1] and [3,4]. The created components are nodes [0], [1,2,3] and [4]. The sum of the values in each component equals 6. It can be proven that no better deletion exists, so the answer is 2.
```

Example 2:

```text
Input: nums = [2], edges = []
Output: 0
Explanation: There are no edges to be deleted.
```

__Constraints:__

- `1 <= n <= 2 * 10 ^ 4`
- `nums.length == n`
- `1 <= nums[i] <= 50`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= edges[i][0], edges[i][1] <= n - 1`
- `edges` represents a valid tree.

__题目描述:__

有一棵 `n` 个节点的无向树，节点编号为 `0` 到 `n - 1` 。

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `nums` ，其中 `nums[i]` 表示第 `i` 个节点的值。同时给你一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` 表示节点 `ai` 与 `bi` 之间有一条边。

你可以 __删除__ 一些边，将这棵树分成几个连通块。一个连通块的 __价值__ 定义为这个连通块中 __所有__ 节点 `i` 对应的 `nums[i]` 之和。

你需要删除一些边，删除后得到的各个连通块的价值都相等。请返回你可以删除的边数 最多 为多少。

__示例:__

示例 1：

![2440-2](https://assets.leetcode.com/uploads/2022/08/26/diagramdrawio.png)

```text
输入：nums = [6,2,2,2,6], edges = [[0,1],[1,2],[1,3],[3,4]] 
输出：2 
解释：上图展示了我们可以删除边 [0,1] 和 [3,4] 。得到的连通块为 [0] ，[1,2,3] 和 [4] 。每个连通块的价值都为 6 。可以证明没有别的更好的删除方案存在了，所以答案为 2 。
```

示例 2：

```text
输入：nums = [2], edges = []
输出：0
解释：没有任何边可以删除。
```

__提示：__

- `1 <= n <= 2 * 10 ^ 4`
- `nums.length == n`
- `1 <= nums[i] <= 50`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= edges[i][0], edges[i][1] <= n - 1`
- `edges` 表示一棵合法的树。

__思路:__

```text
DFS
若最后剩下 i 个连通块，则需要删除 i - 1 条边
设 total = sum(nums), 则每块的价值为 total / i, 所以 total % i == 0, 记 total / i = target
每块的最小值为 max(nums), 所以 i <= total / max(nums)
以 0 为根遍历整棵树
当 total % i == 0 时, 尝试将树分成 i 块, 每块的价值为 target
如果当前的块累计值为 target 就需要分出一块并返回 0, 视为成功
如果累计值超过 target 就返回 -1, 视为失败
否则继续累积当前块的值
时间复杂度为 O(MN), 空间复杂度为 O(N), M 为 sum(nums) 的因子个数
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int componentValue(vector<int>& nums, vector<vector<int>>& edges) {
        int n = nums.size(), total = accumulate(nums.begin(), nums.end(), 0), max_value = total / *max_element(nums.begin(), nums.end());
        vector<vector<int>> graph(n);
        for (const auto& edge : edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        function<int(int, int, int)> dfs = [&](int u, int fa, int target)
        {
            int s = nums[u];
            for (const auto& v : graph[u])
            {
                if (v != fa)
                {
                    int nxt = dfs(v, u, target);
                    if (nxt < 0) return -1;
                    s += nxt;
                }
            }
            return s > target ? -1 : (s != target) * s;
        };
        for (int i = max_value; i > 1; i--) if (!(total % i)) if (!dfs(0, -1, total / i)) return i - 1;
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    private List<Integer>[] graph;
    private int[] nums;

    public int componentValue(int[] nums, int[][] edges) {
        int total = Arrays.stream(nums).sum(), maxValue = total / Arrays.stream(nums).max().getAsInt(), n = nums.length;
        graph = new ArrayList[n];
        Arrays.setAll(graph, e -> new ArrayList<>());
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        this.nums = nums;
        for (int i = maxValue; i > 1; i--) if (total % i == 0) if (dfs(0, -1, total / i) == 0) return i - 1;
        return 0;
    }

    private int dfs(int u, int fa, int target) {
        int s = nums[u];
        for (int v : graph[u]) {
            if (v != fa) {
                int nxt = dfs(v, u, target);
                if (nxt < 0) return -1;
                s += nxt;
            }
        }
        return s > target ? -1 : (s == target ? 0 : s);
    }
}
```

__Python__:

```Python
class Solution:
    def componentValue(self, nums: List[int], edges: List[List[int]]) -> int:
        graph, max_value = defaultdict(set), (total := sum(nums)) // max(nums)
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(u: int, fa: int, target: int) -> int:
            s = nums[u]
            for v in graph[u]:
                if v != fa:
                    if (nxt := dfs(v, u, target)) < 0: 
                        return -1
                    s += nxt
            return -1 if s > target else (s != target) * s
        for i in range(max_value, 1, -1):
            if not total % i:
                if not dfs(0, -1, total // i): 
                    return i - 1
        return 0
```
