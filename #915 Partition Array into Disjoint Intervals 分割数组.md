# 915 Partition Array into Disjoint Intervals 分割数组

__Description__:
Given an integer array nums, partition it into two (contiguous) subarrays left and right so that:

Every element in left is less than or equal to every element in right.
left and right are non-empty.
left has the smallest possible size.
Return the length of left after such a partitioning.

Test cases are generated such that partitioning exists.

__Example:__

Example 1:

Input: nums = [5,0,3,8,6]
Output: 3
Explanation: left = [5,0,3], right = [8,6]

Example 2:

Input: nums = [1,1,1,0,6,12]
Output: 4
Explanation: left = [1,1,1,0], right = [6,12]

__Constraints:__

2 <= nums.length <= 10^5
0 <= nums[i] <= 10^6
There is at least one valid answer for the given input.

__题目描述__:
给定一个数组 A，将其划分为两个连续子数组 left 和 right， 使得：

left 中的每个元素都小于或等于 right 中的每个元素。
left 和 right 都是非空的。
left 的长度要尽可能小。
在完成这样的分组后返回 left 的长度。可以保证存在这样的划分方法。

__示例 :__

示例 1：

输入：[5,0,3,8,6]
输出：3
解释：left = [5,0,3]，right = [8,6]

示例 2：

输入：[1,1,1,0,6,12]
输出：4
解释：left = [1,1,1,0]，right = [6,12]

__提示:__

2 <= A.length <= 30000
0 <= A[i] <= 10^6
可以保证至少有一种方法能够按题目所描述的那样对 A 进行划分。

__思路__:

模拟
维护左边列表的最大值 left_value 和全局最大值 max_value
遍历数组同时更新 max_value
如果当前元素小于 left_value, left_value 更新为 max_value, 即左边列表最大值等于当前最大值
result 更新为 i + 1, 即当前左边列表的长度
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int partitionDisjoint(vector<int>& nums) 
    {
        int left_value = nums[0], max_value = nums[0], result = 1, n = nums.size();
        for (int i = 0; i < n; i++) 
        {
            max_value = max(max_value, nums[i]);
            if (nums[i] < left_value) 
            {
                left_value = max_value;
                result = i + 1;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int partitionDisjoint(int[] nums) {
        int leftValue = nums[0], maxValue = nums[0], result = 1, n = nums.length;
        for (int i = 0; i < n; i++) {
            maxValue = Math.max(maxValue, nums[i]);
            if (nums[i] < leftValue) {
                leftValue = maxValue;
                result = i + 1;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def partitionDisjoint(self, nums: List[int]) -> int:
        left_value, max_value, result, n = nums[0], nums[0], 1, len(nums)
        for i in range(n):
            max_value = max(max_value, nums[i])
            if nums[i] < left_value:
                left_value, result = max_value, i + 1
        return result
```
