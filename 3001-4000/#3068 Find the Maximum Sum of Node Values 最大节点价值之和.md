# 3068 Find the Maximum Sum of Node Values 最大节点价值之和

__Description:__

There exists an __undirected__ tree with `n` nodes numbered `0` to `n - 1`. You are given a __0-indexed__ 2D integer array `edges` of length `n - 1`, where `edges[i] = [ui, vi]` indicates that there is an edge between nodes `ui` and `vi` in the tree. You are also given a __positive__ integer `k`, and a __0-indexed__ array of __non-negative__ integers `nums` of length `n`, where `nums[i]` represents the __value__ of the node numbered `i`.

Alice wants the sum of values of tree nodes to be maximum, for which Alice can perform the following operation any number of times (including zero) on the tree:

- Choose any edge `[u, v]` connecting the nodes `u` and `v`, and update their values as follows:

  - `nums[u] = nums[u] XOR k`
  - `nums[v] = nums[v] XOR k`

Return the maximum possible sum of the values Alice can achieve by performing the operation any number of times.

__Example:__

Example 1:

![3068-1](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

```text
Input: nums = [1,2,1], k = 3, edges = [[0,1],[0,2]]
Output: 6
Explanation: Alice can achieve the maximum sum of 6 using a single operation:
```

- Choose the edge [0,2]. nums[0] and nums[2] become: 1 XOR 3 = 2, and the array nums becomes: [1,2,1] -> [2,2,2].

The total sum of values is 2 + 2 + 2 = 6.

It can be shown that 6 is the maximum achievable sum of values.

Example 2:

![3068-2](https://assets.leetcode.com/uploads/2024/01/09/screenshot-2024-01-09-220017.png)

```text
Input: nums = [2,3], k = 7, edges = [[0,1]]
Output: 9
Explanation: Alice can achieve the maximum sum of 9 using a single operation:
```

- Choose the edge [0,1]. nums[0] becomes: 2 XOR 7 = 5 and nums[1] become: 3 XOR 7 = 4, and the array nums becomes: [2,3] -> [5,4].

The total sum of values is 5 + 4 = 9.

It can be shown that 9 is the maximum achievable sum of values.

Example 3:

![3068-3](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

```text
Input: nums = [7,7,7,7,7,7], k = 3, edges = [[0,1],[0,2],[0,3],[0,4],[0,5]]
Output: 42
Explanation: The maximum achievable sum is 42 which can be achieved by Alice performing no operations.
```

__Constraints:__

- `2 <= n == nums.length <= 2 * 10 ^ 4`
- `1 <= k <= 10 ^ 9`
- `0 <= nums[i] <= 10 ^ 9`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= edges[i][0], edges[i][1] <= n - 1`
- The input is generated such that `edges` represent a valid tree.

__题目描述:__

给你一棵 `n` 个节点的 __无向__ 树，节点从 `0` 到 `n - 1` 编号。树以长度为 `n - 1` 下标从 __0__ 开始的二维整数数组 `edges` 的形式给你，其中 `edges[i] = [ui, vi]` 表示树中节点 `ui` 和 `vi` 之间有一条边。同时给你一个 __正__ 整数 `k` 和一个长度为 `n` 下标从 __0__ 开始的 __非负__ 整数数组 `nums` ，其中 `nums[i]` 表示节点 `i` 的 __价值__ 。

Alice 想 最大化 树中所有节点价值之和。为了实现这一目标，Alice 可以执行以下操作 任意 次（包括 0 次）：

- 选择连接节点 `u` 和 `v` 的边 `[u, v]` ，并将它们的值更新为:

  - `nums[u] = nums[u] XOR k`
  - `nums[v] = nums[v] XOR k`

请你返回 Alice 通过执行以上操作 任意次 后，可以得到所有节点 价值之和 的 最大值 。

__示例:__

示例 1：

![3068-4](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012513.png)

```text
输入：nums = [1,2,1], k = 3, edges = [[0,1],[0,2]]
输出：6
解释：Alice 可以通过一次操作得到最大价值和 6 ：
```

- 选择边 [0,2] 。nums[0] 和 nums[2] 都变为：1 XOR 3 = 2 ，数组 nums 变为：[1,2,1] -> [2,2,2] 。

所有节点价值之和为 2 + 2 + 2 = 6 。

6 是可以得到最大的价值之和。

示例 2：

![3068-5](https://assets.leetcode.com/uploads/2024/01/09/screenshot-2024-01-09-220017.png)

```text
输入：nums = [2,3], k = 7, edges = [[0,1]]
输出：9
解释：Alice 可以通过一次操作得到最大和 9 ：
```

- 选择边 [0,1] 。nums[0] 变为：2 XOR 7 = 5 ，nums[1] 变为：3 XOR 7 = 4 ，数组 nums 变为：[2,3] -> [5,4] 。

所有节点价值之和为 5 + 4 = 9 。

9 是可以得到最大的价值之和。

示例 3：

![3068-6](https://assets.leetcode.com/uploads/2023/11/09/screenshot-2023-11-10-012641.png)

```text
输入：nums = [7,7,7,7,7,7], k = 3, edges = [[0,1],[0,2],[0,3],[0,4],[0,5]]
输出：42
解释：Alice 不需要执行任何操作，就可以得到最大价值之和 42 。
```

__提示：__

- `2 <= n == nums.length <= 2 * 10 ^ 4`
- `1 <= k <= 10 ^ 9`
- `0 <= nums[i] <= 10 ^ 9`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= edges[i][0], edges[i][1] <= n - 1`
- 输入保证 `edges` 构成一棵合法的树。

__思路:__

```text
动态规划
因为可以任意连接两个节点, 其实 edges 没用, 也不需要建树
设 dp[i + 1][0] 表示当前选偶数个元素的最大价值
dp[i + 1][1] 表示当前选奇数个元素的最大价值
初始化 dp[0][0] = 0, 表示选 0 个元素, dp[0][1] = -inf, 选 0 个元素的时候不为奇数, 这是一个非法的初始值
那么 dp[i + 1][0] = max(dp[i][0] + nums[i], dp[i][1] + (nums[i] ^ k))
奇数类似
注意到 dp[i + 1] 只和 dp[i] 有关, 可以使用滚动数组更新
注意需要同时更新 dp[i + 1]
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumValueSum(vector<int>& nums, int k, vector<vector<int>>& edges) 
    {
        long long a = 0LL, b = LLONG_MIN, c = 0LL;
        for (const auto& num : nums) 
        {
            c = b;
            b = max(a + (num ^ k), b + num);
            a = max(c + (num ^ k), a + num);
        }
        return a;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumValueSum(int[] nums, int k, int[][] edges) {
        long a = 0L, b = Long.MIN_VALUE, c = 0L;
        for (int num : nums) {
            c = b;
            b = Math.max(a + (num ^ k), b + num);
            a = Math.max(c + (num ^ k), a + num);
        }
        return a;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumValueSum(self, nums: List[int], k: int, edges: List[List[int]]) -> int:
        a, b = 0, -inf
        for num in nums:
            a, b = max(b + (num ^ k), a + num), max(a + (num ^ k), b + num)
        return a
```
