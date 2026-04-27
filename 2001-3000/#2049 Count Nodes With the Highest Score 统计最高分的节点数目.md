# 2049 Count Nodes With the Highest Score 统计最高分的节点数目

__Description:__

There is a __binary__ tree rooted at `0` consisting of `n` nodes. The nodes are labeled from `0` to `n - 1`. You are given a __0-indexed__ integer array `parents` representing the tree, where `parents[i]` is the parent of node `i`. Since node `0` is the root, `parents[0] == -1`.

Each node has a score. To find the score of a node, consider if the node and the edges connected to it were removed. The tree would become one or more non-empty subtrees. The size of a subtree is the number of the nodes in it. The score of the node is the product of the sizes of all those subtrees.

Return the number of nodes that have the highest score.

__Example:__

Example 1:

![2049-1](https://assets.leetcode.com/uploads/2021/10/03/example-1.png)

```text
Input: parents = [-1,2,0,2,0]
Output: 3
Explanation:
```

- The score of node 0 is: 3 \* 1 = 3
- The score of node 1 is: 4 = 4
- The score of node 2 is: 1 \* 1 \* 2 = 2
- The score of node 3 is: 4 = 4
- The score of node 4 is: 4 = 4

The highest score is 4, and three nodes (node 1, node 3, and node 4) have the highest score.

Example 2:

![2049-2](https://assets.leetcode.com/uploads/2021/10/03/example-2.png)

```text
Input: parents = [-1,2,0]
Output: 2
Explanation:
```

- The score of node 0 is: 2 = 2
- The score of node 1 is: 2 = 2
- The score of node 2 is: 1 \* 1 = 1

The highest score is 2, and two nodes (node 0 and node 1) have the highest score.

__Constraints:__

- `n == parents.length`
- `2 <= n <= 10 ^ 5`
- `parents[0] == -1`
- `0 <= parents[i] <= n - 1` for `i != 0`
- `parents` represents a valid binary tree.

__题目描述:__

给你一棵根节点为 `0` 的 __二叉树__ ，它总共有 `n` 个节点，节点编号为 `0` 到 `n - 1` 。同时给你一个下标从 __0__ 开始的整数数组 `parents` 表示这棵树，其中 `parents[i]` 是节点 `i` 的父节点。由于节点 `0` 是根，所以 `parents[0] == -1` 。

一个子树的 大小 为这个子树内节点的数目。每个节点都有一个与之关联的 分数 。求出某个节点分数的方法是，将这个节点和与它相连的边全部 删除 ，剩余部分是若干个 非空 子树，这个节点的 分数 为所有这些子树 大小的乘积 。

请你返回有 最高得分 节点的 数目 。

__示例:__

示例 1:

![2049-3](https://assets.leetcode.com/uploads/2021/10/03/example-1.png)

```text
输入：parents = [-1,2,0,2,0]
输出：3
解释：
```

- 节点 0 的分数为：3 \* 1 = 3
- 节点 1 的分数为：4 = 4
- 节点 2 的分数为：1 \* 1 \* 2 = 2
- 节点 3 的分数为：4 = 4
- 节点 4 的分数为：4 = 4

最高得分为 4 ，有三个节点得分为 4 （分别是节点 1，3 和 4 ）。

示例 2：

![2049-4](https://assets.leetcode.com/uploads/2021/10/03/example-2.png)

```text
输入：parents = [-1,2,0]
输出：2
解释：
```

- 节点 0 的分数为：2 = 2
- 节点 1 的分数为：2 = 2
- 节点 2 的分数为：1 \* 1 = 1

最高分数为 2 ，有两个节点分数为 2 （分别为节点 0 和 1 ）。

__提示：__

- `n == parents.length`
- `2 <= n <= 10 ^ 5`
- `parents[0] == -1`
- 对于 `i != 0` ，有 `0 <= parents[i] <= n - 1`
- `parents` 表示一棵二叉树。

__思路:__

```text
DFS
如果为根节点, 分数为两个子树大小的乘积
其他节点, 最多可以分为 3 个子树的乘积
使用 DFS 记录每个子树的大小, 以及最大的分数
在计算分数的过程中, 记录最大的分数和节点数
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countHighestScoreNodes(vector<int>& parents) 
    {
        n = parents.size();
        graph.resize(n);
        for (int i = 0; i < n; i++) if (parents[i] != -1) graph[parents[i]].emplace_back(i);
        dfs(0);
        return c;
    }
private:
    long long max_value;
    int n;
    int c;
    vector<vector<int>> graph;

    int dfs(int u) 
    {
        long score = 1, sz = n - 1;
        for (const auto& v : graph[u])
        {
            int child = dfs(v);
            score *= child;
            sz -= child;
        }
        if (u) score *= sz;
        if (score == max_value) ++c;
        else if (score > max_value)
        {
            max_value = score;
            c = 1;
        }
        return n - sz;
    }
};
```

__Java__:

```Java
class Solution {
    private long maxValue = 0;
    private int count = 0;
    private int n;
    private List<List<Integer>> graph = new ArrayList<>();

    public int countHighestScoreNodes(int[] parents) {
        n = parents.length;
        for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
        for (int i = 0; i < n; i++) if (parents[i] != -1) graph.get(parents[i]).add(i);
        dfs(0);
        return count;
    }

    public int dfs(int u) {
        long score = 1;
        int size = n - 1;
        for (int v : graph.get(u)) {
            int child = dfs(v);
            score *= child;
            size -= child;
        }
        if (u != 0) score *= size;
        if (score == maxValue) ++count;
        else if (score > maxValue) {
            maxValue = score;
            count = 1;
        }
        return n - size;
    }
}
```

__Python__:

```Python
class Solution:
    def countHighestScoreNodes(self, parents: List[int]) -> int:
        n, graph, max_value, count = len(parents), defaultdict(set), 0, 0
        for cur, p in enumerate(parents):
            if p != -1:
                graph[p].add(cur)
        
        def dfs(u: int) -> int:
            score, size = 1, n - 1
            for v in graph[u]:
                child = dfs(v)
                score *= child
                size -= child
            if u:
                score *= size
            nonlocal max_value, count
            if score == max_value:
                count += 1
            elif score > max_value:
                max_value, count = score, 1
            return n - size
        dfs(0)
        return count
```
