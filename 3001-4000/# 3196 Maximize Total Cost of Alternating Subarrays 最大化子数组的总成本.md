# 3196 Maximize Total Cost of Alternating Subarrays 最大化子数组的总成本

__Description:__

You are given an integer array `nums` with length `n`.

The cost of a __subarray__ `nums[l..r]`, where `0 <= l <= r < n`, is defined as:

`cost(l, r) = nums[l] - nums[l + 1] + ... + nums[r] * (−1) ^ (r − l)`

Your task is to __split__ `nums` into subarrays such that the __total cost__ of the subarrays is __maximized__, ensuring each element belongs to __exactly one__ subarray.

Formally, if `nums` is split into `k` subarrays, where `k > 1`, at indices `i_1, i_2, ..., i_k − 1`, where `0 <= i_1 < i_2 < ... < i_k - 1 < n - 1`, then the total cost will be:

`cost(0, i_1) + cost(i_1 + 1, i_2) + ... + cost(i_k − 1 + 1, n − 1)`

Return an integer denoting the _maximum total_ cost of the subarrays after splitting the array optimally.

__Note__: If `nums` is not split into subarrays, i.e. `k = 1`, the total cost is simply `cost(0, n - 1)`.

__Example:__

Example 1:

```text
Input: nums = [1,-2,3,4]

Output: 10

Explanation:

One way to maximize the total cost is by splitting [1, -2, 3, 4] into subarrays [1, -2, 3] and [4]. The total cost will be (1 + 2 + 3) + 4 = 10.
```

Example 2:

```text
Input: nums = [1,-1,1,-1]

Output: 4

Explanation:

One way to maximize the total cost is by splitting [1, -1, 1, -1] into subarrays [1, -1] and [1, -1]. The total cost will be (1 + 1) + (1 + 1) = 4.
```

Example 3:

```text
Input: nums = [0]

Output: 0

Explanation:

We cannot split the array further, so the answer is 0.
```

Example 4:

```text
Input: nums = [1,-1]

Output: 2

Explanation:

Selecting the whole array gives a total cost of 1 + 1 = 2, which is the maximum.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums`。

子数组 `nums[l..r]`（其中 `0 <= l <= r < n`）的 __成本__ 定义为：

`cost(l, r) = nums[l] - nums[l + 1] + ... + nums[r] * (−1) ^ (r − l)`

你的任务是将 `nums` 分割成若干子数组，使得所有子数组的成本之和 __最大化__，并确保每个元素 __正好__ 属于一个子数组。

具体来说，如果 `nums` 被分割成 `k` 个子数组，且分割点为索引 `i_1, i_2, ..., i_k − 1`（其中 `0 <= i_1 < i_2 < ... < i_k - 1 < n - 1`），则总成本为：

`cost(0, i_1) + cost(i_1 + 1, i_2) + ... + cost(i_k − 1 + 1, n − 1)`

返回在最优分割方式下的子数组成本之和的最大值。

__注意__：如果 `nums` 没有被分割，即 `k = 1`，则总成本即为 `cost(0, n - 1)`。

__示例:__

示例 1：

```text
输入： nums = [1,-2,3,4]

输出： 10

解释：

一种总成本最大化的方法是将 [1, -2, 3, 4] 分割成子数组 [1, -2, 3] 和 [4]。总成本为 (1 + 2 + 3) + 4 = 10。
```

示例 2：

```text
输入： nums = [1,-1,1,-1]

输出： 4

解释：

一种总成本最大化的方法是将 [1, -1, 1, -1] 分割成子数组 [1, -1] 和 [1, -1]。总成本为 (1 + 1) + (1 + 1) = 4。
```

示例 3：

```text
输入： nums = [0]

输出： 0

解释：

无法进一步分割数组，因此答案为 0。
```

示例 4：

```text
输入： nums = [1,-1]

输出： 2

解释：

选择整个数组，总成本为 1 + 1 = 2，这是可能的最大成本。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__思路:__

```text
贪心
设 dp[i] 表示 nums[i:] 的总成本
对于子数组长度为 1 cost = +
对于子数组长度为 2 cost = +-
对于大于 2 的子数组, 比如 5 cost = +-+-+, 实际上可以看作 +- +- +
也就是说大于长度 2 的子数组实际上都是由长度为 1 和 2 的子数组构成的
如果在 nums[i] 处截断出一个新数组 dp[i] = dp[i - 1] + nums[i]
否则 nums[i] 和 nums[i - 1] 构成一个子数组 dp[i] = dp[i - 2] + nums[i - 1] - nums[i]
这两种情况取较大值
又因为 dp[i] 只与 dp[i - 1] 和 dp[i - 2] 有关
可以用滚动数组优化空间复杂度到 O(1)
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumTotalCost(vector<int>& nums) 
    {
        long long a = 0LL, b = nums.front(), t = 0LL;
        for (int i = 1, n = nums.size(); i < n; i++) 
        {
            t = b;
            b = max(a + nums[i - 1] - nums[i], b + nums[i]);
            a = t;
        }
        return b;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumTotalCost(int[] nums) {
        long a = 0L, b = nums[0], t = 0L;
        for (int i = 1, n = nums.length; i < n; i++) {
            t = b;
            b = Math.max(a + nums[i - 1] - nums[i], b + nums[i]);
            a = t;
        }
        return b;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumTotalCost(self, nums: List[int]) -> int:
        a, b, n = 0, nums[0], len(nums)
        for x, y in pairwise(nums):
            a, b = b, max(b + y, a + x - y)
        return b
```
