# 2302 Count Subarrays With Score Less Than K 统计得分小于 K 的子数组数目

__Description:__

The score of an array is defined as the product of its sum and its length.

- For example, the score of `[1, 2, 3, 4, 5]` is `(1 + 2 + 3 + 4 + 5) * 5 = 75`.

Given a positive integer array `nums` and an integer `k`, return _the __number of non-empty subarrays__ of_ `nums` _whose score is __strictly less__ than_ `k`.

A subarray is a contiguous sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [2,1,4,3,5], k = 10
Output: 6
Explanation:
The 6 subarrays having scores less than 10 are:
```

- [2] with score 2 * 1 = 2.
- [1] with score 1 * 1 = 1.
- [4] with score 4 * 1 = 4.
- [3] with score 3 * 1 = 3.
- [5] with score 5 * 1 = 5.
- [2,1] with score (2 + 1) * 2 = 6.

Note that subarrays such as [1,4] and [4,3,5] are not considered because their scores are 10 and 36 respectively, while we need scores strictly less than 10.

Example 2:

```text
Input: nums = [1,1,1], k = 5
Output: 5
Explanation:
Every subarray except [1,1,1] has a score less than 5.
[1,1,1] has a score (1 + 1 + 1) * 3 = 9, which is greater than 5.
Thus, there are 5 subarrays having scores less than 5.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 15`

__题目描述:__

一个数组的 分数 定义为数组之和 乘以 数组的长度。

- 比方说， `[1, 2, 3, 4, 5]` 的分数为 `(1 + 2 + 3 + 4 + 5) * 5 = 75` 。

给你一个正整数数组 `nums` 和一个整数 `k` ，请你返回 `nums` 中分数 __严格小于__ `k` 的 __非空整数子数组数目__。

子数组 是数组中的一个连续元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,1,4,3,5], k = 10
输出：6
解释：
有 6 个子数组的分数小于 10 ：
```

- [2] 分数为 2 * 1 = 2 。
- [1] 分数为 1 * 1 = 1 。
- [4] 分数为 4 * 1 = 4 。
- [3] 分数为 3 * 1 = 3 。
- [5] 分数为 5 * 1 = 5 。
- [2,1] 分数为 (2 + 1) * 2 = 6 。

注意，子数组 [1,4] 和 [4,3,5] 不符合要求，因为它们的分数分别为 10 和 36，但我们要求子数组的分数严格小于 10 。

示例 2：

```text
输入：nums = [1,1,1], k = 5
输出：5
解释：
除了 [1,1,1] 以外每个子数组分数都小于 5 。
[1,1,1] 分数为 (1 + 1 + 1) * 3 = 9 ，大于 5 。
所以总共有 5 个子数组得分小于 5 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 15`

__思路:__

```text
滑动窗口
用一个滑动窗口维护子数组的和 s
当 s * (right - left + 1) >= k 时, 窗口左边界向右移动
此时, 以 right 为右边界的子数组的和都小于 k
result += right - left + 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countSubarrays(vector<int>& nums, long long k) 
    {
        long long result = 0LL, s = 0LL;
        for (int right = 0, n = nums.size(), left = 0; right < n; right++) 
        {
            s += nums[right];
            while (s * (right - left + 1LL) >= k) s -= nums[left++];
            result += right - left + 1LL;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        long result = 0L, s = 0L;
        for (int right = 0, n = nums.length, left = 0; right < n; right++) {
            s += nums[right];
            while (s * (right - left + 1L) >= k) s -= nums[left++];
            result += right - left + 1L;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        result = s = left = 0
        for right, num in enumerate(nums):
            s += num
            while s * (right - left + 1) >= k:
                s -= nums[left]
                left += 1
            result += right - left + 1
        return result
```
