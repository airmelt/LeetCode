# 2322 Minimum Score After Removals on a Tree 从树中删除边的最小分数

__Description:__

There is an undirected connected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given a __0-indexed__ integer array `nums` of length `n` where `nums[i]` represents the value of the `i ^ th` node. You are also given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

Remove two distinct edges of the tree to form three connected components. For a pair of removed edges, the following steps are defined:

- For example, say the three components have the node values: `[4,5,7]`, `[1,9]`, and `[3,3,3]`. The three XOR values are `4 ^ 5 ^ 7` = __6__, `1 ^ 9` = __8__, and `3 ^ 3 ^ 3` = __3__. The largest XOR value is `8` and the smallest XOR value is `3`. The score is then `8 - 3 = 5`.

Return the minimum score of any possible pair of edge removals on the given tree.

__Example:__

Example 1:

![2322-1](https://assets.leetcode.com/uploads/2022/05/03/ex1drawio.png)

```text
Input: nums = [1,5,5,4,11], edges = [[0,1],[1,2],[1,3],[3,4]]
Output: 9
Explanation: The diagram above shows a way to make a pair of removals.
```

- The 1st component has nodes [1,3,4] with values [5,4,11]. Its XOR value is 5  ^  4  ^  11 = 10.
- The 2nd component has node [0] with value [1]. Its XOR value is 1 = 1.
- The 3rd component has node [2] with value [5]. Its XOR value is 5 = 5.

The score is the difference between the largest and smallest XOR value which is 10 - 1 = 9.
It can be shown that no other pair of removals will obtain a smaller score than 9.

Example 2:

![2322-2](https://assets.leetcode.com/uploads/2022/05/03/ex2drawio.png)

```text
Input: nums = [5,5,2,4,4,2], edges = [[0,1],[1,2],[5,2],[4,3],[1,3]]
Output: 0
Explanation: The diagram above shows a way to make a pair of removals.
```

- The 1st component has nodes [3,4] with values [4,4]. Its XOR value is 4  ^  4 = 0.
- The 2nd component has nodes [1,0] with values [5,5]. Its XOR value is 5  ^  5 = 0.
- The 3rd component has nodes [2,5] with values [2,2]. Its XOR value is 2  ^  2 = 0.

The score is the difference between the largest and smallest XOR value which is 0 - 0 = 0.
We cannot obtain a smaller score than 0.

__Constraints:__

- `n == nums.length`
- `3 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 8`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.

__题目描述:__

存在一棵无向连通树，树中有编号从 `0` 到 `n - 1` 的 `n` 个节点， 以及 `n - 1` 条边。

给你一个下标从 __0__ 开始的整数数组 `nums` ，长度为 `n` ，其中 `nums[i]` 表示第 `i` 个节点的值。另给你一个二维整数数组 `edges` ，长度为 `n - 1` ，其中 `edges[i] = [ai, bi]` 表示树中存在一条位于节点 `ai` 和 `bi` 之间的边。

删除树中两条 不同 的边以形成三个连通组件。对于一种删除边方案，定义如下步骤以计算其分数：

- 例如，三个组件的节点值分别是: `[4,5,7]`、 `[1,9]` 和 `[3,3,3]` 。三个异或值分别是 `4 ^ 5 ^ 7` = ___6___、 `1 ^ 9` = ___8___ 和 `3 ^ 3 ^ 3` = ___3___ 。最大异或值是 `8` ，最小异或值是 `3` ，分数是 `8 - 3 = 5` 。

返回在给定树上执行任意删除边方案可能的 最小 分数。

__示例:__

示例 1：

![2322-3](https://assets.leetcode.com/uploads/2022/05/03/ex1drawio.png)

```text
输入：nums = [1,5,5,4,11], edges = [[0,1],[1,2],[1,3],[3,4]]
输出：9
解释：上图展示了一种删除边方案。
```

- 第 1 个组件的节点是 [1,3,4] ，值是 [5,4,11] 。异或值是 5  ^  4  ^  11 = 10 。
- 第 2 个组件的节点是 [0] ，值是 [1] 。异或值是 1 = 1 。
- 第 3 个组件的节点是 [2] ，值是 [5] 。异或值是 5 = 5 。

分数是最大异或值和最小异或值的差值，10 - 1 = 9 。
可以证明不存在分数比 9 小的删除边方案。

示例 2：

![2322-4](https://assets.leetcode.com/uploads/2022/05/03/ex2drawio.png)

```text
输入：nums = [5,5,2,4,4,2], edges = [[0,1],[1,2],[5,2],[4,3],[1,3]]
输出：0
解释：上图展示了一种删除边方案。
```

- 第 1 个组件的节点是 [3,4] ，值是 [4,4] 。异或值是 4  ^  4 = 0 。
- 第 2 个组件的节点是 [1,0] ，值是 [5,5] 。异或值是 5  ^  5 = 0 。
- 第 3 个组件的节点是 [2,5] ，值是 [2,2] 。异或值是 2  ^  2 = 0 。

分数是最大异或值和最小异或值的差值，0 - 0 = 0 。
无法获得比 0 更小的分数 0 。

__提示：__

- `n == nums.length`
- `3 <= n <= 1000`
- `1 <= nums[i] <= 10 ^ 8`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵有效的树

__思路:__

```text
DFS
子树的异或值可以由总异或值和其他子树的异或值计算得到
计算树的总异或值 total
枚举根节点
枚举需要删除的边, 并记录以树的根节点为根节点的子树的异或值 xor1
接下来在以树的根节点为根节点的子树中枚举需要删除的边, 计算以该边为根节点的子树的异或值 cur = xor1 ^ dfs(nei, start, ban)
取 xor1 ^ total, xor1 ^ dfs2(nei, start, ban), dfs(nei, start, ban) 的最大值和最小值, 更新结果
total ^ xor1 对应删除第一条边时另一条子树的异或值
xor1 ^ dfs(nei, start, ban) 对应删除第二条边时另一条树的根节点为根节点的子树的异或值
dfs(nei, start, ban) 对应删除第二条边时另一条树的根节点为根节点的子树的异或值
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N), N 为节点数, 边数为 N - 1
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumScore(vector<int>& nums, vector<vector<int>>& edges) 
    {
        int n = nums.size();
        this -> nums = nums;
        total = accumulate(nums.begin(), nums.end(), 0, [](auto a, auto b){ return a ^ b; });
        for (const auto& edge : edges) 
        {
            graph[edge.front()].insert(edge.back());
            graph[edge.back()].insert(edge.front());
        }
        for (int i = 0; i < n; i++) 
        {
            for (int j : graph[i])
            {
                xor1 = dfs(i, -1, j);
                dfs2(i, -1, j);
            }
        }
        return result;
    }
private:
    int total, xor1, result = 0x3f3f3f3f;
    vector<int> nums;
    unordered_map<int, unordered_set<int>> graph;

    int dfs(int start, int fa, int ban) 
    {
        int cur = nums[start];
        for (int nei : graph[start]) if (nei != fa and nei != ban) cur ^= dfs(nei, start, ban);
        return cur;
    }

    int dfs2(int start, int fa, int ban) 
    {
        int cur = nums[start], x = 0;
        for (int nei : graph[start]) 
        {
            if (nei != fa and nei != ban) 
            {
                cur ^= (x = dfs2(nei, start, ban));
                result = min({result, max({total ^ xor1, xor1 ^ x, x}) - min({total ^ xor1, xor1 ^ x, x})});
            }
        }
        return cur;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Integer, Set<Integer>> graph = new HashMap<>();
    private int total, xor1, result = 0x3f3f3f3f, nums[];

    public int minimumScore(int[] nums, int[][] edges) {
        int n = nums.length;
        this.nums = nums;
        total = Arrays.stream(nums).reduce(0, (x, y) -> x ^ y);
        for (int i = 0; i < n; i++) graph.put(i, new HashSet<>());
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }
        for (int i = 0; i < n; i++) {
            for (int j : graph.get(i)) {
                xor1 = dfs(i, -1, j);
                dfs2(i, -1, j);
            }
        }
        return result;
    }

    private int dfs(int start, int fa, int ban) {
        int cur = nums[start];
        for (int nei : graph.get(start)) if (nei != fa && nei != ban) cur ^= dfs(nei, start, ban);
        return cur;
    }

    private int dfs2(int start, int fa, int ban) {
        int cur = nums[start], x = 0;
        for (int nei : graph.get(start)) {
            if (nei != fa && nei != ban) {
                cur ^= (x = dfs2(nei, start, ban));
                result = Math.min(result, Math.max(Math.max(total ^ xor1, xor1 ^ x), x) - Math.min(Math.min(total ^ xor1, xor1 ^ x), x));
            }
        }
        return cur;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumScore(self, nums: List[int], edges: List[List[int]]) -> int:
        n, graph, total, xor1, result = len(nums), defaultdict(set), reduce(operator.xor, nums), 0, inf
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)

        def dfs(start: int, fa: int, ban: int) -> int:
            cur = nums[start]
            for nei in graph[start]:
                if nei != fa and nei != ban:
                    cur ^= dfs(nei, start, ban)
            return cur

        def dfs2(start: int, fa: int, ban: int) -> int:
            cur = nums[start]
            nonlocal result
            for nei in graph[start]:
                if nei != fa and nei != ban:
                    cur ^= (x := dfs2(nei, start, ban))
                    result = min(result, max(xor1 ^ x, total ^ xor1, x) - min(xor1 ^ x, total ^ xor1, x))
            return cur

        for i in range(n):
            for j in graph[i]:
                xor1 = dfs(i, -1, j)
                dfs2(i, -1, j)
        return result
```
