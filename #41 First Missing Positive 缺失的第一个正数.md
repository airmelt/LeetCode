# 41 First Missing Positive 缺失的第一个正数

__Description__:
Given an unsorted integer array, find the smallest missing positive integer.

__Example:__

Example 1:

Input: [1,2,0]
Output: 3

Example 2:

Input: [3,4,-1,1]
Output: 2

Example 3:

Input: [7,8,9,11,12]
Output: 1

__Note:__

Your algorithm should run in O(n) time and uses constant extra space.

__题目描述__:
给定一个未排序的整数数组，找出其中没有出现的最小的正整数。

__示例 :__

示例 1:

输入: [1,2,0]
输出: 3

示例 2:

输入: [3,4,-1,1]
输出: 2

示例 3:

输入: [7,8,9,11,12]
输出: 1

__说明:__

你的算法的时间复杂度应为O(n)，并且只能使用常数级别的空间。

__思路__:

此题为408原题
思路是用数组下标当作桶, 进行桶排序
第一次遍历先将 1~nums.size()范围内的元素放在正确的位置
比如 1, 2, 3... 可以用判断 nums[i] == nums[nums[i] - 1]实现
第二次遍历查找是否有不在位置上的元素, 如果有输出 i + 1否则输出数组长度 + 1
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int firstMissingPositive(vector<int>& nums) 
    {
        for (int i = 0; i < nums.size(); i++) while (nums[i] > 0 and nums[i] < nums.size() + 1 and nums[i] != nums[nums[i] - 1]) swap(nums[i], nums[nums[i] - 1]);
        for (int i = 0; i < nums.size(); i++) if (nums[i] != i + 1) return i + 1;
        return nums.size() + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int firstMissingPositive(int[] nums) {
        for (int i = 0; i < nums.length; i++) while (nums[i] > 0 && nums[i] < 1 + nums.length && nums[i] != nums[nums[i] - 1]) swap(nums, i, nums[i] - 1);
        for (int i = 0; i < nums.length; i++) if (nums[i] != i + 1) return i + 1;
        return nums.length + 1;
    }
    
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

__Python__:

```Python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        def swap(nums: List[int], i: int, j: int) -> None:
            nums[i], nums[j] = nums[j], nums[i]
        for i in range(len(nums)):
            while 0 < nums[i] <= len(nums) and nums[i] != nums[nums[i] - 1]:
                swap(nums, i, nums[i] - 1)
        for i in range(len(nums)):
            if nums[i] != i + 1:
                return i + 1
        return len(nums) + 1
```
