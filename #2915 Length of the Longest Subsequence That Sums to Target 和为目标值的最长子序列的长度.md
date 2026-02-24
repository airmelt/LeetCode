# 2915 Length of the Longest Subsequence That Sums to Target 和为目标值的最长子序列的长度

__Description:__

You are given a __0-indexed__ array of integers `nums`, and an integer `target`.

Return _the __length of the longest subsequence__ of_ `nums` _that sums up to_ `target`. _If no such subsequence exists, return_ `-1`.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5], target = 9
Output: 3
Explanation: There are 3 subsequences with a sum equal to 9: [4,5], [1,3,5], and [2,3,4]. The longest subsequences are [1,3,5], and [2,3,4]. Hence, the answer is 3.
```

Example 2:

```text
Input: nums = [4,1,3,2,1,5], target = 7
Output: 4
Explanation: There are 5 subsequences with a sum equal to 7: [4,3], [4,1,2], [4,2,1], [1,1,5], and [1,3,2,1]. The longest subsequence is [1,3,2,1]. Hence, the answer is 4.
```

Example 3:

```text
Input: nums = [1,1,5,4,5], target = 3
Output: -1
Explanation: It can be shown that nums has no subsequence that sums up to 3.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `1 <= target <= 1000`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `target` 。

返回和为 `target` 的 `nums` 子序列中，子序列 __长度的最大值__ 。如果不存在和为 `target` 的子序列，返回 `-1` 。

子序列 指的是从原数组中删除一些或者不删除任何元素后，剩余元素保持原来的顺序构成的数组。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5], target = 9
输出：3
解释：总共有 3 个子序列的和为 9 ：[4,5] ，[1,3,5] 和 [2,3,4] 。最长的子序列是 [1,3,5] 和 [2,3,4] 。所以答案为 3 。
```

示例 2：

```text
输入：nums = [4,1,3,2,1,5], target = 7
输出：4
解释：总共有 5 个子序列的和为 7 ：[4,3] ，[4,1,2] ，[4,2,1] ，[1,1,5] 和 [1,3,2,1] 。最长子序列为 [1,3,2,1] 。所以答案为 4 。
```

示例 3：

```text
输入：nums = [1,1,5,4,5], target = 3
输出：-1
解释：无法得到和为 3 的子序列。
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 1000`
- `1 <= target <= 1000`

__思路:__

```text
动态规划
0 - 1 背包
设 dp[i] 表示和为 i 的最长子序列的长度
则 dp[i] = max(dp[j]) + 1, 其中 0 <= j < i
初始化 dp[0] = 0, 即和为 0 的子序列长度为 0
时间复杂度为 O(MN), 空间复杂度为 O(M), M 为 target 的值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int lengthOfLongestSubsequence(vector<int>& nums, int target) 
    {
        vector<int> dp(target + 1, -0x3f3f3f3f);
        dp.front() = 0;
        for (const auto& num : nums) for (int i = target; i >= num; i--) dp[i] = max(dp[i], dp[i - num] + 1);
        return dp.back() > 0 ? dp.back() : -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int lengthOfLongestSubsequence(List<Integer> nums, int target) {
        int inf = 0x3f3f3f3f, dp[] = new int[target + 1];
        Arrays.fill(dp, -inf);
        dp[0] = 0;
        for (int num : nums) for (int i = target; i >= num; i--) dp[i] = Math.max(dp[i], dp[i - num] + 1);
        return dp[target] > 0 ? dp[target] : -1;
    }
}
```

__Python__:

```Python
class Solution:
    def lengthOfLongestSubsequence(self, nums: List[int], target: int) -> int:
        dp = [0] + [-inf] * target
        for num in nums:
            for i in range(target, num - 1, -1):
                dp[i] = max(dp[i], dp[i - num] + 1)
        return dp[-1] if dp[-1] > 0 else -1
```
