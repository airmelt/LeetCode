__Description__:
Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

__Note:__
 You are not suppose to use the library's sort function for this problem.

__Example:__

Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

__Follow up:__

A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
Could you come up with a one-pass algorithm using only constant space?

__题目描述__:
给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

__注意:__
不能使用代码库中的排序函数来解决这道题。

__示例 :__

输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]

__进阶：__

一个直观的解决方案是使用计数排序的两趟扫描算法。
首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
你能想出一个仅使用常数空间的一趟扫描算法吗？

__思路__:
双指针
left指向 1的左边, right指向 1的右边, i指向当前数组元素
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void sortColors(vector<int>& nums) 
    {
        int left = 0, right = nums.size() - 1, i = 0;
        while (i <= right) {
            if (nums[i] == 0) swap(nums[left++], nums[i++]);
            else if (nums[i] == 1) ++i;
            else if (i <= right && nums[i] == 2) swap(nums[right--], nums[i]);
        }
    }
};
```

__Java__:
```Java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0, right = nums.length - 1, i = 0;
        while (i <= right) {
            if (nums[i] == 0) swap(left++, i++, nums);
            else if (nums[i] == 1) ++i;
            else if (i <= right && nums[i] == 2) swap(right--, i, nums);
        }
    }
    
    private void swap(int i, int j, int[] nums) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

__Python__:
```Python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums.sort()
```