# 169 Majority Element 求众数

__Description__:
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

__Example__:

Example 1:
Input: [3,2,3]
Output: 3

Example 2:
Input: [2,2,1,1,1,2,2]
Output: 2

__题目描述__:
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

__示例__:

示例 1:
输入: [3,2,3]
输出: 3

示例 2:
输入: [2,2,1,1,1,2,2]
输出: 2

__思路__:

遍历数组, 维护一个 count, 初始为0, 当 count为 0时, 将结果更新为当前元素, 结果与数组元素相等时, count++, 否则 count--, 由于众数一定是大于 ⌊ n/2 ⌋ 的元素, 最后得到的结果元素的 count一定大于 0.
不能使用排序, 因为排序时间复杂度为O(nlgn)
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int majorityElement(vector<int>& nums) 
    {
        int count = 0, result = 0;
        for (int num : nums) 
        {
            if (!count) result = num;
            if (result == num) count++;
            else count--;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int result = 0;
        for (int num : nums) {
            if (count == 0) result = num;
            if (result == num) count++;
            else count--;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        return sorted(nums)[len(nums) // 2]
```
