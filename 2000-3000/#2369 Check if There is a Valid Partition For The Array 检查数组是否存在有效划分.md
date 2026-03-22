# 2369 Check if There is a Valid Partition For The Array 检查数组是否存在有效划分

__Description:__

You are given a __0-indexed__ integer array `nums`. You have to partition the array into one or more __contiguous__ subarrays.

We call a partition of the array valid if each of the obtained subarrays satisfies one of the following conditions:

Return `true` _if the array has __at least__ one valid partition_. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: nums = [4,4,4,5,6]
Output: true
Explanation: The array can be partitioned into the subarrays [4,4] and [4,5,6].
This partition is valid, so we return true.
```

Example 2:

```text
Input: nums = [1,1,1,2]
Output: false
Explanation: There is no valid partition for this array.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` ，你必须将数组划分为一个或多个 __连续__ 子数组。

如果获得的这些子数组中每个都能满足下述条件 之一 ，则可以称其为数组的一种 有效 划分：

如果数组 __至少__ 存在一种有效划分，返回 `true` ，否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：nums = [4,4,4,5,6]
输出：true
解释：数组可以划分成子数组 [4,4] 和 [4,5,6] 。
这是一种有效划分，所以返回 true 。
```

示例 2：

```text
输入：nums = [1,1,1,2]
输出：false
解释：该数组不存在有效划分。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
动态规划
dp[i + 1] 表示 nums[:i] 是否存在有效划分
初始化 dp[0] = True
dp[i + 1] = dp[i - 1] and nums[i] == nums[i - 1] or dp[i - 2] and (nums[i] == nums[i - 1] and nums[i] == nums[i - 2] or nums[i] == nums[i - 1] + 1 and nums[i] == nums[i - 2] + 2)
即要么 nums[i] 和前一个相等
要么 nums[i] 和前两个相等
或者 nums[i] 和前一个和前两个分别加一相等
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool validPartition(vector<int>& nums) 
    {
        vector<bool> dp(nums.size() + 1);
        dp.front() = true;
        for (int i = 0, n = nums.size(); i < n; i++) dp[i + 1] = i and dp[i - 1] and nums[i] == nums[i - 1] or (i > 1 and dp[i - 2] and (nums[i] == nums[i - 1] and nums[i] == nums[i - 2] or nums[i] == nums[i - 1] + 1 and nums[i] == nums[i - 2] + 2));
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public boolean validPartition(int[] nums) {
        boolean[] dp = new boolean[nums.length + 1];
        dp[0] = true;
        for (int i = 0, n = nums.length; i < n; i++) dp[i + 1] = (i > 0 && dp[i - 1] && nums[i] == nums[i - 1]) || (i > 1 && dp[i - 2] && ((nums[i] == nums[i - 1] && nums[i] == nums[i - 2]) || (nums[i] == nums[i - 1] + 1 && nums[i] == nums[i - 2] + 2)));
        return dp[nums.length];
    }
}
```

__Python__:

```Python
class Solution:
    def validPartition(self, nums: List[int]) -> bool:
        dp = [True] + [False] * len(nums)
        for i, num in enumerate(nums):
            dp[i + 1] = i and dp[i - 1] and num == nums[i - 1] or (i > 1 and dp[i - 2] and (num == nums[i - 1] == nums[i - 2] or num == nums[i - 1] + 1 == nums[i - 2] + 2))
        return dp[-1]
```
