# 713 Subarray Product Less Than K 乘积小于K的子数组

__Description__:
Given an array of integers nums and an integer k, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than k.

__Example:__

Example 1:

Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

Example 2:

Input: nums = [1,2,3], k = 0
Output: 0

__Constraints:__

1 <= nums.length <= 3 * 10^4
1 <= nums[i] <= 1000
0 <= k <= 10^6

__题目描述__:
给定一个正整数数组 nums。

找出该数组内乘积小于 k 的连续的子数组的个数。

__示例 :__

示例 1:

输入: nums = [10,5,2,6], k = 100
输出: 8
解释: 8个乘积小于100的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
需要注意的是 [10,5,2] 并不是乘积小于100的子数组。

__说明:__

0 < nums.length <= 50000
0 < nums[i] < 1000
0 <= k < 10^6

__思路__:

双指针法
假设 cur = nums[left] \* nums[left + 1] \* ... \* nums[right - 1] \* nums[right]
则如果 cur < k, 说明以 nums[left:right + 1] 之间的子数组都满足
result += right - left + 1(区间长度)
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) 
    {
        int result = 0, left = 0, n = nums.size(), cur = 1;
        if (!k or k == 1) return result;
        for (int right = 0; right < n; right++) 
        {
            cur *= nums[right];
            while (cur >= k) cur /= nums[left++];
            result += right - left + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int result = 0, left = 0, n = nums.length, cur = 1;
        if (k == 0 || k == 1) return result;
        for (int right = 0; right < n; right++) {
            cur *= nums[right];
            while (cur >= k) cur /= nums[left++];
            result += right - left + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        result, left, n, cur = 0, 0, len(nums), 1
        if not k or k == 1:
            return result
        for right in range(n):
            cur *= nums[right]
            while cur >= k:
                cur //= nums[left]
                left += 1
            result += right - left + 1
        return result
```
