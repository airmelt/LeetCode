# 2518 Number of Great Partitions 好分区的数目

__Description:__

You are given an array `nums` consisting of __positive__ integers and an integer `k`.

__Partition__ the array into two ordered __groups__ such that each element is in exactly __one__ group. A partition is called great if the __sum__ of elements of each group is greater than or equal to `k`.

Return _the number of __distinct__ great partitions_. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

Two partitions are considered distinct if some element `nums[i]` is in different groups in the two partitions.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4], k = 4
Output: 6
Explanation: The great partitions are: ([1,2,3], [4]), ([1,3], [2,4]), ([1,4], [2,3]), ([2,3], [1,4]), ([2,4], [1,3]) and ([4], [1,2,3]).
```

Example 2:

```text
Input: nums = [3,3,3], k = 4
Output: 0
Explanation: There are no great partitions for this array.
```

Example 3:

```text
Input: nums = [6,6], k = 2
Output: 2
Explanation: We can either put nums[0] in the first partition or in the second partition.
The great partitions will be ([6], [6]) and ([6], [6]).
```

__Constraints:__

- `1 <= nums.length, k <= 1000`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个正整数数组 `nums` 和一个整数 `k` 。

__分区__ 的定义是:将数组划分成两个有序的 __组__ ，并满足每个元素 __恰好__ 存在于 __某一个__ 组中。如果分区中每个组的元素和都大于等于 `k` ，则认为分区是一个好分区。

返回 __不同__ 的好分区的数目。由于答案可能很大，请返回对 `10 ^ 9 + 7` __取余__ 后的结果。

如果在两个分区中，存在某个元素 `nums[i]` 被分在不同的组中，则认为这两个分区不同。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4], k = 4
输出：6
解释：好分区的情况是 ([1,2,3], [4]), ([1,3], [2,4]), ([1,4], [2,3]), ([2,3], [1,4]), ([2,4], [1,3]) 和 ([4], [1,2,3]) 。
```

示例 2：

```text
输入：nums = [3,3,3], k = 4
输出：0
解释：数组中不存在好分区。
```

示例 3：

```text
输入：nums = [6,6], k = 2
输出：2
解释：可以将 nums[0] 放入第一个分区或第二个分区中。
好分区的情况是 ([6], [6]) 和 ([6], [6]) 。
```

__提示：__

- `1 <= nums.length, k <= 1000`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
动态规划
如果数组的和小于 2 * k，则不存在好分区
设 dp[i][j] 表示前 i 个数中和为 j 的组合数
初始化 dp[0][0] = 1
dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i - 1]], 其中 nums[i - 1] <= j
因为 dp[i][j] 只与 dp[i - 1][j] 和 dp[i - 1][j - nums[i - 1]] 有关，所以可以将 dp 数组压缩为一维数组
dp[j] = dp[j] + dp[j - nums[i - 1]]
最后的结果为 2 ^ n - 2 - (dp[1] - ... - dp[k - 1]) * 2
2 ^ n 表示子集数, 空集和全集刚好都是 1, 即 dp[0]
所以可以简化为 2 ^ n - sum(dp)
时间复杂度为 O(NK), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPartitions(vector<int>& nums, int k) 
    {
        long long s = accumulate(nums.begin(), nums.end(), 0LL), result = 1LL, MOD = 1e9 + 7;
        if (s < ((long long)k << 1LL)) return 0;
        vector<long long> dp(k);
        dp.front() = 1LL;
        for (const auto& num : nums) for (int i = k - 1; i > num - 1; i--) dp[i] = (dp[i] + dp[i - num]) % MOD;
        for (const auto& num : nums) result = (result << 1LL) % MOD;
        for (const auto& x : dp) result = (result - (x << 1L) % MOD + MOD) % MOD;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countPartitions(int[] nums, int k) {
        long s = Arrays.stream(nums).mapToLong(i -> i).sum(), MOD = 1_000_000_007, dp[] = new long[k], result = 1L;
        if (s < ((long)k << 1L)) return 0;
        dp[0] = 1;
        for (int num : nums) for (int i = k - 1; i > num - 1; i--) dp[i] = (dp[i] + dp[i - num]) % MOD;
        for (int num : nums) result = (result << 1L) % MOD;
        for (long x : dp) result = (result - (x << 1L) % MOD + MOD) % MOD;
        return (int)result;
    }
}
```

__Python__:

```Python
class Solution {
    public int countPartitions(int[] nums, int k) {
        long s = Arrays.stream(nums).mapToLong(i -> i).sum(), MOD = 1_000_000_007, dp[] = new long[k], result = 1L;
        if (s < ((long)k << 1L)) return 0;
        dp[0] = 1;
        for (int num : nums) for (int i = k - 1; i > num - 1; i--) dp[i] = (dp[i] + dp[i - num]) % MOD;
        for (int num : nums) result = (result << 1L) % MOD;
        for (long x : dp) result = (result - (x << 1L) % MOD + MOD) % MOD;
        return (int)result;
    }
}
```
