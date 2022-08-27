# 1283 Find the Smallest Divisor Given a Threshold 使结果不超过阈值的最小除数

__Description:__

Given an array of integers nums and an integer threshold, we will choose a positive integer divisor, divide all the array by it, and sum the division's result. Find the smallest divisor such that the result mentioned above is less than or equal to threshold.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: 7/3 = 3 and 10/2 = 5).

The test cases are generated so that there will be an answer.

__Example:__

Example 1:

Input: nums = [1,2,5,9], threshold = 6
Output: 5
Explanation: We can get a sum to 17 (1+2+5+9) if the divisor is 1.
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2).
Example 2:

Input: nums = [44,22,33,11,1], threshold = 5
Output: 44

__Constraints:__

1 <= nums.length <= 5 * 10^4
1 <= nums[i] <= 10^6
nums.length <= threshold <= 10^6

__题目描述：__

给你一个整数数组 nums 和一个正整数 threshold  ，你需要选择一个正整数作为除数，然后将数组里每个数都除以它，并对除法结果求和。

请你找出能够使上述结果小于等于阈值 threshold 的除数中 最小 的那个。

每个数除以除数后都向上取整，比方说 7/3 = 3 ， 10/2 = 5 。

题目保证一定有解。

__示例：__

示例 1：

输入：nums = [1,2,5,9], threshold = 6
输出：5
解释：如果除数为 1 ，我们可以得到和为 17 （1+2+5+9）。
如果除数为 4 ，我们可以得到和为 7 (1+1+2+3) 。如果除数为 5 ，和为 5 (1+1+1+2)。

示例 2：

输入：nums = [2,3,5,7,11], threshold = 11
输出：3

示例 3：

输入：nums = [19], threshold = 5
输出：4

__提示：__

1 <= nums.length <= 5 * 10^4
1 <= nums[i] <= 10^6
nums.length <= threshold <= 10^6

__思路：__

二分查找
由于 threshold >= nums.length
说明结果至少为 max(nums)
查找区间为 [1, max(nums)]
地板除只要余数不为 0 就加 1
时间复杂度为 O(mn), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int smallestDivisor(vector<int>& nums, int threshold) 
    {
        int left = 1, right = *max_element(nums.begin(), nums.end());
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (check(nums, mid, threshold)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
private:
    bool check(vector<int>& nums, int mid, int threshold) 
    {
        int sum = 0;
        for (const auto& num : nums) if ((sum += num / mid + (num % mid ? 1 : 0)) > threshold) return false;
        return true;   
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestDivisor(int[] nums, int threshold) {
        int left = 1, right = 1_000_000;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(nums, mid, threshold)) right = mid;
            else left = mid + 1;
        }
        return left;
    }
    
    private boolean check(int[] nums, int mid, int threshold) {
        int sum = 0;
        for (int num : nums) if ((sum = sum + num / mid + (num % mid == 0 ? 0 : 1)) > threshold) return false;
        return true;          
    }
}
```

__Python__:

```Python
class Solution:
    def smallestDivisor(self, nums: List[int], threshold: int) -> int:
        return 10 ** 6 - bisect_right(range(10 ** 6, 0, -1), threshold, key=lambda div: sum(ceil(i / div) for i in nums)) + 1
```
