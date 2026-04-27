# 2374 Node With Highest Edge Score 边积分最高的节点

__Description:__

You are given a directed graph with `n` nodes labeled from `0` to `n - 1`, where each node has __exactly one__ outgoing edge.

The graph is represented by a given __0-indexed__ integer array `edges` of length `n`, where `edges[i]` indicates that there is a __directed__ edge from node `i` to node `edges[i]`.

The __edge score__ of a node `i` is defined as the sum of the __labels__ of all the nodes that have an edge pointing to `i`.

Return the node with the highest edge score. If multiple nodes have the same edge score, return the node with the smallest index.

__Example:__

Example 1:

![2374-1](https://assets.leetcode.com/uploads/2022/06/20/image-20220620195403-1.png)

```text
Input: edges = [1,0,0,0,0,7,7,5]
Output: 7
Explanation:
```

- The nodes 1, 2, 3 and 4 have an edge pointing to node 0. The edge score of node 0 is 1 + 2 + 3 + 4 = 10.
- The node 0 has an edge pointing to node 1. The edge score of node 1 is 0.
- The node 7 has an edge pointing to node 5. The edge score of node 5 is 7.
- The nodes 5 and 6 have an edge pointing to node 7. The edge score of node 7 is 5 + 6 = 11.

Node 7 has the highest edge score so return 7.

Example 2:

![2374-2](https://assets.leetcode.com/uploads/2022/06/20/image-20220620200212-3.png)

```text
Input: edges = [2,0,0,2]
Output: 0
Explanation:
```

- The nodes 1 and 2 have an edge pointing to node 0. The edge score of node 0 is 1 + 2 = 3.
- The nodes 0 and 3 have an edge pointing to node 2. The edge score of node 2 is 0 + 3 = 3.

Nodes 0 and 2 both have an edge score of 3. Since node 0 has a smaller index, we return 0.

__Constraints:__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `0 <= edges[i] < n`
- `edges[i] != i`

__题目描述:__

给你一个有向图，图中有 `n` 个节点，节点编号从 `0` 到 `n - 1` ，其中每个节点都 __恰有一条__ 出边。

图由一个下标从 __0__ 开始、长度为 `n` 的整数数组 `edges` 表示，其中 `edges[i]` 表示存在一条从节点 `i` 到节点 `edges[i]` 的 __有向__ 边。

节点 `i` 的 __边积分__ 定义为:所有存在一条指向节点 `i` 的边的节点的 __编号__ 总和。

返回 边积分 最高的节点。如果多个节点的 边积分 相同，返回编号 最小 的那个。

__示例:__

示例 1：

![2374-3](https://assets.leetcode.com/uploads/2022/06/20/image-20220620195403-1.png)

```text
输入：edges = [1,0,0,0,0,7,7,5]
输出：7
解释：
```

- 节点 1、2、3 和 4 都有指向节点 0 的边，节点 0 的边积分等于 1 + 2 + 3 + 4 = 10 。
- 节点 0 有一条指向节点 1 的边，节点 1 的边积分等于 0 。
- 节点 7 有一条指向节点 5 的边，节点 5 的边积分等于 7 。
- 节点 5 和 6 都有指向节点 7 的边，节点 7 的边积分等于 5 + 6 = 11 。

节点 7 的边积分最高，所以返回 7 。

示例 2：

![2374-4](https://assets.leetcode.com/uploads/2022/06/20/image-20220620200212-3.png)

```text
输入：edges = [2,0,0,2]
输出：0
解释：
```

- 节点 1 和 2 都有指向节点 0 的边，节点 0 的边积分等于 1 + 2 = 3 。
- 节点 0 和 3 都有指向节点 2 的边，节点 2 的边积分等于 0 + 3 = 3 。

节点 0 和 2 的边积分都是 3 。由于节点 0 的编号更小，返回 0 。

__提示：__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `0 <= edges[i] < n`
- `edges[i] != i`

__思路:__

```text
模拟
遍历每个节点, 计算其边积分
可以同时计算得分和判断当前节点是否为最高得分
这样只需要遍历一次数组即可
C++ 和 Java 中使用 long 类型存储得分, 防止溢出
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int edgeScore(vector<int>& edges) 
    {
        long long m = 0LL, n = edges.size(), result = 0LL;
        vector<long long> score(n);
        for (int i = 0; i < n; i++) score[edges[i]] += i;
        for (int i = 0; i < n; i++) 
        {
            if (score[i] > m) 
            {
                m = score[i];
                result = i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int edgeScore(int[] edges) {
        int n = edges.length, result = 0;
        long m = 0L, score[] = new long[n];
        for (int i = 0; i < n; i++) score[edges[i]] += i;
        for (int i = 0; i < n; i++) {
            if (score[i] > m) {
                m = score[i];
                result = i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def edgeScore(self, edges: List[int]) -> int:
        score = [0] * len(edges)
        for i, e in enumerate(edges):
            score[e] += i
        return score.index(max(score))
```
