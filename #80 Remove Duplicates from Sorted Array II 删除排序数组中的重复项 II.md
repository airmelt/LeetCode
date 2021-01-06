# 80 Remove Duplicates from Sorted Array II 删除排序数组中的重复项 II

__Description__:
Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

__Example:__

Example 1:

Given nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.

Example 2:

Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.

__Clarification:__

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```Java
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

__题目描述__:
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

__示例 :__

示例 1:

给定 nums = [1,1,1,2,2,3],

函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。

你不需要考虑数组中超出新长度后面的元素。

示例 2:

给定 nums = [0,0,1,1,1,1,2,3,3],

函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。

你不需要考虑数组中超出新长度后面的元素。

__说明：__

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```Java
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

__思路__:

设置一个指针指向下标, 用 count记录重复元素的数目, count重置为 1
注意当数组为空的时候可能会越界
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int removeDuplicates(vector<int>& nums) 
    {
        if (nums.size() < 3) return nums.size();
        int result = 1, count = 1;
        for (int i = 1; i < nums.size(); i++)
        {
            if (nums[i - 1] == nums[i]) ++count;
            else count = 1;
            if (count <= 2) nums[result++] = nums[i];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int removeDuplicates(int[] nums) {
        int result = 0;
        for (int num : nums) if (result < 2 || num > nums[result - 2]) nums[result++] = num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        result = 0
        for num in nums:
            if result < 2 or num > nums[result - 2]:
                nums[result] = num
                result += 1
        return result
```
