# 283 Move Zeroes 移动零

__Description__:
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example :**

Input: [0,1,0,3,12]
Output: [1,3,12,0,0]

__Note:__

1. You must do this in-place without making a copy of the array.
2. Minimize the total number of operations.

__题目描述__:
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例 :**

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]

__说明:__

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

__思路__:

设置两个指针, 数组前面的一部分交换为非 0值, 后面的直接设为 0
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    void moveZeroes(vector<int>& nums) 
    {
        int i = 0, j = 0, n = nums.size();
        for (; i < n; i++) if (nums[i]) nums[j++] = nums[i];
        for (; j < n; j++) nums[j] = 0;
    }
};
```

__Java__:

```Java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0, j = 0, n = nums.length;
        for (; i < n; i++) if (nums[i] != 0) nums[j++] = nums[i];
        for (; j < n; j++) nums[j] = 0;
    }
}
```

__Python__:

```Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums.sort(key = bool, reverse = True)
```
