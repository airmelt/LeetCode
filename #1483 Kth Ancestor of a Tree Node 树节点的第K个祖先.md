# 1483 Kth Ancestor of a Tree Node 树节点的第K个祖先

__Description:__

You are given a tree with `n` nodes numbered from `0` to `n - 1` in the form of a parent array `parent` where `parent[i]` is the parent of `i ^ th` node. The root of the tree is node `0`. Find the `k ^ th` ancestor of a given node.

The `k ^ th` ancestor of a tree node is the `k ^ th` node in the path from that node to the root node.

Implement the `TreeAncestor` class:

- `TreeAncestor(int n, int[] parent)` Initializes the object with the number of nodes in the tree and the parent array.
- `int getKthAncestor(int node, int k)` return the `k ^ th` ancestor of the given node `node`. If there is no such ancestor, return `-1`.

__Example:__

Example 1:

![1483-1](https://assets.leetcode.com/uploads/2019/08/28/1528_ex1.png)

```text
Input
["TreeAncestor", "getKthAncestor", "getKthAncestor", "getKthAncestor"]
[[7, [-1, 0, 0, 1, 1, 2, 2]], [3, 1], [5, 2], [6, 3]]
Output
[null, 1, 0, -1]

Explanation
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);
treeAncestor.getKthAncestor(3, 1); // returns 1 which is the parent of 3
treeAncestor.getKthAncestor(5, 2); // returns 0 which is the grandparent of 5
treeAncestor.getKthAncestor(6, 3); // returns -1 because there is no such ancestor
```

__Constraints:__

- `1 <= k <= n <= 5 * 10 ^ 4`
- `parent.length == n`
- `parent[0] == -1`
- `0 <= parent[i] < n` for all `0 < i < n`
- `0 <= node < n`
- There will be at most `5 * 10 ^ 4` queries.

__题目描述:__

给你一棵树，树上有 `n` 个节点，按从 `0` 到 `n-1` 编号。树以父节点数组的形式给出，其中 `parent[i]` 是节点 `i` 的父节点。树的根节点是编号为 `0` 的节点。

树节点的第 _`k`_ 个祖先节点是从该节点到根节点路径上的第 `k` 个节点。

实现 `TreeAncestor` 类：

- `TreeAncestor（int n， int[] parent）` 对树和父数组中的节点数初始化对象。
- `getKthAncestor` `(int node, int k)` 返回节点 `node` 的第 `k` 个祖先节点。如果不存在这样的祖先节点，返回 `-1` 。

__示例:__

示例 1：

![1483-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/06/14/1528_ex1.png)

```text
输入：
["TreeAncestor","getKthAncestor","getKthAncestor","getKthAncestor"]
[[7,[-1,0,0,1,1,2,2]],[3,1],[5,2],[6,3]]

输出：
[null,1,0,-1]

解释：
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);

treeAncestor.getKthAncestor(3, 1);  // 返回 1 ，它是 3 的父节点
treeAncestor.getKthAncestor(5, 2);  // 返回 0 ，它是 5 的祖父节点
treeAncestor.getKthAncestor(6, 3);  // 返回 -1 因为不存在满足要求的祖先节点
```

__提示：__

- `1 <= k <= n <= 5 * 10 ^ 4`
- `parent[0] == -1` 表示编号为 `0` 的节点是根节点。
- 对于所有的 `0 < i < n` ， `0 <= parent[i] < n` 总成立
- `0 <= node < n`
- 至多查询 `5 * 10 ^ 4` 次

__思路:__

```text
1. 动态规划倍增法
将 k 视为二进制 k = 2 ^ 0 * a0 + 2 ^ 1 * a1 + ... + 2 ^ m * am
那么可以在 O(logK) 的时间复杂度得到第 k 个祖先
设 dp[node][j] 为 node 结点的第 2 ^ j 个祖先
那么 dp[node][0] 即为 parent[node]
转移方程为 dp[node][j] = dp[dp[node][j - 1]][j - 1], 即递归查找结点 2 ^ (j - 1) 的 2 ^ (j - 1) 的祖先结点
注意在预处理和查找时找到 -1 都需要剪枝终止查找
其中 k ^= k & -k 表示将二进制 k 的最后一个 1 置 0
预处理时间复杂度为 O(NlogN), 查找时间复杂度 O(logK), 空间复杂度为 O(NlogN)
2. DFS ➕ BFS
记录从根结点到叶子结点到路径及深度
在路径上查询即可
预处理时间复杂度为 O(N ^ 2), 查找时间复杂度 O(1), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class TreeAncestor 
{
private:
    vector<vector<int>> dp;
public:
    TreeAncestor(int n, vector<int>& parent): dp(n)
    {
        for (int i = 0; i < n; i++) dp[i].push_back(parent[i]);
        for (int j = 1; (1 << j) <= n; j++) for (int i = 0; i < n; i++) dp[i].emplace_back(~dp[i].back() ? dp[dp[i].back()][j - 1] : -1);
    }
    
    int getKthAncestor(int node, int k) 
    {
        while (k and ~node)
        {
            node = dp[node][__builtin_ctz(k)];
            k ^= k & -k;
        }
        return node;
    }
};

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor* obj = new TreeAncestor(n, parent);
 * int param_1 = obj->getKthAncestor(node,k);
 */
```

__Java__:

```Java
class TreeAncestor {
    private List<Integer>[] dp;

    public TreeAncestor(int n, int[] parent) {
        dp = new List[n];
        for (int i = 0; i < n; i++) {
            dp[i] = new ArrayList<>();
            dp[i].add(parent[i]);
        }
        for (int j = 1; (1 << j) <= n; j++) for (int i = 0; i < n; i++) dp[i].add(dp[i].get(j - 1) == -1 ? -1 : dp[dp[i].get(j - 1)].get(j - 1));
    }
    
    public int getKthAncestor(int node, int k) {
        while (k != 0 && node != -1) {
            node = dp[node].get(count(k));
            k ^= (k & (-k));
        }
        return node;
    }
    
    private int count(int k) {
        int result = 0;
        while ((k & 1) != 1) {
            ++result;
            k >>= 1;
        }
        return result;
    }
}

/**
 * Your TreeAncestor object will be instantiated and called as such:
 * TreeAncestor obj = new TreeAncestor(n, parent);
 * int param_1 = obj.getKthAncestor(node,k);
 */
```

__Python__:

```Python
class TreeAncestor:

    def __init__(self, n: int, parent: List[int]):
        self.dp = [deque([i]) for i in parent]
        self.f = lambda k: 0 if not k or (k & 1) else self.f(k >> 1) + 1
        for j in range(1, int(log2(n + 0.5) + 1)):
            for i in range(n):
                self.dp[i].append(self.dp[self.dp[i][-1]][j - 1] if ~self.dp[i][-1] else -1)
            


    def getKthAncestor(self, node: int, k: int) -> int:
        while k and ~node:
            node = self.dp[node][self.f(k)]
            k ^= k & -k
        return node


# Your TreeAncestor object will be instantiated and called as such:
# obj = TreeAncestor(n, parent)
# param_1 = obj.getKthAncestor(node,k)
```
