# 81 Search in Rotated Sorted Array II 搜索旋转排序数组 II

__Description__:
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

__Example:__

Example 1:

Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true

Example 2:

Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false

__Follow up:__

This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
Would this affect the run-time complexity? How and why?

__题目描述__:
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,0,1,2,2,5,6] 可能变为 [2,5,6,0,0,1,2] )。

编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 true，否则返回 false。

__示例 :__

示例 1:

输入: nums = [2,5,6,0,0,1,2], target = 0
输出: true

示例 2:

输入: nums = [2,5,6,0,0,1,2], target = 3
输出: false

__进阶:__

这是 搜索旋转排序数组 的延伸题目，本题中的 nums  可能包含重复元素。
这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？

__思路__:

参考[LeetCode #33 Search in Rotated Sorted Array 搜索旋转排序数组](https://www.jianshu.com/p/17588a98f42c)
增加两行去重
平均时间复杂度 O(lgn), 最差时间复杂度 O(n), 当数组全是重复元素时需要逐个检查
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool search(vector<int>& nums, int target) 
    {
        int low = 0, high = nums.size() - 1;
        while (low <= high)
        {
            while (low < high and nums[high - 1] == nums[high]) --high;
            while (low < high and nums[low + 1] == nums[low]) ++low;
            int mid = ((high - low) >> 1) + low;
            if (nums[mid] == target) return true;
            else if (nums[mid] < nums[high])
            {
                if (nums[mid] < target and target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
            else
            {
                if (nums[mid] > target and target >= nums[low]) high = mid - 1;
                else low = mid + 1;
            }
        }
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while (low <= high) {
            while (low < high && nums[high - 1] == nums[high]) --high;
            while (low < high && nums[low + 1] == nums[low]) ++low;
            int mid = ((high - low) >>> 1) + low;
            if (nums[mid] == target) return true;
            else if (nums[mid] < nums[high]) {
                if (nums[mid] < target && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
            else {
                if (nums[mid] > target && target >= nums[low]) high = mid - 1;
                else low = mid + 1;
            }
        }
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        return target in nums
```
