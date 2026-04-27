# 2467 Most Profitable Path in a Tree 树上最大得分和路径

__Description:__

There is an undirected tree with `n` nodes labeled from `0` to `n - 1`, rooted at node `0`. You are given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree.

At every node `i`, there is a gate. You are also given an array of even integers `amount`, where `amount[i]` represents:

- the price needed to open the gate at node `i`, if `amount[i]` is negative, or,
- the cash reward obtained on opening the gate at node `i`, otherwise.

The game goes on as follows:

- Initially, Alice is at node `0` and Bob is at node `bob`.
- At every second, Alice and Bob _each_ move to an adjacent node. Alice moves towards some __leaf node__, while Bob moves towards node `0`.
- For __every__ node along their path, Alice and Bob either spend money to open the gate at that node, or accept the reward. Note that:
  - If the gate is __already open__, no price will be required, nor will there be any cash reward.
  - If Alice and Bob reach the node __simultaneously__, they share the price/reward for opening the gate there. In other words, if the price to open the gate is `c`, then both Alice and Bob pay `c / 2` each. Similarly, if the reward at the gate is `c`, both of them receive `c / 2` each.
- If Alice reaches a leaf node, she stops moving. Similarly, if Bob reaches node `0`, he stops moving. Note that these events are __independent__ of each other.

Return the maximum net income Alice can have if she travels towards the optimal leaf node.

__Example:__

Example 1:

![2467-1](https://assets.leetcode.com/uploads/2022/10/29/eg1.png)

```text
Input: edges = [[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6]
Output: 6
Explanation: 
The above diagram represents the given tree. The game goes as follows:
```

- Alice is initially on node 0, Bob on node 3. They open the gates of their respective nodes.
  Alice's net income is now -2.
- Both Alice and Bob move to node 1.
  Since they reach here simultaneously, they open the gate together and share the reward.
  Alice's net income becomes -2 + (4 / 2) = 0.
- Alice moves on to node 3. Since Bob already opened its gate, Alice's income remains unchanged.
  Bob moves on to node 0, and stops moving.
- Alice moves on to node 4 and opens the gate there. Her net income becomes 0 + 6 = 6.

Now, neither Alice nor Bob can make any further moves, and the game ends.

It is not possible for Alice to get a higher net income.

Example 2:

![2467-2](https://assets.leetcode.com/uploads/2022/10/29/eg2.png)

```text
Input: edges = [[0,1]], bob = 1, amount = [-7280,2350]
Output: -7280
Explanation: 
Alice follows the path 0->1 whereas Bob follows the path 1->0.
Thus, Alice opens the gate at node 0 only. Hence, her net income is -7280.
```

__Constraints:__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.
- `1 <= bob < n`
- `amount.length == n`
- `amount[i]` is an __even__ integer in the range `[-10 ^ 4, 10 ^ 4]`.

__题目描述:__

一个 `n` 个节点的无向树，节点编号为 `0` 到 `n - 1` ，树的根结点是 `0` 号节点。给你一个长度为 `n - 1` 的二维整数数组 `edges` ，其中 `edges[i] = [ai, bi]` ，表示节点 `ai` 和 `bi` 在树中有一条边。

在每一个节点 `i` 处有一扇门。同时给你一个都是偶数的数组 `amount` ，其中 `amount[i]` 表示:

- 如果 `amount[i]` 的值是负数，那么它表示打开节点 `i` 处门扣除的分数。
- 如果 `amount[i]` 的值是正数，那么它表示打开节点 `i` 处门加上的分数。

游戏按照如下规则进行：

- 一开始，Alice 在节点 `0` 处，Bob 在节点 `bob` 处。
- 每一秒钟，Alice 和 Bob __分别__ 移动到相邻的节点。Alice 朝着某个 __叶子结点__ 移动，Bob 朝着节点 `0` 移动。
- 对于他们之间路径上的 __每一个__ 节点，Alice 和 Bob 要么打开门并扣分，要么打开门并加分。注意:
  - 如果门 __已经打开__ （被另一个人打开），不会有额外加分也不会扣分。
  - 如果 Alice 和 Bob __同时__ 到达一个节点，他们会共享这个节点的加分或者扣分。换言之，如果打开这扇门扣 `c` 分，那么 Alice 和 Bob 分别扣 `c / 2` 分。如果这扇门的加分为 `c` ，那么他们分别加 `c / 2` 分。
- 如果 Alice 到达了一个叶子结点，她会停止移动。类似的，如果 Bob 到达了节点 `0` ，他也会停止移动。注意这些事件互相 __独立__ ，不会影响另一方移动。

请你返回 Alice 朝最优叶子结点移动的 最大 净得分。

__示例:__

示例 1：

![2467-3](https://assets.leetcode.com/uploads/2022/10/29/eg1.png)

```text
输入：edges = [[0,1],[1,2],[1,3],[3,4]], bob = 3, amount = [-2,4,2,-4,6]
输出：6
解释：
上图展示了输入给出的一棵树。游戏进行如下：
```

- Alice 一开始在节点 0 处，Bob 在节点 3 处。他们分别打开所在节点的门。
  Alice 得分为 -2 。
- Alice 和 Bob 都移动到节点 1 。
  因为他们同时到达这个节点，他们一起打开门并平分得分。
  Alice 的得分变为 -2 + (4 / 2) = 0 。
- Alice 移动到节点 3 。因为 Bob 已经打开了这扇门，Alice 得分不变。
  Bob 移动到节点 0 ，并停止移动。
- Alice 移动到节点 4 并打开这个节点的门，她得分变为 0 + 6 = 6 。

现在，Alice 和 Bob 都不能进行任何移动了，所以游戏结束。

Alice 无法得到更高分数。

示例 2：

![2467-4](https://assets.leetcode.com/uploads/2022/10/29/eg2.png)

```text
输入：edges = [[0,1]], bob = 1, amount = [-7280,2350]
输出：-7280
解释：
Alice 按照路径 0->1 移动，同时 Bob 按照路径 1->0 移动。
所以 Alice 只打开节点 0 处的门，她的得分为 -7280 。
```

__提示：__

- `2 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` 表示一棵有效的树。
- `1 <= bob < n`
- `amount.length == n`
- `amount[i]` 是范围 `[-10 ^ 4, 10 ^ 4]` 之间的一个 __偶数__ 。

__思路:__

```text
DFS
第一次 DFS 从 bob 出发找到 bob 到达每个节点的时间 t, 记录在 bob_time 数组中
只有从 u 出发能够到达 0 时，才会更新 bob_time[u] = t
比如图 1 中从 3 往 4 的路径不会到达 0, bob_time[4] 不会更新
初始化 bob_time 数组为 n 或者 INF, 代表每个节点都无法到达 0
第二次 DFS 从 0 出发, 计算 Alice 的得分, 记录当前时间 t 和当前得分 cur
如果 t < bob_time[u], Alice 到达 u 时 bob 还没有到达, Alice 得分 += amount[u]
如果 t == bob_time[u], Alice 到达 u 时 bob 也到达了, Alice 得分 += amount[u] / 2
如果 u 是叶子节点, 更新 result
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int mostProfitablePath(vector<vector<int>>& edges, int bob, vector<int>& amount) 
    {
        unordered_map<int, unordered_set<int>> graph;
        int n = edges.size() + 1, result = INT_MIN;
        vector<int> bob_time(n, n);
        for (const auto& edge : edges) 
        {
            graph[edge.front()].insert(edge.back());
            graph[edge.back()].insert(edge.front());
        }
        function<bool(int, int, int)> dfs_bob = [&](int u, int fa, int t) -> bool
        {
            if (!u) return bob_time[u] = t;
            for (const auto& v : graph[u]) if (v != fa and dfs_bob(v, u, t + 1)) return bob_time[u] = t;
            return false;
        };
        dfs_bob(bob, -1, 0);
        graph[0].insert(-1);
        function<void(int, int, int, int)> dfs_alice = [&](int u, int fa, int t, int cur) -> void
        {
            if (t < bob_time[u]) cur += amount[u];
            else if (t == bob_time[u]) cur += amount[u] >> 1;
            if (graph[u].size() == 1) result = max(result, cur);
            for (const auto& v : graph[u]) if (v != fa) dfs_alice(v, u, t + 1, cur);
        };
        dfs_alice(0, -1, 0, 0);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private Map<Integer, Set<Integer>> graph = new HashMap<>();
    private int n, result = Integer.MIN_VALUE, bobTime[], amount[];

    public int mostProfitablePath(int[][] edges, int bob, int[] amount) {
        this.amount = amount;
        n = edges.length + 1;
        bobTime = new int[n];
        Arrays.fill(bobTime, n);
        for (int[] edge : edges) {
            graph.computeIfAbsent(edge[0], k -> new HashSet<>()).add(edge[1]);
            graph.computeIfAbsent(edge[1], k -> new HashSet<>()).add(edge[0]);
        }
        dfsBob(bob, -1, 0);
        graph.get(0).add(-1);
        dfsAlice(0, -1, 0, 0);
        return result;
    }

    private boolean dfsBob(int u, int fa, int t) {
        if (u == 0) {
            bobTime[u] = t;
            return true;
        }
        for (int v : graph.get(u)) {
            if (v != fa && dfsBob(v, u, t + 1)) {
                bobTime[u] = t;
                return true;
            }
        }
        return false;
    }

    private void dfsAlice(int u, int fa, int t, int cur) {
        if (t < bobTime[u]) cur += amount[u];
        else if (t == bobTime[u]) cur += amount[u] >> 1;
        if (graph.get(u).size() == 1) result = Math.max(result, cur);
        for (int v : graph.get(u)) if (v != fa) dfsAlice(v, u, t + 1, cur);
    }
}
```

__Python__:

```Python
class Solution:
    def mostProfitablePath(self, edges: List[List[int]], bob: int, amount: List[int]) -> int:
        bob_time, graph, result = [inf] * (n := len(edges) + 1), defaultdict(set), -inf
        for u, v in edges:
            graph[u].add(v)
            graph[v].add(u)
        
        def dfs_bob(u: int, fa: int, t: int) -> bool:
            if not u:
                bob_time[u] = t
                return True
            for v in graph[u]:
                if v != fa and dfs_bob(v, u, t + 1):
                    bob_time[u] = t
                    return True
            return False
        dfs_bob(bob, -1, 0)
        graph[0].add(-1)
        
        def dfs_alice(u: int, fa: int, t: int, cur: int) -> NoReturn:
            if t < bob_time[u]:
                cur += amount[u]
            elif t == bob_time[u]:
                cur += amount[u] >> 1
            if len(graph[u]) == 1:
                nonlocal result
                result = max(result, cur)
            for v in graph[u]:
                if v != fa:
                    dfs_alice(v, u, t + 1, cur)
        dfs_alice(0, -1, 0, 0)
        return result
```
