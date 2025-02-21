# 2444 Count Subarrays With Fixed Bounds 统计定界子数组的数目

__Description:__

You are given an integer array `nums` and two integers `minK` and `maxK`.

A __fixed-bound subarray__ of `nums` is a subarray that satisfies the following conditions:

- The __minimum__ value in the subarray is equal to `minK`.
- The __maximum__ value in the subarray is equal to `maxK`.

Return the number of fixed-bound subarrays.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

```text
Input: nums = [1,3,5,2,7,5], minK = 1, maxK = 5
Output: 2
Explanation: The fixed-bound subarrays are [1,3,5] and [1,3,5,2].
```

Example 2:

```text
Input: nums = [1,1,1,1], minK = 1, maxK = 1
Output: 10
Explanation: Every subarray of nums is a fixed-bound subarray. There are 10 possible subarrays.
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], minK, maxK <= 10 ^ 6`

__题目描述:__

给你一个整数数组 `nums` 和两个整数 `minK` 以及 `maxK` 。

`nums` 的定界子数组是满足下述条件的一个子数组:

- 子数组中的 __最小值__ 等于 `minK` 。
- 子数组中的 __最大值__ 等于 `maxK` 。

返回定界子数组的数目。

子数组是数组中的一个连续部分。

__示例:__

示例 1：

```text
输入：nums = [1,3,5,2,7,5], minK = 1, maxK = 5
输出：2
解释：定界子数组是 [1,3,5] 和 [1,3,5,2] 。
```

示例 2：

```text
输入：nums = [1,1,1,1], minK = 1, maxK = 1
输出：10
解释：nums 的每个子数组都是一个定界子数组。共有 10 个子数组。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], minK, maxK <= 10 ^ 6`

__思路:__

```text
滑动窗口
设 left 和 right 是包括 [minK, maxK] 的窗口的左右边界
如果 [left, right] 中都是 [minK, maxK] 的数, 那么不需要移动 left 只需要移动 right
需要在 [left, right] 中找到 minK 和 maxK 的下标, 设为 last1 和 last2
如果 last1 和 last2 都不等于 -1
那么以 left 为左边界, last1 和 last2 中的最小值为右边界的子数组都是符合条件的
如果有一个数超过了 [minK, maxK] 的范围, 那么 left 和 right 都要移动, last1, last2 都要重置
从另一个角度来看, 可以将 left 和 right 看做 i 为右端点
找到一个不在 [minK, maxK] 的下标 i0
那么以 i 为右端点的合法子数组的个数为 min(last1, last2) - i0
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countSubarrays(vector<int>& nums, int minK, int maxK) 
    {
        long long result = 0LL;
        for (int i = 0, n = nums.size(), last1 = -1, last2 = -1, i0 = -1; i < n; i++) result += max(min(last1 = nums[i] == minK ? i : last1, last2 = nums[i] == maxK ? i : last2) - (i0 = nums[i] < minK or nums[i] > maxK ? i : i0), 0);
        return result; 
    }
};
```

__Java__:

```Java
class Solution {
    public long countSubarrays(int[] nums, int minK, int maxK) {
        long result = 0L;
        for (int n = nums.length, left = 0, right = 0, last1 = -1, last2 = -1; right < n; right++) {
            if (nums[right] > maxK || nums[right] < minK) {
                left = right + 1;
                last1 = last2 = -1;
            } else {
                if (nums[right] == minK) last1 = right;
                if (nums[right] == maxK) last2 = right;
                if (last1 != -1 && last2 != -1) result += Math.min(last1, last2) - left + 1L;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSubarrays(self, nums: List[int], minK: int, maxK: int) -> int:
        result, last1, last2, i0 = 0, -1, -1, -1
        for i, x in enumerate(nums):
            result += max(min(last1 := i if x == minK else last1, last2 := i if x == maxK else last2) - (i0 := i if not minK <= x <= maxK else i0), 0)
        return result
```
