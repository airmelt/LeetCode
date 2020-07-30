__Description__:
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

You may assume no duplicate exists in the array.

__Example:__
Example 1:

Input: [3,4,5,1,2] 
Output: 1

Example 2:

Input: [4,5,6,7,0,1,2]
Output: 0

__题目描述__:
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

请找出其中最小的元素。

你可以假设数组中不存在重复元素。

__示例 :__
示例 1:

输入: [3,4,5,1,2]
输出: 1

示例 2:

输入: [4,5,6,7,0,1,2]
输出: 0

__思路__:
1. 可以逐个搜索最小值
搜索到第一次增加的下标, 后一个即为最小值
时间复杂度O(n), 空间复杂度O(1)
2. 由于旋转数组本身部分有序考虑用二分法
由于是查找最小值, 所以, 偏向于在左边查找
中间与右边界比较, 如果小于右边界 right = mid
否则, left = mid + 1(这里 + 1是因为 mid比 right大说明一定不是 mid)
时间复杂度O(lgn), 空间复杂度O(1)

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
            if (nums[mid] < nums[right]) right = mid;
            else left = mid + 1;
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
            if (nums[mid] < nums[right]) right = mid;
            else left = mid + 1;
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