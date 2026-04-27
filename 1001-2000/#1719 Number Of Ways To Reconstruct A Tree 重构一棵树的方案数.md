# 1719 Number Of Ways To Reconstruct A Tree 重构一棵树的方案数

__Description:__

You are given an array `pairs`, where `pairs[i] = [xi, yi]`, and:

- There are no duplicates.
- `xi < yi`

Let `ways` be the number of rooted trees that satisfy the following conditions:

- The tree consists of nodes whose values appeared in `pairs`.
- A pair `[xi, yi]` exists in `pairs` __if and only if__ `xi` is an ancestor of `yi` or `yi` is an ancestor of `xi`.
- __Note:__ the tree does not have to be a binary tree.

Two ways are considered to be different if there is at least one node that has different parents in both ways.

Return:

- `0` if `ways == 0`
- `1` if `ways == 1`
- `2` if `ways > 1`

A rooted tree is a tree that has a single root node, and all edges are oriented to be outgoing from the root.

An ancestor of a node is any node on the path from the root to that node (excluding the node itself). The root has no ancestors.

__Example:__

Example 1:

![1719-1](https://assets.leetcode.com/uploads/2020/12/03/trees2.png)

```text
Input: pairs = [[1,2],[2,3]]
Output: 1
Explanation: There is exactly one valid rooted tree, which is shown in the above figure.
```

Example 2:

![1719-2](https://assets.leetcode.com/uploads/2020/12/03/tree.png)

```text
Input: pairs = [[1,2],[2,3],[1,3]]
Output: 2
Explanation: There are multiple valid rooted trees. Three of them are shown in the above figures.
```

Example 3:

```text
Input: pairs = [[1,2],[2,3],[2,4],[1,5]]
Output: 0
Explanation: There are no valid rooted trees.
```

__Constraints:__

- `1 <= pairs.length <= 10 ^ 5`
- `1 <= xi < yi <= 500`
- The elements in `pairs` are unique.

__题目描述:__

给你一个数组 `pairs` ，其中 `pairs[i] = [xi, yi]` ，并且满足:

- `pairs` 中没有重复元素
- `xi < yi`

令 `ways` 为满足下面条件的有根树的方案数:

- 树所包含的所有节点值都在 `pairs` 中。
- 一个数对 `[xi, yi]` 出现在 `pairs` 中 __当且仅当__ `xi` 是 `yi` 的祖先或者 `yi` 是 `xi` 的祖先。
- __注意:__构造出来的树不一定是二叉树。

两棵树被视为不同的方案当存在至少一个节点在两棵树中有不同的父节点。

请你返回：

- 如果 `ways == 0` ，返回 `0` 。
- 如果 `ways == 1` ，返回 `1` 。
- 如果 `ways > 1` ，返回 `2` 。

一棵 有根树 指的是只有一个根节点的树，所有边都是从根往外的方向。

我们称从根到一个节点路径上的任意一个节点（除去节点本身）都是该节点的 祖先 。根节点没有祖先。

__示例:__

示例 1：

![1719-3](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/09/trees2.png)

```text
输入：pairs = [[1,2],[2,3]]
输出：1
解释：如上图所示，有且只有一个符合规定的有根树。
```

示例 2：

![1719-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/09/tree.png)

```text
输入：pairs = [[1,2],[2,3],[1,3]]
输出：2
解释：有多个符合规定的有根树，其中三个如上图所示。
```

示例 3：

```text
输入：pairs = [[1,2],[2,3],[2,4],[1,5]]
输出：0
解释：没有符合规定的有根树。
```

__提示：__

- `1 <= pairs.length <= 10 ^ 5`
- `1 <= xi < yi <= 500`
- `pairs` 中的元素互不相同。

__思路:__

```text
模拟
将 pairs 中的关系转化为图
先检验图中是否有唯一的根结点, 根结点的度为 N - 1
然后检验每个结点的父结点, 父结点的度大于等于当前结点的度, 且父结点是相邻结点中度最小的结点
如果没有父结点或父结点的相邻结点不包含当前结点, 则返回 0
否则检测父结点是否和当前结点的相邻结点相同, 如果相同则返回 2, 此时说明交换两个结点仍然能构成一颗合法的树
时间复杂度为 O(N ^ 2 + M), 空间复杂度为 O(M), 其中 M 为 pairs 的长度
, N 为结点数
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int checkWays(vector<vector<int>>& pairs) 
    {
        unordered_map<int, unordered_set<int>> graph;
        for (const auto &p : pairs) 
        {
            graph[p.front()].emplace(p.back());
            graph[p.back()].emplace(p.front());
        }
        int root = -1, result = 1;
        for (const auto &[node, neighbours] : graph) if (neighbours.size() == graph.size() - 1) root = node;
        if (root == -1) return 0;
        for (const auto &[node, neighbours] : graph) 
        {
            if (node == root) continue;
            int cur_degree = neighbours.size(), parent = -1, parent_degree = INT_MAX;
            for (const auto &neighbour : neighbours) 
            {
                if (graph[neighbour].size() < parent_degree and graph[neighbour].size() >= cur_degree) 
                {
                    parent = neighbour;
                    parent_degree = graph[neighbour].size();
                }
            }
            if (parent == -1) return 0;
            for (const auto &neighbour : neighbours) 
            {
                if (neighbour == parent) continue;
                if (!graph[parent].count(neighbour)) return 0;
            }
            if (parent_degree == cur_degree) result = 2;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int checkWays(int[][] pairs) {
        Map<Integer, Set<Integer>> graph = new HashMap<Integer, Set<Integer>>();
        for (int[] p : pairs) {
            graph.putIfAbsent(p[0], new HashSet<Integer>());
            graph.putIfAbsent(p[1], new HashSet<Integer>());
            graph.get(p[0]).add(p[1]);
            graph.get(p[1]).add(p[0]);
        }
        int root = -1, result = 1;
        for (Map.Entry<Integer, Set<Integer>> entry : graph.entrySet()) if (entry.getValue().size() == graph.size() - 1) root = entry.getKey();
        if (root == -1) return 0;
        for (Map.Entry<Integer, Set<Integer>> entry : graph.entrySet()) {
            Set<Integer> neighbours = entry.getValue();
            int cur = entry.getKey(), curDegree = neighbours.size(), parent = -1, parentDegree = Integer.MAX_VALUE;
            if (cur == root) continue;
            for (int neighbour : neighbours) {
                if (graph.get(neighbour).size() < parentDegree && graph.get(neighbour).size() >= curDegree) {
                    parent = neighbour;
                    parentDegree = graph.get(neighbour).size();
                }
            }
            if (parent == -1) return 0;
            for (int neighbour : neighbours) {
                if (neighbour == parent) continue;
                if (!graph.get(parent).contains(neighbour)) return 0;
            }
            if (parentDegree == curDegree) result = 2;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def checkWays(self, pairs: List[List[int]]) -> int:
        graph, result = defaultdict(set), 1
        for x, y in pairs:
            graph[x].add(y)
            graph[y].add(x)
        if (root := next((node for node, neighbours in graph.items() if len(neighbours) == len(graph) - 1), -1)) == -1:
            return 0
        for cur, neighbours in graph.items():
            if cur == root:
                continue
            cur_degree, parent, parent_degree = len(neighbours), -1, float('inf')
            for neighbour in neighbours:
                if cur_degree <= len(graph[neighbour]) < parent_degree:
                    parent, parent_degree = neighbour, len(graph[neighbour])
            if parent == -1 or any(neighbour != parent and neighbour not in graph[parent] for neighbour in neighbours):
                return 0
            if parent_degree == cur_degree:
                result = 2
        return result
```
