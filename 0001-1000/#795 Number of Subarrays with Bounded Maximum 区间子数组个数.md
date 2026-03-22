# 795 Number of Subarrays with Bounded Maximum 区间子数组个数

__Description__:
Given an integer array nums and two integers left and right, return the number of contiguous non-empty subarrays such that the value of the maximum array element in that subarray is in the range [left, right].

The test cases are generated so that the answer will fit in a 32-bit integer.

__Example:__

Example 1:

Input: nums = [2,1,4,3], left = 2, right = 3
Output: 3
Explanation: There are three subarrays that meet the requirements: [2], [2, 1], [3].

Example 2:

Input: nums = [2,9,2,5,6], left = 2, right = 8
Output: 7

__Constraints:__

1 <= nums.length <= 10^5
0 <= nums[i] <= 10^9
0 <= left <= right <= 10^9

__题目描述__:
给定一个元素都是正整数的数组A ，正整数 L 以及 R (L <= R)。

求连续、非空且其中最大元素满足大于等于L 小于等于R的子数组个数。

__示例 :__

例如 :
输入:
A = [2, 1, 4, 3]
L = 2
R = 3
输出: 3
解释: 满足条件的子数组: [2], [2, 1], [3].

__注意:__

L, R  和 A[i] 都是整数，范围在 [0, 10^9]。
数组 A 的长度范围在[1, 50000]。

__思路__:

动态规划
dp[i] 表示包括下标 i 的子数组个数
初始化均为 0
nums[i] > right, dp[i] = 0, 记录 pre 为上一个大于 right 的下标
nums[i] >= left, dp[i] = i - pre
nums[i] < left, dp[i] = dp[i - 1], dp[i] 不能单独组成满足题意的数组, dp[i - 1] 加上 nums[i] 可以
可以看到 dp[i] 只取决于 i 和 pre, 空间可优化为 O(1)
记录 cur 为当前元素不小于 left 的时候子数组的个数
遍历数组, 若当前元素大于 right, 则 pre 移动到当前元素下标, 若当前元素不大于 left, 则当前元素到 pre 的数组都满足题意, 更新 cur
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSubarrayBoundedMax(vector<int>& nums, int left, int right) 
    {
        int pre = -1, cur = 0, result = 0;
        for (int i = 0, n = nums.size(); i < n; i++) 
        {
            if (nums[i] > right) pre = i;
            if (nums[i] >= left) cur = i - pre;
            result += cur;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numSubarrayBoundedMax(int[] nums, int left, int right) {
        int pre = -1, cur = 0, result = 0;
        for (int i = 0, n = nums.length; i < n; i++) {
            if (nums[i] > right) pre = i;
            if (nums[i] >= left) cur = i - pre;
            result += cur;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numSubarrayBoundedMax(self, nums: List[int], left: int, right: int) -> int:
        return (f := lambda n: sum(accumulate([i <= n for i in nums], lambda a, b: a * b + b)))(right) - f(left - 1)
```
