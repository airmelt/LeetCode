# 1262 Greatest Sum Divisible by Three 可被三整除的最大和

__Description:__

Given an integer array nums, return the maximum possible sum of elements of the array such that it is divisible by three.

__Example:__

Example 1:

Input: nums = [3,6,5,1,8]
Output: 18
Explanation: Pick numbers 3, 6, 1 and 8 their sum is 18 (maximum sum divisible by 3).

Example 2:

Input: nums = [4]
Output: 0
Explanation: Since 4 is not divisible by 3, do not pick any number.

Example 3:

Input: nums = [1,2,3,4,4]
Output: 12
Explanation: Pick numbers 1, 3, 4 and 4 their sum is 12 (maximum sum divisible by 3).

__Constraints:__

1 <= nums.length <= 4 * 10^4
1 <= nums[i] <= 10^4

__题目描述：__

给你一个整数数组 nums，请你找出并返回能被三整除的元素最大和。

__示例：__

示例 1：

输入：nums = [3,6,5,1,8]
输出：18
解释：选出数字 3, 6, 1 和 8，它们的和是 18（可被 3 整除的最大和）。

示例 2：

输入：nums = [4]
输出：0
解释：4 不能被 3 整除，所以无法选出数字，返回 0。

示例 3：

输入：nums = [1,2,3,4,4]
输出：12
解释：选出数字 1, 3, 4 以及 4，它们的和是 12（可被 3 整除的最大和）。

__提示：__

1 <= nums.length <= 4 * 10^4
1 <= nums[i] <= 10^4

__思路：__

动态规划
设 dp[i][j] 表示 nums[:i] 除以 3 余数为 j 的最大的和
初始化 dp[0][0] = 0, dp[0][1] = dp[0][2] = -1
对每个数都可以选或者不选, 对应有 dp[i][j] = max(dp[i - 1][j], dp[i - 1][(j - nums[i]) % 3] + nums[i])
为了避免 (j - nums[i]) 可能存在负数, 可以改写为
dp[i][(j + nums[i]) % 3] = max(dp[i - 1][(j + nums[i]) % 3], dp[i - 1][j] + num)
注意到 dp[i] 只与 dp[i - 1] 有关, 可以用滚动数组压缩空间复杂度到 O(1)
还原时间复杂度为 O(1), 空间复杂度为 O(1), 查找时间复杂度 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxSumDivThree(vector<int>& nums)
    {
        vector<int> dp(3);
        dp[1] = dp[2] = INT_MIN;
        for (const auto& num : nums)
        {
            vector<int> dp2(3);
            for (int i = 0; i < 3; i++) dp2[(i + num) % 3] = max(dp[(i + num) % 3], dp[i] + num);
            dp = dp2;
        }
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSumDivThree(int[] nums) {
        int[] dp = new int[3];
        dp[1] = dp[2] = Integer.MIN_VALUE;
        for (int num : nums) {
            int[] dp2 = new int[3];
            for (int i = 0; i < 3; i++) dp2[(i + num) % 3] = Math.max(dp[(i + num) % 3], dp[i] + num);
            dp = dp2;
        }
        return dp[0];
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumDivThree(self, nums: List[int]) -> int:
        dp = [0, -float('inf'), -float('inf')]
        for num in nums:
            dp2 = [0] * 3
            for i in range(3):
                dp2[(i + num) % 3] = max(dp[(i + num) % 3], dp[i] + num)
            dp = dp2
        return dp[0]
```
