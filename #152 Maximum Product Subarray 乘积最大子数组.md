__Description__:
Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

__Example:__
Example 1:

Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Example 2:

Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

__题目描述__:
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

__示例 :__
示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。

示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

__思路__:
动态规划
dp[i]表示以 nums[i]结尾的子数组最大乘积
转移方程 dp[i] = max(dp[i - 1] * nums[i], nums[i]), 这里实际上用一个变量就可以记录最大值
考虑到 nums[i]如果为负数, 那么乘积之后到最大值和最小值会交换, 需要同时记录最大值和最小值
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxProduct(vector<int>& nums) 
    {
        int result = nums[0], a = 1, b = 1;
        for (auto num : nums)
        {
            if (num < 0) swap(a, b);
            a = max(a * num, num);
            b = min(b * num, num);
            result = max(a, result);
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxProduct(int[] nums) {
        int result = nums[0], max = 1, min = 1;
        for (int num : nums) {
            if (num < 0) {
                max ^= min;
                min ^= max;
                max ^= min;
            }
            max = Math.max(max * num, num);
            min = Math.min(min * num, num);
            result = Math.max(max, result);
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        return max(functools.reduce(lambda x, y: [max(x[0] * y, x[1] * y, y), min(x[1] * y, x[0] * y, y), max(x)], nums[1:], [nums[0], nums[0], nums[0]]))
```