__Description__:
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

__Example:__

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

__题目描述__:
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

__示例 :__
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).

__思路__:
参考[LeetCode #15 3Sum 三数之和](https://www.jianshu.com/p/a6253046921b),排序之后再用双指针查找
时间复杂度O(n^ 2), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int threeSumClosest(vector<int>& nums, int target) 
    {
        sort(nums.begin(), nums.end());
        int result = nums[0] + nums[1] + nums[2], n = nums.size();
        for (int i = 0; i < n - 2; i++) {
            int l = i + 1, r = n - 1;
            while (l < r) 
            {
                int temp = nums[l] + nums[r] + nums[i];
                if (abs(temp - target) < abs(result - target)) result = temp;
                if (temp > target) --r;
                else if (temp < target) ++l;
                else return target;
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int result = nums[0] + nums[1] + nums[2], n = nums.length;
        for (int i = 0; i < n - 2; i++) {
            int l = i + 1, r = n - 1;
            while (l < r) {
                int temp = nums[l] + nums[r] + nums[i];
                if (Math.abs(temp - target) < Math.abs(result - target)) result = temp;
                if (temp > target) --r;
                else if (temp < target) ++l;
                else return target;
            }
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()
        result = nums[0] + nums[1] + nums[2]
        for i in range(len(nums) - 2):
            l, r = i + 1, len(nums) - 1
            while l < r:
                temp = nums[l] + nums[r] + nums[i]
                if abs(temp - target) < abs(result - target):
                    result = temp
                if temp > target:
                    r -= 1
                elif temp < target:
                    l += 1
                else:
                    return target
        return result
```