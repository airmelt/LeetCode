# 3077 Maximum Strength of K Disjoint Subarrays K 个不相交子数组的最大能量值

__Description:__

You are given an array of integers `nums` with length `n`, and a positive __odd__ integer `k`.

Select exactly _`k`_ disjoint subarrays _`sub1, sub2, ..., subk`_ from `nums` such that the last element of `subi` appears before the first element of `sub{i+1}` for all `1 <= i <= k-1`. The goal is to maximize their combined strength.

The strength of the selected subarrays is defined as:

`strength = k * sum(sub1)- (k - 1) * sum(sub2) + (k - 2) * sum(sub3) - ... - 2 * sum(sub{k-1}) + sum(subk)`

where _`sum(subi)`_ is the sum of the elements in the `i`-th subarray.

Return the __maximum__ possible strength that can be obtained from selecting exactly _`k`_ disjoint subarrays from `nums`.

Note that the chosen subarrays don't need to cover the entire array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,-1,2], k = 3

Output: 22

Explanation:

The best possible way to select 3 subarrays is: nums[0..2], nums[3..3], and nums[4..4]. The strength is calculated as follows:
```

`strength = 3 * (1 + 2 + 3) - 2 * (-1) + 2 = 22`

Example 2:

```text
Input: nums = [12,-2,-2,-2,-2], k = 5

Output: 64

Explanation:

The only possible way to select 5 disjoint subarrays is: nums[0..0], nums[1..1], nums[2..2], nums[3..3], and nums[4..4]. The strength is calculated as follows:
```

`strength = 5 * 12 - 4 * (-2) + 3 * (-2) - 2 * (-2) + (-2) = 64`

Example 3:

```text
Input: nums = [-1,-2,-3], k = 1

Output: -1

Explanation:

The best possible way to select 1 subarray is: nums[0..0]. The strength is -1.
```

__Constraints:__

- `1 <= n <= 10 ^ 4`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= n`
- `1 <= n * k <= 10 ^ 6`
- `k` is odd.

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的整数数组 `nums` 和一个 __正奇数__ 整数 `k` 。

`x` 个子数组的能量值定义为 `strength = sum[1] * x - sum[2] * (x - 1) + sum[3] * (x - 2) - sum[4] * (x - 3) + ... + sum[x] * 1` ，其中 `sum[i]` 是第 `i` 个子数组的和。更正式的，能量值是满足 `1 <= i <= x` 的所有 `i` 对应的 `(-1) ^ i+1 * sum[i] * (x - i + 1)` 之和。

你需要在 `nums` 中选择 `k` 个 __不相交__ __子数组__ ，使得 __能量值最大__ 。

请你返回可以得到的 最大能量值 。

注意，选出来的所有子数组 不 需要覆盖整个数组。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,-1,2], k = 3
输出：22
解释：选择 3 个子数组的最好方式是选择：nums[0..2] ，nums[3..3] 和 nums[4..4] 。能量值为 (1 + 2 + 3) * 3 - (-1) * 2 + 2 * 1 = 22 。
```

示例 2：

```text
输入：nums = [12,-2,-2,-2,-2], k = 5
输出：64
解释：唯一一种选 5 个不相交子数组的方案是：nums[0..0] ，nums[1..1] ，nums[2..2] ，nums[3..3] 和 nums[4..4] 。能量值为 12 * 5 - (-2) * 4 + (-2) * 3 - (-2) * 2 + (-2) * 1 = 64 。
```

示例 3：

```text
输入：nums = [-1,-2,-3], k = 1
输出：-1
解释：选择 1 个子数组的最优方案是：nums[0..0] 。能量值为 -1 。
```

__提示：__

- `1 <= n <= 10 ^ 4`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= n`
- `1 <= n * k <= 10 ^ 6`
- `k` 是奇数。

__思路:__

```text
动态规划
设 dp[i][j] 表示将 nums[:j] 分成 i 个组的最大能量
初始化 dp[0][j] = 0, 表示分成 0 组的初始能量为 0
dp[i][i - 1] = -inf, 表示不可能得到, 实际上对于所有 j < i 都为 -inf, 因为子数组不能为空
对于 nums[j - 1]: 
当不选 nums[j - 1], dp[i][j] = dp[i][j - 1], 表示从前 j - 1 个数中选出 i 个组
当选择 nums[j - 1] 时, nums[j - 1] 作为第 i 组的最后一个数
那么遍历 l, l 的取值范围为 [i - 1, j - 1], 因为需要保证前 i - 1 个组至少有 1 个元素, 最后不能取到 nums[j - 1]
所以 dp[i][j] = max(dp[i - 1][l] + (pre[j] - pre[l]) * (k - i + 1) * (1 if i & 1 else -1))
这里 pre 为前缀和, 令 w = (k - i + 1) * (1 if i & 1 else -1)
这种解法时间复杂度为 O(KN ^ 2) 会超时
将 dp[i][j] 改写为
pre[j] * w + max(dp[i - 1][l] - pre[l] * w)
记录上一次遍历的 dp[i - 1][l] 为 cur, 并注意到 l 的取值范围为 [i - 1, j - 1]
所以可以用 mx = max(cur, pre[j - 1] * w) 来求得最大的值
cur 每次更新为 dp[i - 1][j] 即可
这样可以一边遍历 j 一边求出最大值
又由于 dp[i] 只和 dp[i - 1] 有关, 可以用滚动数组优化到 O(N)
时间复杂度为 O(KN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumStrength(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<long long> dp(n + 1), pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 1; i <= k; i++) 
        {
            long long cur = dp[i - 1], mx = 0LL, w = (k - i + 1) * (i & 1 ? 1LL : -1LL);
            dp[i - 1] = mx = LLONG_MIN;
            for (int j = i; j <= n - k + i; j++)
            {
                mx = max(mx, cur - pre[j - 1] * w);
                cur = dp[j];
                dp[j] = max(dp[j - 1], mx + pre[j] * w);
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumStrength(int[] nums, int k) {
        int n = nums.length;
        long dp[] = new long[n + 1], pre[] = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
        for (int i = 1; i <= k; i++) {
            long cur = dp[i - 1], w = (k - i + 1) * ((i & 1) == 1 ? 1L : -1L), mx = 0L;
            dp[i - 1] = mx = Long.MIN_VALUE;
            for (int j = i; j <= n - k + i; j++) {
                mx = Math.max(mx, cur - w * pre[j - 1]);
                cur = dp[j];
                dp[j] = Math.max(dp[j - 1], mx + w * pre[j]);
            }
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maximumStrength(self, nums: List[int], k: int) -> int:
        dp, pre = [0] * ((n := len(nums)) + 1), list(accumulate(nums, initial=0))
        for i in range(1, k + 1):
            dp[i - 1], w, mx, cur = -inf, (k - i + 1) * (1 if i & 1 else -1), -inf, dp[i - 1]
            for j in range(i, n - k + i + 1):
                dp[j], cur = max(dp[j - 1], (mx := max(mx, cur - pre[j - 1] * w)) + pre[j] * w), dp[j]
        return dp[n]
```
