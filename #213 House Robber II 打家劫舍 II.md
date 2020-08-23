__Description__:
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

__Example:__
Example 1:

Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.

Example 2:

Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.

__题目描述__:
你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

__示例 :__
示例 1:

输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。

示例 2:

输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。

__思路__:
参考[https://www.jianshu.com/p/617078f0fbb5](LeetCode #198 House Robber 打家劫舍)
动态规划
dp[i]表示前 i天能偷到的最大价值
转移方程 dp[i] = max(dp[i - 1], dp[i - 1] + nums[i])
这里由于最后一个和第一个相邻, 可以考虑分别从第一个开始不包括最后一个和第二个开始到最后一个
由于只要用到 dp[i - 1], dp[i - 2]可以优化到 O(1)的空间
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int rob(vector<int>& nums) 
    {
        if (nums.empty()) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> nums1(nums.begin(), nums.end() - 1), nums2(nums.begin() + 1, nums.end());
        return max(helper(nums1), helper(nums2));
    }
private:
    int helper(vector<int>& nums) 
    {
        int pre = 0, cur = 0, temp;
        for(int num : nums) {
            temp = cur;
            cur = max(pre + num, cur);
            pre = temp;
        }
        return cur;
    }
};
```

__Java__:
```Java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) return 0;
        if (nums.length == 1) return nums[0];
        return Math.max(helper(Arrays.copyOfRange(nums, 0, nums.length - 1)), helper(Arrays.copyOfRange(nums, 1, nums.length)));
    }
    
    private int helper(int[] nums) {
        int pre = 0, cur = 0, temp;
        for(int num : nums) {
            temp = cur;
            cur = Math.max(pre + num, cur);
            pre = temp;
        }
        return cur;
    }
}
```

__Python__:
```Python
class Solution:
    def rob(self, nums: List[int]) -> int:
        def helper(nums: List[int]) -> int:
            cur, pre = 0, 0
            for num in nums:
                cur, pre = max(pre + num, cur), cur
            return cur
        return max(helper(nums[:-1]), helper(nums[1:])) if len(nums) != 1 else nums[0]
```