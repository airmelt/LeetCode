# 775 Global and Local Inversions 全局倒置与局部倒置

__Description__:
You are given an integer array nums of length n which represents a permutation of all the integers in the range [0, n - 1].

The number of global inversions is the number of the different pairs (i, j) where:

0 <= i < j < n
nums[i] > nums[j]
The number of local inversions is the number of indices i where:

0 <= i < n - 1
nums[i] > nums[i + 1]
Return true if the number of global inversions is equal to the number of local inversions.

__Example:__

Example 1:

Input: nums = [1,0,2]
Output: true
Explanation: There is 1 global inversion and 1 local inversion.

Example 2:

Input: nums = [1,2,0]
Output: false
Explanation: There are 2 global inversions and 1 local inversion.

__Constraints:__

n == nums.length
1 <= n <= 10^5
0 <= nums[i] < n
All the integers of nums are unique.
nums is a permutation of all the numbers in the range [0, n - 1].

__题目描述__:
给你一个长度为 n 的整数数组 nums ，表示由范围 [0, n - 1] 内所有整数组成的一个排列。

全局倒置 的数目等于满足下述条件不同下标对 (i, j) 的数目：

0 <= i < j < n
nums[i] > nums[j]
局部倒置 的数目等于满足下述条件的下标 i 的数目：

0 <= i < n - 1
nums[i] > nums[i + 1]
当数组 nums 中 全局倒置 的数量等于 局部倒置 的数量时，返回 true ；否则，返回 false 。

__示例 :__

示例 1：

输入：nums = [1,0,2]
输出：true
解释：有 1 个全局倒置，和 1 个局部倒置。

示例 2：

输入：nums = [1,2,0]
输出：false
解释：有 2 个全局倒置，和 1 个局部倒置。

__提示:__

n == nums.length
1 <= n <= 5000
0 <= nums[i] < n
nums 中的所有整数 互不相同
nums 是范围 [0, n - 1] 内所有数字组成的一个排列

__思路__:

全局倒置指的是, nums 的逆序数, 比如 [1, 0, 2], 中只有 1, 0 是逆序的, i, j 不需要连续
局部倒置指的是, nums 中子序列的逆序数, 比如 [1, 2, 0], 中只有 2, 0 是子序列且逆序, i, j 需要连续
那么全局倒置一定包括局部倒置
只需要检查是否所有局部倒置都是全局倒置即可
由于局部倒置必须是连续的, 所以如果一个局部倒置也是全局倒置, 那么它本身和索引的差应该小于或等于 1, 否则一定子序列中会出现类似 [1, 2, 0]/[2, 0, 1]/[2, 1, 0] 的这种排列导致全局倒置和局部倒置不相等
遍历 nums 只要有 abs(nums[i] - i) > 1 就返回 False, 否则返回 True
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isIdealPermutation(vector<int>& nums) 
    {
        for (int i = 0, n = nums.size(); i < n; i++) if (abs(nums[i] - i) > 1) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isIdealPermutation(int[] nums) {
        for (int i = 0, n = nums.length; i < n; i++) if (Math.abs(nums[i] - i) > 1) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isIdealPermutation(self, nums: List[int]) -> bool:
        return all(abs(nums[i] - i) < 2 for i in range(len(nums)))
```
