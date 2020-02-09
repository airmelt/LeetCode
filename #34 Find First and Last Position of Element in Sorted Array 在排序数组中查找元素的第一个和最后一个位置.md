__Description__:
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

__Example:__
Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

__题目描述__:
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

__示例 :__
示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]

示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]

__思路__:
由于数组有序, 可以使用二分查找, 分别查找左边界和右边界
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> searchRange(vector<int>& nums, int target) 
    {
        vector<int> result(2, -1);
        if (nums.empty()) return result;
        int n = nums.size(), left = 0, right = n;
        while (left < right)
        {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] < target) left = mid + 1;
            else right = mid;
        }
        if (left != n && nums[left] == target) result[0] = left;
        left = 0;
        right = n;
        while (left < right)
        {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] > target) right = mid;
            else left = mid + 1;
        }
        if (left != 0 && nums[--left] == target) result[1] = left;
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int result[] = new int[]{-1, -1}, n = nums.length;
        if (n == 0) return result;
        int left = 0, right = n;
        while (left < right) {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] < target) left = mid + 1;
            else right = mid;
        }
        if (left != n && nums[left] == target) result[0] = left;
        left = 0;
        right = n;
        while (left < right) {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] > target) right = mid;
            else left = mid + 1;
        }
        if (left != 0 && nums[--left] == target) result[1] = left;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        return [-1 if target not in nums else nums.index(target), -1 if target not in nums else len(nums) - nums[::-1].index(target) - 1]
```