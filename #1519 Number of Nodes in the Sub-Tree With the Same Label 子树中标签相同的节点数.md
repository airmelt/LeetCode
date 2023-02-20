# 1519 Number of Nodes in the Sub-Tree With the Same Label 子树中标签相同的节点数

__Description:__

You are given a tree (i.e. a connected, undirected graph that has no cycles) consisting of `n` nodes numbered from `0` to `n - 1` and exactly `n - 1` `edges`. The __root__ of the tree is the node `0`, and each node of the tree has __a label__ which is a lower-case character given in the string `labels` (i.e. The node with the number `i` has the label `labels[i]`).

The `edges` array is given on the form `edges[i] = [ai, bi]`, which means there is an edge between nodes `ai` and `bi` in the tree.

Return _an array of size `n`_ where `ans[i]` is the number of nodes in the subtree of the `i ^ th` node which have the same label as node `i`.

A subtree of a tree `T` is the tree consisting of a node in `T` and all of its descendant nodes.

__Example:__

Example 1:

![1519-1](https://assets.leetcode.com/uploads/2020/07/01/q3e1.jpg)

```text
Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = "abaedcd"
Output: [2,1,1,1,1,1,1]
Explanation: Node 0 has label 'a' and its sub-tree has node 2 with label 'a' as well, thus the answer is 2. Notice that any node is part of its sub-tree.
Node 1 has a label 'b'. The sub-tree of node 1 contains nodes 1,4 and 5, as nodes 4 and 5 have different labels than node 1, the answer is just 1 (the node itself).
```

Example 2:

![1519-2](https://assets.leetcode.com/uploads/2020/07/01/q3e2.jpg)

```text
Input: n = 4, edges = [[0,1],[1,2],[0,3]], labels = "bbbb"
Output: [4,2,1,1]
Explanation: The sub-tree of node 2 contains only node 2, so the answer is 1.
The sub-tree of node 3 contains only node 3, so the answer is 1.
The sub-tree of node 1 contains nodes 1 and 2, both have label 'b', thus the answer is 2.
The sub-tree of node 0 contains nodes 0, 1, 2 and 3, all with label 'b', thus the answer is 4.
```

Example 3:

![1519-3](https://assets.leetcode.com/uploads/2020/07/01/q3e3.jpg)

```text
Input: n = 5, edges = [[0,1],[0,2],[1,3],[0,4]], labels = "aabab"
Output: [3,2,1,1,1]
```

__Constraints:__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `labels.length == n`
- `labels` is consisting of only of lowercase English letters.

__题目描述:__

给你一棵树（即，一个连通的无环无向图），这棵树由编号从 `0` 到 `n - 1` 的 n 个节点组成，且恰好有 `n - 1` 条 `edges` 。树的根节点为节点 `0` ，树上的每一个节点都有一个标签，也就是字符串 `labels` 中的一个小写字符（编号为 `i` 的 节点的标签就是 `labels[i]` ）

边数组 `edges` 以 `edges[i] = [ai, bi]` 的形式给出，该格式表示节点 `ai` 和 `bi` 之间存在一条边。

返回一个大小为 _`n`_ 的数组，其中 `ans[i]` 表示第 `i` 个节点的子树中与节点 `i` 标签相同的节点数。

树 `T` 中的子树是由 `T` 中的某个节点及其所有后代节点组成的树。

__示例:__

示例 1：

![1519-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e1.jpg)

```text
输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], labels = "abaedcd"
输出：[2,1,1,1,1,1,1]
解释：节点 0 的标签为 'a' ，以 'a' 为根节点的子树中，节点 2 的标签也是 'a' ，因此答案为 2 。注意树中的每个节点都是这棵子树的一部分。
节点 1 的标签为 'b' ，节点 1 的子树包含节点 1、4 和 5，但是节点 4、5 的标签与节点 1 不同，故而答案为 1（即，该节点本身）。
```

示例 2：

![1519-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e2.jpg)

```text
输入：n = 4, edges = [[0,1],[1,2],[0,3]], labels = "bbbb"
输出：[4,2,1,1]
解释：节点 2 的子树中只有节点 2 ，所以答案为 1 。
节点 3 的子树中只有节点 3 ，所以答案为 1 。
节点 1 的子树中包含节点 1 和 2 ，标签都是 'b' ，因此答案为 2 。
节点 0 的子树中包含节点 0、1、2 和 3，标签都是 'b'，因此答案为 4 。
```

示例 3：

![1519-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/q3e3.jpg)

```text
输入：n = 5, edges = [[0,1],[0,2],[1,3],[0,4]], labels = "aabab"
输出：[3,2,1,1,1]
```

__提示：__

- `1 <= n <= 10 ^ 5`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `labels.length == n`
- `labels` 仅由小写英文字母组成

__思路:__

```text
DFS
将 edges 转换为无向图
对树进行深度优先遍历, 并记录 26 个英文字母出现的次数
深度优先遍历时只要不和父结点相同就可以继续搜索
最后按照每个结点的值返回子结点数目即可
时间复杂度为 O(N), 空间复杂度为 O(N), 另外只需要 26 个英文字母, 时间空间复杂度均为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> countSubTrees(int n, vector<vector<int>>& edges, string labels) 
    {
        vector<vector<int>> graph(n), count(n, vector<int>(26));
        vector<int> result(n);
        for (const auto& edge: edges)
        {
            graph[edge.front()].emplace_back(edge.back());
            graph[edge.back()].emplace_back(edge.front());
        }
        function<vector<int>(int, int)> dfs = [&] (int u, int p) 
        {
            vector<int> cnt(26);
            ++cnt[labels[u] - 'a'];
            for (const auto &v : graph[u]) 
            {
                if (v != p)
                {
                    auto child = dfs(v, u);
                    for (int i = 0; i < 26; i++) cnt[i] += child[i]; 
                }
            }
            result[u] = cnt[labels[u] - 'a'];
            return cnt; 
        };
        dfs(0, -1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] countSubTrees(int n, int[][] edges, String labels) {
        int result[] = new int[n], count[][] = new int[n][26];
        List<Integer>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
        for (int[] edge: edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }
        dfs(0, -1, count, labels, graph);
        for (int i = 0; i < n; i++) result[i] = count[i][labels.charAt(i) - 'a'];
        return result;
    }
    
    private void dfs(int u, int p, int[][] count, String labels, List<Integer>[] graph) {
        count[u][labels.charAt(u) - 'a'] = 1;
        for (int v: graph[u]) {
            if (v != p) {
                dfs(v, u, count, labels, graph);
                for (int i = 0; i < 26; i++) count[u][i] += count[v][i];
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def countSubTrees(self, n: int, edges: List[List[int]], labels: str) -> List[int]:
        graph, count = defaultdict(list), [[0] * 26 for _ in range(n)]
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        
        def dfs(u: int, p: int) -> NoReturn:
            count[u][ord(labels[u]) - ord('a')] = 1
            for v in graph[u]:
                if v != p:
                    dfs(v, u)
                    for i in range(26):
                        count[u][i] += count[v][i]
        dfs(0, -1)
        return [count[i][ord(labels[i]) - ord('a')] for i in range(n)]
```
