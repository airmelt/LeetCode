__Description__:
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

__Example:__
Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

__题目描述__:
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

__示例 :__
示例 1:

输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4

示例 2:

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

__思路__:
1. 暴力法, 逐个查找
时间复杂度O(n), 空间复杂度O(1)
2. 旋转之后的数组也是部分有序的, 可以用类似二分查找的方法查找
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int search(vector<int>& nums, int target) 
    {
        int low = 0, high = nums.size() - 1;
        while (low <= high)
        {
            int mid = ((high - low) >> 1) + low;
            if (nums[mid] == target) return mid;
            else if (nums[mid] < nums[high])
            {
                if (nums[mid] < target && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
            else
            {
                if (nums[mid] > target && target >= nums[low]) high = mid - 1;
                else low = mid + 1;
            }
        }
        return -1;
    }
};
```

__Java__:
```Java
class Solution {
    public int search(int[] nums, int target) {
        int result = -1;
        for (int i = 0; i < nums.length; i++) if (nums[i] == target) result = i;
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        return nums.index(target) if target in nums else -1
```