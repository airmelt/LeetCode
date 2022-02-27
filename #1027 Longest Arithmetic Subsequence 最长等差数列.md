# 1027 Longest Arithmetic Subsequence 最长等差数列

__Description__:
Given an array nums of integers, return the length of the longest arithmetic subsequence in nums.

Recall that a subsequence of an array nums is a list nums[i1], nums[i2], ..., nums[ik] with 0 <= i1 < i2 < ... < ik <= nums.length - 1, and that a sequence seq is arithmetic if seq[i+1] - seq[i] are all the same value (for 0 <= i < seq.length - 1).

__Example:__

Example 1:

Input: nums = [3,6,9,12]
Output: 4
Explanation:
The whole array is an arithmetic sequence with steps of length = 3.

Example 2:

Input: nums = [9,4,7,2,10]
Output: 3
Explanation:
The longest arithmetic subsequence is [4,7,10].

Example 3:

Input: nums = [20,1,15,3,10,5,8]
Output: 4
Explanation:
The longest arithmetic subsequence is [20,15,10,5].

__Constraints:__

2 <= nums.length <= 1000
0 <= nums[i] <= 500

__题目描述__:
给你一个整数数组 nums，返回 nums 中最长等差子序列的长度。

回想一下，nums 的子序列是一个列表 nums[i1], nums[i2], ..., nums[ik] ，且 0 <= i1 < i2 < ... < ik <= nums.length - 1。并且如果 seq[i+1] - seq[i]( 0 <= i < seq.length - 1) 的值都相同，那么序列 seq 是等差的。

__示例 :__

示例 1：

输入：nums = [3,6,9,12]
输出：4
解释：
整个数组是公差为 3 的等差数列。

示例 2：

输入：nums = [9,4,7,2,10]
输出：3
解释：
最长的等差子序列是 [4,7,10]。

示例 3：

输入：nums = [20,1,15,3,10,5,8]
输出：4
解释：
最长的等差子序列是 [20,15,10,5]。

__提示:__

2 <= nums.length <= 1000
0 <= nums[i] <= 500

__思路__:

动态规划
dp[i][d] 表示以 nums[i] 结尾公差为 d 的等差数列的长度
令 p = nums[i] - nums[j] + 500, 500 是为了使 p 为正数
dp[i][d] = max(nums[i][d], nums[j][d] + 1), 其中 0 <= j < i
时间复杂度为 O(n ^ 2), 空间复杂度为 O(nd), 其中 d 表示 max(nums) - min(nums)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestArithSeqLength(vector<int>& nums) 
    {
        int result = 0, n = nums.size();
        vector<vector<int>> dp(1001, vector<int>(1001));
        for (int i = 0; i < n; i++) 
        {
            for (int j = 0; j < i; j++) 
            {
                int d = nums[i] - nums[j] + 500;
                dp[i][d] = max(dp[i][d], dp[j][d] + 1);
                result = max(dp[i][d], result);
            }
        }
        return result + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestArithSeqLength(int[] nums) {
        int dp[][] = new int[1001][1001], result = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                int d = nums[i] - nums[j] + 500;
                dp[i][d] = Math.max(dp[i][d], dp[j][d] + 1);
                result = Math.max(dp[i][d], result);
            }
        }
        return result + 1;
    }
}
```

__Python__:

```Python
class Solution:
    def longestArithSeqLength(self, nums: List[int]) -> int:
        dp, n, result = [[0] * 1001 for _ in range(1001)], len(nums), 0
        for i in range(n):
            for j in range(i):
                dp[i][d] = max(dp[i][(d := nums[i] - nums[j] + 500)], dp[j][d] + 1)
                result = max(result, dp[i][d])
        return result + 1
```
