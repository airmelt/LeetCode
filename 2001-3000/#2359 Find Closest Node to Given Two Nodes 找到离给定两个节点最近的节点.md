# 2359 Find Closest Node to Given Two Nodes 找到离给定两个节点最近的节点

__Description:__

You are given a __directed__ graph of `n` nodes numbered from `0` to `n - 1`, where each node has __at most one__ outgoing edge.

The graph is represented with a given __0-indexed__ array `edges` of size `n`, indicating that there is a directed edge from node `i` to node `edges[i]`. If there is no outgoing edge from `i`, then `edges[i] == -1`.

You are also given two integers `node1` and `node2`.

Return _the __index__ of the node that can be reached from both_ `node1` _and_ `node2`_, such that the __maximum__ between the distance from_ `node1` _to that node, and from_ `node2` _to that node is __minimized___. If there are multiple answers, return the node with the __smallest__ index, and if no possible answer exists, return `-1`.

Note that `edges` may contain cycles.

__Example:__

Example 1:

![2359-1](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-2.png)

```text
Input: edges = [2,2,3,-1], node1 = 0, node2 = 1
Output: 2
Explanation: The distance from node 0 to node 2 is 1, and the distance from node 1 to node 2 is 1.
The maximum of those two distances is 1. It can be proven that we cannot get a node with a smaller maximum distance than 1, so we return node 2.
```

Example 2:

![2359-2](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-4.png)

```text
Input: edges = [1,2,-1], node1 = 0, node2 = 2
Output: 2
Explanation: The distance from node 0 to node 2 is 2, and the distance from node 2 to itself is 0.
The maximum of those two distances is 2. It can be proven that we cannot get a node with a smaller maximum distance than 2, so we return node 2.
```

__Constraints:__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `-1 <= edges[i] < n`
- `edges[i] != i`
- `0 <= node1, node2 < n`

__题目描述:__

给你一个 `n` 个节点的 __有向图__ ，节点编号为 `0` 到 `n - 1` ，每个节点 __至多__ 有一条出边。

有向图用大小为 `n` 下标从 __0__ 开始的数组 `edges` 表示，表示节点 `i` 有一条有向边指向 `edges[i]` 。如果节点 `i` 没有出边，那么 `edges[i] == -1` 。

同时给你两个节点 `node1` 和 `node2` 。

请你返回一个从 `node1` 和 `node2` 都能到达节点的编号，使节点 `node1` 和节点 `node2` 到这个节点的距离 _较大值最小化_ 。如果有多个答案，请返回 __最小__ 的节点编号。如果答案不存在，返回 `-1` 。

注意 `edges` 可能包含环。

__示例:__

示例 1：

![2359-3](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-2.png)

```text
输入：edges = [2,2,3,-1], node1 = 0, node2 = 1
输出：2
解释：从节点 0 到节点 2 的距离为 1 ，从节点 1 到节点 2 的距离为 1 。
两个距离的较大值为 1 。我们无法得到一个比 1 更小的较大值，所以我们返回节点 2 。
```

示例 2：

![2359-4](https://assets.leetcode.com/uploads/2022/06/07/graph4drawio-4.png)

```text
输入：edges = [1,2,-1], node1 = 0, node2 = 2
输出：2
解释：节点 0 到节点 2 的距离为 2 ，节点 2 到它自己的距离为 0 。
两个距离的较大值为 2 。我们无法得到一个比 2 更小的较大值，所以我们返回节点 2 。
```

__提示：__

- `n == edges.length`
- `2 <= n <= 10 ^ 5`
- `-1 <= edges[i] < n`
- `edges[i] != i`
- `0 <= node1, node2 < n`

__思路:__

```text
模拟
初始化距离为 inf, 结果为 -1
分别求出 node1 和 node2 到其他所有能到达的节点的距离 d1, d2
则最终结果为 max(d1[i], d2[i]) 的最小值
由于 edges 中只有 n 条边, 最多构成 1 个环
可以使用一次循环求出 d1 和 d2
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int closestMeetingNode(vector<int>& edges, int node1, int node2) 
    {
        int n = edges.size(), min_dis = n, result = -1, inf = 0x3f3f3f3f;
        auto helper = [&](int x) -> vector<int> 
        {
            vector<int> dis(n, inf);
            for (int d = 0; x >= 0 and dis[x] == inf; x = edges[x]) dis[x] = d++;
            return dis;
        };
        auto d1 = helper(node1), d2 = helper(node2);
        for (int i = 0, d = 0; i < n; i++) 
        {
            if (min_dis > (d = max(d1[i], d2[i]))) 
            {
                min_dis = d;
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
    public int closestMeetingNode(int[] edges, int node1, int node2) {
        int result = -1, n = edges.length, minDis = n, d1[] = helper(node1, edges), d2[] = helper(node2, edges);
        for (int i = 0, d = 0; i < n; i++) {
            if (minDis > (d = Math.max(d1[i], d2[i]))) {
                minDis = d;
                result = i;
            }
        }
        return result;
    }

    private int[] helper(int x, int[] edges) {
        int n = edges.length, dis[] = new int[n], inf = 0x3f3f3f3f;
        Arrays.fill(dis, inf);
        for (int d = 0; x >= 0 && dis[x] == inf; x = edges[x]) dis[x] = d++;
        return dis;
    }
}
```

__Python__:

```Python
class Solution:
    def closestMeetingNode(self, edges: List[int], node1: int, node2: int) -> int:
        n, min_dis, result = len(edges), len(edges), -1
        
        def helper(x: int) -> List[int]:
            dis, d = [inf] * n, 0
            while x >= 0 and dis[x] == inf:
                dis[x] = d
                d += 1
                x = edges[x]
            return dis
        for i, d in enumerate(map(max, zip(helper(node1), helper(node2)))):
            if d < min_dis:
                min_dis, result = d, i
        return result
```
