__Description__:
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

The array may contain duplicates.

__Example:__
Example 1:

Input: [1,3,5]
Output: 1

Example 2:

Input: [2,2,2,0,1]
Output: 0

__Note:__
This is a follow up problem to Find Minimum in Rotated Sorted Array.
Would allow duplicates affect the run-time complexity? How and why?

__题目描述__:
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

请找出其中最小的元素。

注意数组中可能存在重复的元素。

__示例 :__
示例 1：

输入: [1,3,5]
输出: 1

示例 2：

输入: [2,2,2,0,1]
输出: 0

__说明：__

这道题是 寻找旋转排序数组中的最小值 的延伸题目。
允许重复会影响算法的时间复杂度吗？会如何影响，为什么？

__思路__:
参考[LeetCode #153 Find Minimum in Rotated Sorted Array 寻找旋转排序数组中的最小值](https://www.jianshu.com/p/ec8de2741d87)
1. 逐个搜索
时间复杂度O(n), 空间复杂度O(1)
2. 二分法
当nums[right] == nums[mid]时, --right
其他的思路跟上一题一样
时间复杂度O(n), 空间复杂度O(1), 平均时间复杂度O(lgn)

__代码__:
__C++__:
```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] > nums[right]) left = mid + 1;
            else if (nums[mid] < nums[right]) right = mid;
            else if (nums[mid] == nums[right]) --right;
        }
        return nums[left];
    }
};
```

__Java__:
```Java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (nums[mid] > nums[right]) left = mid + 1;
            else if (nums[mid] < nums[right]) right = mid;
            else if (nums[mid] == nums[right]) --right;
        }
        return nums[left];
    }
}
```

__Python__:
```Python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        return min(nums)
```