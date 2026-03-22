# 724 Find Pivot Index 寻找数组的中心索引

__Description__:
Given an array of integers nums, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

__Example:__

Example 1:

Input:
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation:
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.

Example 2:

Input:
nums = [1, 2, 3]
Output: -1
Explanation:
There is no index that satisfies the conditions in the problem statement.

__Note:__

The length of nums will be in the range [0, 10000].
Each element nums[i] will be an integer in the range [-1000, 1000].

__题目描述__:
给定一个整数类型的数组 nums，请编写一个能够返回数组“中心索引”的方法。

我们是这样定义数组中心索引的：数组中心索引的左侧所有元素相加的和等于右侧所有元素相加的和。

如果数组不存在中心索引，那么我们应该返回 -1。如果数组有多个中心索引，那么我们应该返回最靠近左边的那一个。

__示例 :__

示例 1:

输入:
nums = [1, 7, 3, 6, 5, 6]
输出: 3
解释:
索引3 (nums[3] = 6) 的左侧数之和(1 + 7 + 3 = 11)，与右侧数之和(5 + 6 = 11)相等。
同时, 3 也是第一个符合要求的中心索引。

示例 2:

输入:
nums = [1, 2, 3]
输出: -1
解释:
数组中不存在满足此条件的中心索引。

__说明:__

nums 的长度范围为 [0, 10000]。
任何一个 nums[i] 将会是一个范围在 [-1000, 1000]的整数。

__思路__:

先求出数组之和, 然后遍历数组, 当左边的数之和等于右边的输出下标即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int pivotIndex(vector<int>& nums) 
    {
        int s = 0, cur = 0;
        for (int num : nums) s += num;
        for (int i = 0; i < nums.size(); i++) 
        {
            if (s - nums[i] == cur) return i;
            s -= nums[i];
            cur += nums[i];
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int pivotIndex(int[] nums) {
        int s = 0, cur = 0;
        for (int num : nums) s += num;
        for (int i = 0; i < nums.length; i++) {
            if (s - nums[i] == cur) return i;
            s -= nums[i];
            cur += nums[i];
        }
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        s, cur = sum(nums), 0
        for i in range(len(nums)):
            if s - nums[i] == cur:
                return i
            cur += nums[i]
            s -= nums[i]
        return -1
```
