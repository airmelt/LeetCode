__Description__:
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

__Example:__

nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
 

__Follow up:__
What if negative numbers are allowed in the given array?
How does it change the problem?
What limitation we need to add to the question to allow negative numbers?

__Credits:__
Special thanks to @pbrother for adding this problem and creating all test cases.

__题目描述__:
给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

__示例 :__

nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。

__进阶:__
如果给定的数组中含有负数会怎么样？
问题会产生什么变化？
我们需要在题目中添加什么限制来允许负数的出现？

__致谢：__
特别感谢 @pbrother 添加此问题并创建所有测试用例。

__思路__:
动态规划
dp[i]表示 i用 nums中的组合的数量
dp[i] = sum(dp[i - num])
比如, nums = [1, 2, 4], target = 7
dp[7] = dp[1] + dp[2] + dp[4]
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int combinationSum4(vector<int>& nums, int target) 
    {
        vector<unsigned int> dp(target + 1);
        dp[0] = 1;
        for (int i = 1; i < target + 1; i++) for (auto num : nums) if (i >= num) dp[i] += dp[i - num];
        return dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int dp[] = new int[target + 1];
        dp[0] = 1;
        for (int i = 1; i < target + 1; i++) for (int num : nums) if (i >= num) dp[i] += dp[i - num];
        return dp[target];
    }
}
```

__Python__:
```Python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [1] + [0] * target
        for i in range(1, target + 1):
            for num in nums:
                if num <= i:
                    dp[i] += dp[i - num]
        return dp[-1]
```