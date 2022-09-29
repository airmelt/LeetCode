# 1330 Reverse Subarray To Maximize Array Value 翻转子数组得到最大的数组值

__Description:__

You are given an integer array nums. The value of this array is defined as the sum of |nums[i] - nums[i + 1]| for all 0 <= i < nums.length - 1.

You are allowed to select any subarray of the given array and reverse it. You can perform this operation only once.

Find maximum possible value of the final array.

__Example:__

Example 1:

Input: nums = [2,3,1,5,4]
Output: 10
Explanation: By reversing the subarray [3,1,5] the array becomes [2,5,1,3,4] whose value is 10.

Example 2:

Input: nums = [2,4,9,24,2,1,10]
Output: 68

__Constraints:__

1 <= nums.length <= 3 * 10^4
-10^5 <= nums[i] <= 10^5

__题目描述：__

给你一个整数数组 nums 。「数组值」定义为所有满足 0 <= i < nums.length-1 的 |nums[i]-nums[i+1]| 的和。

你可以选择给定数组的任意子数组，并将该子数组翻转。但你只能执行这个操作 一次 。

请你找到可行的最大 数组值 。

__示例：__

示例 1：

输入：nums = [2,3,1,5,4]
输出：10
解释：通过翻转子数组 [3,1,5] ，数组变成 [2,5,1,3,4] ，数组值为 10 。

示例 2：

输入：nums = [2,4,9,24,2,1,10]
输出：68

__提示：__

1 <= nums.length <= 3*10^4
-10^5 <= nums[i] <= 10^5

__思路：__

数学
注意到翻转之后只有子数组首尾的值会对绝对值产生影响
不妨设 nums[i - 1] = a, nums[i] = b, nums[j] = c, nums[j + 1] = d
翻转的区间为 [i, j]
翻转之后值的变化为 abs(c - a) + abs(d - b) - abs(b - a) - abs(d - c)
若 a < c, b > d, 原式 = (c - a) + (b - d) - abs(b - a) - abs(d - c) = (c - d) - abs(c - d) + (b - a) - abs(b - a) < 0
若 a > c, b < d, 原式 = (a - c) + (d - b) - abs(b - a) - abs(d - c) = (d - c) - abs(d - c) + (a - b) - abs(a - b) < 0
若 a > c, b > d, 原式 = (a - c) + (b - d) - abs(a - b) - abs(d - c) = (a + b) - abs(a - b) - (c + d) - abs(c - d) = 2min(a, b) - 2max(c, d)
若 a < c, b < d, 原式 = (c - a) + (d - b) - abs(a - b) - abs(d - c) = (c + d) - abs(c - d) - (a + b) - abs(a - b) = 2min(c, d) - 2max(a, b)
也就是说先算出 sum_value = sum(nums[i] - nums[i + 1])
然后计算翻转的最大值, 同时计算 2 个相邻数的最大值和最小值
最后取翻转的最大值以及相邻数最大值和最小值的差值的两倍即可
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxValueAfterReverse(vector<int>& nums) 
    {
        int n = nums.size(), sum_value = 0, max_value = 0, a = INT_MIN, b = INT_MAX;
        for (int i = 0; i < n - 1; i++) sum_value += abs(nums[i] - nums[i + 1]);
        for (int i = 1; i < n - 1; i++) max_value = max(max_value, abs(nums[i + 1] - nums.front()) - abs(nums[i + 1] - nums[i]));
        for (int i = 1; i < n - 1; i++) max_value = max(max_value, abs(nums[i - 1] - nums.back()) - abs(nums[i - 1] - nums[i]));
        for (int i = 1; i < n; i++)
        {
            a = max(a, min(nums[i], nums[i - 1]));
            b = min(b, max(nums[i], nums[i - 1]));
        }
        return max(max_value, (max(a - b, 0) << 1)) + sum_value;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxValueAfterReverse(int[] nums) {
        int n = nums.length, sumValue = 0, maxValue = 0, a = Integer.MIN_VALUE, b = Integer.MAX_VALUE;
        for (int i = 0; i < n - 1; i++) sumValue += Math.abs(nums[i] - nums[i + 1]);
        for (int i = 1; i < n - 1; i++) maxValue = Math.max(maxValue, Math.abs(nums[i + 1] - nums[0]) - Math.abs(nums[i + 1] - nums[i]));
        for (int i = 1; i < n - 1; i++) maxValue = Math.max(maxValue, Math.abs(nums[i - 1] - nums[n - 1]) - Math.abs(nums[i - 1] - nums[i]));
        for (int i = 1; i < n; i++)
        {
            a = Math.max(a, Math.min(nums[i], nums[i - 1]));
            b = Math.min(b, Math.max(nums[i], nums[i - 1]));
        }
        return Math.max(maxValue, (Math.max(a - b, 0) << 1)) + sumValue;
    }
}
```

__Python__:

```Python
class Solution:
    def maxValueAfterReverse(self, nums: List[int]) -> int:
        n, sum_value, max_value, a, b = len(nums), 0, 0, -float('inf'), float('inf')
        for i in range(n - 1):
            sum_value += abs(nums[i] - nums[i + 1])
            max_value, a, b = max(max_value, abs(nums[i + 1] - nums[0]) - abs(nums[i + 1] - nums[i]), abs(nums[i - 1] - nums[-1]) - abs(nums[i - 1] - nums[i])), max(a, min(nums[i], nums[i + 1])), min(b, max(nums[i], nums[i + 1]))
        return max(max_value, (max(a - b, 0) << 1)) + sum_value
```
