# 2104 Sum of Subarray Ranges 子数组范围和

__Description:__

You are given an integer array `nums`. The __range__ of a subarray of `nums` is the difference between the largest and smallest element in the subarray.

Return _the __sum of all__ subarray ranges of_ `nums`_._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3]
Output: 4
Explanation: The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0 
[2], range = 2 - 2 = 0
[3], range = 3 - 3 = 0
[1,2], range = 2 - 1 = 1
[2,3], range = 3 - 2 = 1
[1,2,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.
```

Example 2:

```text
Input: nums = [1,3,3]
Output: 4
Explanation: The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0
[3], range = 3 - 3 = 0
[3], range = 3 - 3 = 0
[1,3], range = 3 - 1 = 2
[3,3], range = 3 - 3 = 0
[1,3,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.
```

Example 3:

```text
Input: nums = [4,-2,-3,4,1]
Output: 59
Explanation: The sum of all subarray ranges of nums is 59.
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__Follow-up:__ Could you find a solution with `O(n)` time complexity?

__题目描述:__

给你一个整数数组 `nums` 。 `nums` 中，子数组的 __范围__ 是子数组中最大元素和最小元素的差值。

返回 `nums` 中 __所有__ 子数组范围的 __和__ _。_

子数组是数组中一个连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0 
[2]，范围 = 2 - 2 = 0
[3]，范围 = 3 - 3 = 0
[1,2]，范围 = 2 - 1 = 1
[2,3]，范围 = 3 - 2 = 1
[1,2,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 1 + 1 + 2 = 4
```

示例 2：

```text
输入：nums = [1,3,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0
[3]，范围 = 3 - 3 = 0
[3]，范围 = 3 - 3 = 0
[1,3]，范围 = 3 - 1 = 2
[3,3]，范围 = 3 - 3 = 0
[1,3,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 2 + 0 + 2 = 4
```

示例 3：

```text
输入：nums = [4,-2,-3,4,1]
输出：59
解释：nums 中所有子数组范围的和是 59
```

__提示：__

- `1 <= nums.length <= 1000`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`

__进阶:__你可以设计一种时间复杂度为 `O(n)` 的解决方案吗？

__思路:__

```text
1. 暴力法
直接计算所有的子数组
记录所有子数组的最大值和小值
累加即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 单调栈
以 i 为最小值的子数组个数为 (i - left) * (right - i)
其中 left 为 i 左边第一个小于 i 的位置
right 为 i 右边第一个小于 i 的位置
则以 i 为最小值的子数组的贡献为 (i - left1) * (right1 - i) * nums[i]
同理以 i 为最大值的子数组的贡献为 (i - left2) * (right2 - i) * nums[i]
最终结果为以 i 为最大值的子数组的贡献和减去以 i 为最小值的子数组的贡献和
可以用两个单调栈来维护 left 和 right
记录 left 和 right 的哨兵为 -1 和 n
遍历列表, 如果当前元素大于栈顶元素, 则弹出栈顶元素, 直到栈为空或者栈顶元素小于当前元素
就可以找到 left1
其他同理
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long subArrayRanges(vector<int>& nums) 
    {
        int n = nums.size();
        vector<int> minLeft(n), minRight(n), maxLeft(n), maxRight(n);
        stack<int> minStack, maxStack;
        for (int i = 0; i < n; i++) 
        {
            while (!minStack.empty() and nums[minStack.top()] > nums[i]) minStack.pop();
            minLeft[i] = minStack.empty() ? -1 : minStack.top();
            minStack.push(i);
            while (!maxStack.empty() and nums[maxStack.top()] <= nums[i]) maxStack.pop();
            maxLeft[i] = maxStack.empty() ? -1 : maxStack.top();
            maxStack.push(i);
        }
        minStack = stack<int>();
        maxStack = stack<int>();
        for (int i = n - 1; i > -1; i--) 
        {
            while (!minStack.empty() and nums[minStack.top()] >= nums[i]) minStack.pop();
            minRight[i] = minStack.empty() ? n : minStack.top();
            minStack.push(i);
            while (!maxStack.empty() and nums[maxStack.top()] < nums[i]) maxStack.pop();
            maxRight[i] = maxStack.empty() ? n : maxStack.top();
            maxStack.push(i);
        }
        long long sumMax = 0, sumMin = 0;
        for (int i = 0; i < n; i++) 
        {
            sumMax += static_cast<long long>(maxRight[i] - i) * (i - maxLeft[i]) * nums[i];
            sumMin += static_cast<long long>(minRight[i] - i) * (i - minLeft[i]) * nums[i];
        }
        return sumMax - sumMin;
    }
};
```

__Java__:

```Java
class Solution {
    public long subArrayRanges(int[] nums) {
        long result = 0;
        for (int i = 0, n = nums.length; i < n; i++) for (int j = i + 1, minVal = nums[i], maxVal = nums[i]; j < n; j++) result += (maxVal = Math.max(maxVal, nums[j])) - (minVal = Math.min(minVal, nums[j]));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        result, n = 0, len(nums)
        for i in range(n):
            max_value = min_value = nums[i]
            for j in range(i + 1, n):
                result += (max_value := max(max_value, nums[j])) - (min_value := min(min_value, nums[j]))
        return result
```
