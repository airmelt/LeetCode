# 740 Delete and Earn 删除并获得点数

__Description__:
You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times:

Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1.
Return the maximum number of points you can earn by applying the above operation some number of times.

__Example:__

Example 1:

Input: nums = [3,4,2]
Output: 6
Explanation: You can perform the following operations:

- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.

Example 2:

Input: nums = [2,2,3,3,3,4]
Output: 9
Explanation: You can perform the following operations:

- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.

__Constraints:__

1 <= nums.length <= 2 * 10^4
1 <= nums[i] <= 10^4

__题目描述__:
给你一个整数数组 nums ，你可以对它进行一些操作。

每次操作中，选择任意一个 nums[i] ，删除它并获得 nums[i] 的点数。之后，你必须删除 所有 等于 nums[i] - 1 和 nums[i] + 1 的元素。

开始你拥有 0 个点数。返回你能通过这些操作获得的最大点数。

__示例 :__

示例 1：

输入：nums = [3,4,2]
输出：6
解释：
删除 4 获得 4 个点数，因此 3 也被删除。
之后，删除 2 获得 2 个点数。总共获得 6 个点数。

示例 2：

输入：nums = [2,2,3,3,3,4]
输出：9
解释：
删除 3 获得 3 个点数，接着要删除两个 2 和 4 。
之后，再次删除 3 获得 3 个点数，再次删除 3 获得 3 个点数。
总共获得 9 个点数。

__提示:__

1 <= nums.length <= 2 * 10^4
1 <= nums[i] <= 10^4

__思路__:

动态规划
注意到相同数字会被同时全部删除, 考虑求出相同数字总和
取数组中元素的最大值, 建立数组各元素和的统计数组
然后可以参考 [LeetCode #198 House Robber 打家劫舍](https://www.jianshu.com/p/617078f0fbb5)
求解
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int deleteAndEarn(vector<int>& nums) 
    {
        int m = *max_element(nums.begin(), nums.end());
        vector<int> dp(m + 1);
        for (const auto& num : nums) dp[num] += num;
        for (int i = 2; i <= m; i++) dp[i] = max(dp[i - 1], dp[i - 2] + dp[i]);
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int deleteAndEarn(int[] nums) {
        int m = 0;
        for (int num : nums) if (num > m) m = num;
        int[] dp = new int[m + 1];
        for (int num : nums) dp[num] += num;
        for (int i = 2; i <= m; i++) dp[i] = Math.max(dp[i - 1], dp[i - 2] + dp[i]);
        return dp[m];
    }
}
```

__Python__:

```Python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        dp = [0] * ((m := max(nums)) + 1)
        for num in nums:
            dp[num] += num
        for i in range(2, m + 1):
            dp[i] = max(dp[i - 1], dp[i - 2] + dp[i])
        return dp[-1]
```
