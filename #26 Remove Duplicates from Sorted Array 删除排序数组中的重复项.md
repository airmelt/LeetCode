__Description__:
Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

__Example__:
**Example 1:**

Given *nums* = **[1,1,2]**,

Your function should return length = **`2`**, with the first two elements of *`nums`* being **`1`** and **`2`** respectively.

It doesn't matter what you leave beyond the returned length.

**Example 2:**

Given *nums* = **[0,0,1,1,1,2,2,3,3,4]**,

Your function should return length = **`5`**, with the first five elements of *`nums`* being modified to **`0`**, **`1`**, **`2`**, **`3`**, and **`4`** respectively.

It doesn't matter what values are set beyond the returned length.


**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

// **nums** is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);
// any modification to **nums** in your function would be known by the caller.
// using the length returned by your function, it prints the first **len** elements.
```
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

__题目描述__:
给定一个排序数组，你需要在**[原地](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)修改输入数组**并在使用 O(1) 额外空间的条件下完成。
__示例__:
**示例 1:**

给定数组 *nums* = **[1,1,2]**,

函数应该返回新的长度 **2**, 并且原数组 *nums* 的前两个元素被修改为 **`1`**, **`2`**。

你不需要考虑数组中超出新长度后面的元素。

**示例 2:**

给定 *nums* = **[0,0,1,1,1,2,2,3,3,4]**,

函数应该返回新的长度 **5**, 并且原数组 *nums* 的前五个元素被修改为 **`0`**, **`1`**, **`2`**, **`3`**, **`4`**。

你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**“引用”**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// **nums** 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);
// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中**该长度范围内**的所有元素。
```
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

__思路__:
当数组为空的时候返回0; 维护一个长度, 遍历数组找到不同值替换.
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (!nums.size()) return 0;
        int result = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != nums[result]) nums[++result] = nums[i];
        }
        return ++result;
    }
};
```

__Java__:
```
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[result] != nums[i]) nums[++result] = nums[i];
        }
        return ++result;
    }
}
```

__Python__:
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        result = 0
        for i in range(len(nums)):
            if nums[i] != nums[result]:
                result += 1
                nums[result] = nums[i]
        return result + 1
```
