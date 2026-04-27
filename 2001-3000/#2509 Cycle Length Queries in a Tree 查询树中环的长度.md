# 2509 Cycle Length Queries in a Tree 查询树中环的长度

__Description:__

You are given an integer `n`. There is a __complete binary tree__ with `2 ^ n - 1` nodes. The root of that tree is the node with the value `1`, and every node with a value `val` in the range `[1, 2 ^ n - 1 - 1]` has two children where:

- The left node has the value `2 * val`, and
- The right node has the value `2 * val + 1`.

You are also given a 2D integer array `queries` of length `m`, where `queries[i] = [ai, bi]`. For each query, solve the following problem:

Note that:

- A __cycle__ is a path that starts and ends at the same node, and each edge in the path is visited only once.
- The length of a cycle is the number of edges visited in the cycle.
- There could be multiple edges between two nodes in the tree after adding the edge of the query.

Return _an array_ `answer` _of length_ `m` _where_ `answer[i]` _is the answer to the_ `i ^ th` _query._

__Example:__

Example 1:

![2509-1](https://assets.leetcode.com/uploads/2022/10/25/bexample1.png)

```text
Input: n = 3, queries = [[5,3],[4,7],[2,3]]
Output: [4,5,3]
Explanation: The diagrams above show the tree of 23 - 1 nodes. Nodes colored in red describe the nodes in the cycle after adding the edge.
- After adding the edge between nodes 3 and 5, the graph contains a cycle of nodes [5,2,1,3]. Thus answer to the first query is 4. We delete the added edge and process the next query.
- After adding the edge between nodes 4 and 7, the graph contains a cycle of nodes [4,2,1,3,7]. Thus answer to the second query is 5. We delete the added edge and process the next query.
- After adding the edge between nodes 2 and 3, the graph contains a cycle of nodes [2,1,3]. Thus answer to the third query is 3. We delete the added edge.
```

Example 2:

![2509-2](https://assets.leetcode.com/uploads/2022/10/25/aexample2.png)

```text
Input: n = 2, queries = [[1,2]]
Output: [2]
Explanation: The diagram above shows the tree of 22 - 1 nodes. Nodes colored in red describe the nodes in the cycle after adding the edge.
- After adding the edge between nodes 1 and 2, the graph contains a cycle of nodes [2,1]. Thus answer for the first query is 2. We delete the added edge.
```

__Constraints:__

- `2 <= n <= 30`
- `m == queries.length`
- `1 <= m <= 10 ^ 5`
- `queries[i].length == 2`
- `1 <= ai, bi <= 2 ^ n - 1`
- `ai != bi`

__题目描述:__

给你一个整数 `n` ，表示你有一棵含有 `2 ^ n - 1` 个节点的 __完全二叉树__ 。根节点的编号是 `1` ，树中编号在 `[1, 2 ^ n - 1 - 1]` 之间，编号为 `val` 的节点都有两个子节点，满足:

- 左子节点的编号为 `2 * val`
- 右子节点的编号为 `2 * val + 1`

给你一个长度为 `m` 的查询数组 `queries` ，它是一个二维整数数组，其中 `queries[i] = [ai, bi]` 。对于每个查询，求出以下问题的解:

注意：

- __环__ 是开始和结束于同一节点的一条路径，路径中每条边都只会被访问一次。
- 环的长度是环中边的数目。
- 在树中添加额外的边后，两个点之间可能会有多条边。

请你返回一个长度为 `m` 的数组 `answer` ，其中 `answer[i]` 是第 `i` 个查询的结果_。_

__示例:__

示例 1：

![2509-3](https://assets.leetcode.com/uploads/2022/10/25/bexample1.png)

```text
输入：n = 3, queries = [[5,3],[4,7],[2,3]]
输出：[4,5,3]
解释：上图是一棵有 23 - 1 个节点的树。红色节点表示添加额外边后形成环的节点。
- 在节点 3 和节点 5 之间添加边后，环为 [5,2,1,3] ，所以第一个查询的结果是 4 。删掉添加的边后处理下一个查询。
- 在节点 4 和节点 7 之间添加边后，环为 [4,2,1,3,7] ，所以第二个查询的结果是 5 。删掉添加的边后处理下一个查询。
- 在节点 2 和节点 3 之间添加边后，环为 [2,1,3] ，所以第三个查询的结果是 3 。删掉添加的边。
```

示例 2：

![2509-4](https://assets.leetcode.com/uploads/2022/10/25/aexample2.png)

```text
输入：n = 2, queries = [[1,2]]
输出：[2]
解释：上图是一棵有 22 - 1 个节点的树。红色节点表示添加额外边后形成环的节点。
- 在节点 1 和节点 2 之间添加边后，环为 [2,1] ，所以第一个查询的结果是 2 。删掉添加的边。
```

__提示：__

- `2 <= n <= 30`
- `m == queries.length`
- `1 <= m <= 10 ^ 5`
- `queries[i].length == 2`
- `1 <= ai, bi <= 2 ^ n - 1`
- `ai != bi`

__思路:__

```text
LCA ➕ 位运算
找到 LCA 则环的长度为
两个节点到 LCA 的距离之和 + 1
先将两个节点去向同一行
保证 a <= b
设 d = b.bit_length() - a.bit_length()
将 b 右移 d 位
这时 b 与 a 在同一行
计算 a 和 b 的异或
异或的长度就是 LCA 到 a 的距离 k
最终为 d + (k << 1) + 1
时间复杂度为 O(M), 空间复杂度为 O(1), M 为查询的数量, 与 N 无关
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>>& queries) 
    {
        vector<int> result;
        for (int i = 0, m = queries.size(); i < m; i++) result.emplace_back(abs(__builtin_clz(queries[i][1]) - __builtin_clz(queries[i][0])) + (!(min(queries[i][0], queries[i][1]) ^ (max(queries[i][0], queries[i][1]) >> (abs(__builtin_clz(queries[i][1]) - __builtin_clz(queries[i][0]))))) ? 0 : (32 - (__builtin_clz(min(queries[i][0], queries[i][1]) ^ (max(queries[i][0], queries[i][1]) >> (abs(__builtin_clz(queries[i][1]) - __builtin_clz(queries[i][0])))))) << 1)) + 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] cycleLengthQueries(int n, int[][] queries) {
        return Arrays.stream(queries).map(query -> Math.abs(Integer.numberOfLeadingZeros(query[1]) - Integer.numberOfLeadingZeros(query[0])) + (32 - Integer.numberOfLeadingZeros(Math.min(query[0], query[1]) ^ (Math.max(query[0], query[1]) >> (Math.abs(Integer.numberOfLeadingZeros(query[1]) - Integer.numberOfLeadingZeros(query[0]))))) << 1) + 1).mapToInt(i -> i).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def cycleLengthQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        return [abs(b.bit_length() - a.bit_length()) + ((min(a, b) ^ (max(a, b) >> (abs(b.bit_length() - a.bit_length())))).bit_length() << 1) + 1 for i, (a, b) in enumerate(queries)]
```
