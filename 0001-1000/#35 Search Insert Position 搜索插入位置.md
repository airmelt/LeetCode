# 35 Search Insert Position 搜索插入位置

__Description__:
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

__Example__:

Example 1:
Input: [1,3,5,6], 5
Output: 2

Example 2:
Input: [1,3,5,6], 2
Output: 1

Example 3:
Input: [1,3,5,6], 7
Output: 4

Example 4:
Input: [1,3,5,6], 0
Output: 0

__题目描述__:
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

__示例__:

示例 1:
输入: [1,3,5,6], 5
输出: 2

示例 2:
输入: [1,3,5,6], 2
输出: 1

示例 3:
输入: [1,3,5,6], 7
输出: 4

示例 4:
输入: [1,3,5,6], 0
输出: 0

__思路__:

由于数组已排序, 最快的方法为二分查找
时间复杂度为O(lgn), 空间复杂度为O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int searchInsert(vector<int>& nums, int target) 
    {
        // return lower_bound(nums.begin(), nums.end(), target) - nums.begin();
        if (!nums.size()) return 0;
        int low = 0, high = nums.size() - 1;
        while (low <= high) 
        {
            // 这里其实有low/high越界的问题, 最好的方式是用无符号右移
            int mid = low + ((high - low) >> 1);
            if (nums[mid] >= target) high = mid - 1;
            else low = mid + 1;
        }
        return low;
    }
};
```

__Java__:

```Java
class Solution {
    public int searchInsert(int[] nums, int target) {
        return Arrays.binarySearch(nums, target) >= 0 ? Arrays.binarySearch(nums, target) : -(Arrays.binarySearch(nums, target) + 1);
    }
}
```

__Python__:

```Python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        # return bisect.bisect_left(nums, target)
        low, high = 0, len(nums) - 1
        while (low <= high):
            mid = (high + low) // 2
            if (nums[mid] >= target):
                high = mid - 1
            else:
                low = mid + 1
        return low
```
