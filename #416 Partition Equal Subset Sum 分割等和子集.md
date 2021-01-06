# 416 Partition Equal Subset Sum 分割等和子集

__Description__:
Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

__Example:__

Example 1:

Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].

Example 2:

Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.

__Constraints:__

1 <= nums.length <= 200
1 <= nums[i] <= 100

__题目描述__:
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

__注意:__

每个数组中的元素不会超过 100
数组的大小不会超过 200

__示例 :__

示例 1:

输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].

示例 2:

输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.

__思路__:

动态规划
dp[i][j] 表示在 [0, i]区间中是否能选出一些 nums数组中的值, 使得这些元素和等于 j
dp[0][j] 应该初始化为 nums[k] == j, 其中 0 <= k <= len(nums) - 1
从题意可得, sum(num)必须为偶数, 只要求得 dp[len(nums) - 1][sum(nums) / 2]即可
dp[i][j]可以分为两个部分, 要么不取 nums[i], 要么取 nums[i]
转移方程 dp[i][j] = dp[i - 1][j] || dp[i][sum(nums) / 2 - nums[i]]
可以看出来只需要记录一行 dp变量, 压缩空间复杂度到 O(sum(nums))
时间复杂度O(nsum(nums)), 空间复杂度O(sum(nums))

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canPartition(vector<int>& nums) 
    {
        int sum = accumulate(nums.begin(), nums.end(), 0), n = nums.size();
        if (sum & 1) return false;
        vector<bool> dp((sum >>= 1) + 1);
        for (int i = 0; i < n; i++) for (int j = sum; j > nums[i] - 1; j--)
        {
            if (!i) dp[j] = (nums[i] == j);
            else dp[j] = dp[j] or dp[j - nums[i]];
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canPartition(int[] nums) {
        int s = 0, n = nums.length;
        for (int num : nums) s += num;
        if ((s & 1) != 0) return false;
        s >>= 1;
        boolean dp[] = new boolean[s + 1];
        for (int i = 0; i < n; i++) for (int j = s; j > nums[i] - 1; j--) {
            if (i == 0) dp[j] = (j == nums[i]);
            else dp[j] = dp[j] || dp[j - nums[i]];
        }
        return dp[s];
    }
}
```

__Python__:

```Python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        return 1 - (sum(nums) & 1) == reduce(lambda x, y: x | (x << y), nums, 1) >> (sum(nums) >> 1) & 1
```
