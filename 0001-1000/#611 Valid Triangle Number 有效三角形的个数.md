# 611 Valid Triangle Number 有效三角形的个数

__Description__:
Given an integer array nums, return the number of triplets chosen from the array that can make triangles if we take them as side lengths of a triangle.

__Example:__

Example 1:

Input: nums = [2,2,3,4]
Output: 3
Explanation: Valid combinations are:
2,3,4 (using the first 2)
2,3,4 (using the second 2)
2,2,3

Example 2:

Input: nums = [4,2,3,4]
Output: 4

__Constraints:__

1 <= nums.length <= 1000
0 <= nums[i] <= 1000

__题目描述__:
给定一个包含非负整数的数组，你的任务是统计其中可以组成三角形三条边的三元组个数。

__示例 :__

示例 1:

输入: [2,2,3,4]
输出: 3
解释:
有效的组合是:
2,3,4 (使用第一个 2)
2,3,4 (使用第二个 2)
2,2,3

__注意:__

数组长度不超过1000。
数组里整数的范围为 [0, 1000]。

__思路__:

排序之后用双指针查找
时间复杂度 O(n ^ 2), 空间复杂度 O(lgn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int triangleNumber(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        int result = 0, n = nums.size();
        for (int i = n - 1; i > 1; i--) 
        {
            int left = 0, right = i - 1;
            while (left < right)
            {
                if (nums[left] + nums[right] > nums[i]) result += -left + right--;
                else ++left;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int triangleNumber(int[] nums) {
        Arrays.sort(nums);
        int result = 0, n = nums.length;
        for (int i = n - 1; i > 1; i--) {
            int left = 0, right = i - 1;
            while (left < right) {
                if (nums[left] + nums[right] > nums[i]) result += -left + right--;
                else ++left;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:
        nums.sort()
        result, n = 0, len(nums)
        for i in range(n - 1, 1, -1):
            left, right = 0, i - 1
            while left < right:
                if nums[left] + nums[right] > nums[i]:
                    result += right - left
                    right -= 1
                else:
                    left += 1
        return result
```
