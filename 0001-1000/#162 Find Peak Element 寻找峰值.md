# 162 Find Peak Element 寻找峰值

__Description__:
A peak element is an element that is greater than its neighbors.

Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that nums[-1] = nums[n] = -∞.

__Example:__

Example 1:

Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.

Example 2:

Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.

__Follow up:__
Your solution should be in logarithmic complexity.

__题目描述__:
峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 nums，其中 nums[i] ≠ nums[i+1]，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞。

__示例 :__

示例 1:

输入: nums = [1,2,3,1]
输出: 2
解释: 3 是峰值元素，你的函数应该返回其索引 2。

示例 2:

输入: nums = [1,2,1,3,5,6,4]
输出: 1 或 5
解释: 你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。

__说明：__

你的解法应该是 O(logN) 时间复杂度的。

__思路__:

1. 逐个搜索
搜索到第一个递减的就是答案
时间复杂度O(n), 空间复杂度O(1)
2. 由于题目要求对数时间复杂度, 考虑二分法
由于数组中 nums[i] != nums[i + 1],
若nums[mid] > nums[mid + 1](注意 nums[mid] != nums[mid + 1]), 则 mid有可能是 peak, 去掉右半部分, 即 right = mid
否则, 去掉左半部分, 即 left = mid + 1
时间复杂度O(lgn), 空间复杂度O(1)
3. 由于最大值一定是峰值
直接返回最大值下标即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findPeakElement(vector<int>& nums) 
    {
        int left = 0, right = nums.size() - 1;
        while (left < right)
        {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] > nums[mid + 1]) right = mid;
            else left = mid + 1;
        }
        return left;
    }
};
```

__Java__:

```Java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right)
        {
            int mid = left + ((right - left) >>> 1);
            if (nums[mid] > nums[mid + 1]) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

__Python__:

```Python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        return nums.index(max(nums))
```
