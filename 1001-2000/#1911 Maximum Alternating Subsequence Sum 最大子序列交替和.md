# 1911 Maximum Alternating Subsequence Sum 最大子序列交替和

__Description:__

The alternating sum of a 0-indexed array is defined as the sum of the elements at even indices minus the sum of the elements at odd indices.

- For example, the alternating sum of `[4,2,5,3]` is `(4 + 5) - (2 + 3) = 4`.

Given an array `nums`, return _the __maximum alternating sum__ of any subsequence of_ `nums` _(after __reindexing__ the elements of the subsequence)_.

A __subsequence__ of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, `[2,7,4]` is a subsequence of `[4,2,3,7,2,1,4]` (the underlined elements), while `[2,4,2]` is not.

__Example:__

Example 1:

```text
Input: nums = [4,2,5,3]
Output: 7
Explanation: It is optimal to choose the subsequence [4,2,5] with alternating sum (4 + 5) - 2 = 7.
```

Example 2:

```text
Input: nums = [5,6,7,8]
Output: 8
Explanation: It is optimal to choose the subsequence [8] with alternating sum 8.
```

Example 3:

```text
Input: nums = [6,2,1,2,4,5]
Output: 10
Explanation: It is optimal to choose the subsequence [6,1,5] with alternating sum (6 + 5) - 1 = 10.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

一个下标从 0 开始的数组的 交替和 定义为 偶数 下标处元素之 和 减去 奇数 下标处元素之 和 。

- 比方说，数组 `[4,2,5,3]` 的交替和为 `(4 + 5) - (2 + 3) = 4` 。

给你一个数组 `nums` ，请你返回 `nums` 中任意子序列的 __最大交替和__ （子序列的下标 __重新__ 从 0 开始编号）。

一个数组的 __子序列__ 是从原数组中删除一些元素后（也可能一个也不删除）剩余元素不改变顺序组成的数组。比方说， `[2,7,4]` 是 [4,__2__,3,__7__,2,1,__4__] 的一个子序列（加粗元素），但是 `[2,4,2]` 不是。

__示例:__

示例 1：

```text
输入：nums = [4,2,5,3]
输出：7
解释：最优子序列为 [4,2,5] ，交替和为 (4 + 5) - 2 = 7 。
```

示例 2：

```text
输入：nums = [5,6,7,8]
输出：8
解释：最优子序列为 [8] ，交替和为 8 。
```

示例 3：

```text
输入：nums = [6,2,1,2,4,5]
输出：10
解释：最优子序列为 [6,1,5] ，交替和为 (6 + 5) - 1 = 10 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
动态规划
设 dp[i][0] 表示 nums[:i] 中最后一个数下标为偶数的最大交替子序列和, dp[i][1] 表示nums[:i] 中最后一个数下标为奇数的最大交替子序列和
如果取 nums[i], dp[i + 1][0] = dp[i][1] - nums[i], dp[i + 1][1] = dp[i][0] + nums[i]
如果不取 nums[i], dp[i + 1][0] = dp[i][0], dp[i + 1][1] = dp[i][1]
取两者的较大值, 即 dp[i + 1][0] = max(dp[i][1] - nums[i], dp[i][0]), dp[i + 1][1] = max(dp[i][0] + nums[i], dp[i][1])
最后如果为奇数下标需要被减, 所以 dp[n][1] < dp[n][0], 返回 dp[n][0] 即可
初始化 dp[0][0] = 0, dp[0][1] = -inf
注意到 dp[i + 1] 只与 dp[i] 有关, 所以可以优化空间复杂度为 O(1)
即 f = max(g - x, f), g = max(f + x, g)
初始化 f = g = 0, 由于 g - x < g, f < f + x, 最后返回 g 即可
如果我们将子序列中的偶数部分看作购买需要的本金, 奇数部分看作卖出后的收益, 那么最大交替子序列和就是最后的收益
假设刚开始的本金为 0, 那只需要找到相邻的两个数的差值累加上正数即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxAlternatingSum(vector<int>& nums) 
    {
        return accumulate(nums.cbegin(), nums.cend(), 0ll, [b = 0ll](auto a, auto num)mutable { return max((b = max(a - num, b)) + num, a); });
    }
};
```

__Java__:

```Java
class Solution {
    public long maxAlternatingSum(int[] nums) {
        long x = nums[0];
        for (int i = 1, n = nums.length; i < n; i++) x += Math.max(nums[i] - nums[i - 1], 0);
        return x;
    }
}
```

__Python__:

```Python
class Solution:
    def maxAlternatingSum(self, nums: List[int]) -> int:
        return nums[0] + sum(max(nums[i + 1] - nums[i], 0) for i in range(len(nums) - 1))
```
