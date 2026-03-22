# 2673 Make Costs of Paths Equal in a Binary Tree 使二叉树所有路径值相等的最小代价

__Description:__

You are given an integer `n` representing the number of nodes in a __perfect binary tree__ consisting of nodes numbered from `1` to `n`. The root of the tree is node `1` and each node `i` in the tree has two children where the left child is the node `2 * i` and the right child is `2 * i + 1`.

Each node in the tree also has a __cost__ represented by a given __0-indexed__ integer array `cost` of size `n` where `cost[i]` is the cost of node `i + 1`. You are allowed to __increment__ the cost of __any__ node by `1` __any__ number of times.

Return the minimum number of increments you need to make the cost of paths from the root to each leaf node equal.

Note:

- A __perfect binary tree__ is a tree where each node, except the leaf nodes, has exactly 2 children.
- The __cost of a path__ is the sum of costs of nodes in the path.

__Example:__

Example 1:

![2673-1](https://assets.leetcode.com/uploads/2023/04/04/binaryytreeedrawio-4.png)

```text
Input: n = 7, cost = [1,5,2,2,3,3,1]
Output: 6
Explanation: We can do the following increments:
```

- Increase the cost of node 4 one time.
- Increase the cost of node 3 three times.
- Increase the cost of node 7 two times.

Each path from the root to a leaf will have a total cost of 9.
The total increments we did is 1 + 3 + 2 = 6.

It can be shown that this is the minimum answer we can achieve.

Example 2:

![2673-2](https://assets.leetcode.com/uploads/2023/04/04/binaryytreee2drawio.png)

```text
Input: n = 3, cost = [5,3,3]
Output: 0
Explanation: The two paths already have equal total costs, so no increments are needed.
```

__Constraints:__

- `3 <= n <= 10 ^ 5`
- `n + 1` is a power of `2`
- `cost.length == n`
- `1 <= cost[i] <= 10 ^ 4`

__题目描述:__

给你一个整数 `n` 表示一棵 _满二叉树_ 里面节点的数目，节点编号从 `1` 到 `n` 。根节点编号为 `1` ，树中每个非叶子节点 `i` 都有两个孩子，分别是左孩子 `2 * i` 和右孩子 `2 * i + 1` 。

树中每个节点都有一个值，用下标从 _0_ 开始、长度为 `n` 的整数数组 `cost` 表示，其中 `cost[i]` 是第 `i + 1` 个节点的值。每次操作，你可以将树中 __任意__ 节点的值 __增加__ `1` 。你可以执行操作 __任意__ 次。

你的目标是让根到每一个 叶子结点 的路径值相等。请你返回 最少 需要执行增加操作多少次。

注意：

- __满二叉树__ 指的是一棵树，它满足树中除了叶子节点外每个节点都恰好有 2 个子节点，且所有叶子节点距离根节点距离相同。
- __路径值__ 指的是路径上所有节点的值之和。

__示例:__

示例 1：

![2673-3](https://assets.leetcode.com/uploads/2023/04/04/binaryytreeedrawio-4.png)

```text
输入：n = 7, cost = [1,5,2,2,3,3,1]
输出：6
解释：我们执行以下的增加操作：
```

- 将节点 4 的值增加一次。
- 将节点 3 的值增加三次。
- 将节点 7 的值增加两次。

从根到叶子的每一条路径值都为 9 。

总共增加次数为 1 + 3 + 2 = 6 。

这是最小的答案。

示例 2：

![2673-4](https://assets.leetcode.com/uploads/2023/04/04/binaryytreee2drawio.png)

```text
输入：n = 3, cost = [5,3,3]
输出：0
解释：两条路径已经有相等的路径值，所以不需要执行任何增加操作。
```

__提示：__

- `3 <= n <= 10 ^ 5`
- `n + 1` 是 `2` 的幂
- `cost.length == n`
- `1 <= cost[i] <= 10 ^ 4`

__思路:__

```text
贪心
最终每行的节点值都相等
所以可以从叶子节点开始
先加上相邻兄弟节点的差值的绝对值
再把这两个节点的父节点加上它们的最大值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minIncrements(int n, vector<int>& cost) 
    {
        int result = 0;
        for (int i = (n >> 1); i > 0; i--) 
        {
            result += abs(cost[(i << 1) - 1] - cost[i << 1]);
            cost[i - 1] += max(cost[(i << 1) - 1], cost[i << 1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minIncrements(int n, int[] cost) {
        int result = 0;
        for (int i = (n >>> 1); i > 0; i--) {
            result += Math.abs(cost[(i << 1) - 1] - cost[i << 1]);
            cost[i - 1] += Math.max(cost[(i << 1) - 1], cost[i << 1]);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minIncrements(self, n: int, cost: List[int]) -> int:
        result = 0
        for i in range(n >> 1, 0, -1):
            result += abs(cost[(i << 1) - 1] - cost[i << 1])
            cost[i - 1] += max(cost[(i << 1) - 1], cost[i << 1])
        return result
```
