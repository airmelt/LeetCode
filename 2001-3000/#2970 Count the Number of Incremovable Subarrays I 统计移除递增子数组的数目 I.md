# 2970 Count the Number of Incremovable Subarrays I 统计移除递增子数组的数目 I

__Description:__

You are given a __0-indexed__ array of __positive__ integers `nums`.

A subarray of `nums` is called __incremovable__ if `nums` becomes __strictly increasing__ on removing the subarray. For example, the subarray `[3, 4]` is an incremovable subarray of `[5, 3, 4, 6, 7]` because removing this subarray changes the array `[5, 3, 4, 6, 7]` to `[5, 6, 7]` which is strictly increasing.

Return _the total number of __incremovable__ subarrays of_ `nums`.

Note that an empty array is considered strictly increasing.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: 10
Explanation: The 10 incremovable subarrays are: [1], [2], [3], [4], [1,2], [2,3], [3,4], [1,2,3], [2,3,4], and [1,2,3,4], because on removing any one of these subarrays nums becomes strictly increasing. Note that you cannot select an empty subarray.
```

Example 2:

```text
Input: nums = [6,5,7,8]
Output: 7
Explanation: The 7 incremovable subarrays are: [5], [6], [5,7], [6,5], [5,7,8], [6,5,7] and [6,5,7,8].
It can be shown that there are only 7 incremovable subarrays in nums.
```

Example 3:

```text
Input: nums = [8,7,6,6]
Output: 3
Explanation: The 3 incremovable subarrays are: [8,7,6], [7,6,6], and [8,7,6,6]. Note that [8,7] is not an incremovable subarray because after removing [8,7] nums becomes [6,6], which is sorted in ascending order but not strictly increasing.
```

__Constraints:__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__题目描述:__

给你一个下标从 __0__ 开始的 _正_ 整数数组 `nums` 。

如果 `nums` 的一个子数组满足:移除这个子数组后剩余元素 __严格递增__ ，那么我们称这个子数组为 __移除递增__ 子数组。比方说， `[5, 3, 4, 6, 7]` 中的 `[3, 4]` 是一个移除递增子数组，因为移除该子数组后， `[5, 3, 4, 6, 7]` 变为 `[5, 6, 7]` ，是严格递增的。

请你返回 `nums` 中 _移除递增_ 子数组的总数目。

注意 ，剩余元素为空的数组也视为是递增的。

子数组 指的是一个数组中一段非空且连续的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：10
解释：10 个移除递增子数组分别为：[1], [2], [3], [4], [1,2], [2,3], [3,4], [1,2,3], [2,3,4] 和 [1,2,3,4]。移除任意一个子数组后，剩余元素都是递增的。注意，空数组不是移除递增子数组。
```

示例 2：

```text
输入：nums = [6,5,7,8]
输出：7
解释：7 个移除递增子数组分别为：[5], [6], [5,7], [6,5], [5,7,8], [6,5,7] 和 [6,5,7,8] 。
nums 中只有这 7 个移除递增子数组。
```

示例 3：

```text
输入：nums = [8,7,6,6]
输出：3
解释：3 个移除递增子数组分别为：[8,7,6], [7,6,6] 和 [8,7,6,6] 。注意 [8,7] 不是移除递增子数组因为移除 [8,7] 后 nums 变为 [6,6] ，它不是严格递增的。
```

__提示：__

- `1 <= nums.length <= 50`
- `1 <= nums[i] <= 50`

__思路:__

```text
暴力法
检查每一个 [left, right] 以外的子数组
是否为递增的数组
时间复杂度为 O(N  ^ 3), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int incremovableSubarrayCount(vector<int>& nums) 
    {
        int n = nums.size(), result = 0;
        auto check = [&](int left, int right) -> bool
        {
            for (int i = 1; i < n; i++) if ((i < left or i > right + 1)) if (nums[i] <= nums[i - 1]) return false;
            return left < 1 or right > n - 2 or nums[right + 1] > nums[left - 1];
        };
        for (int i = 0; i < n; i++) for (int j = i; j < n; j++) result += check(i, j);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int incremovableSubarrayCount(int[] nums) {
        int n = nums.length, result = 0;
        for (int i = 0; i < n; i++) for (int j = i; j < n; j++) if (check(nums, i, j)) ++result;
        return result;
    }

    private boolean check(int[] nums, int left, int right) {
        for (int i = 1, n = nums.length; i < n; i++) if ((i < left || i > right + 1)) if (nums[i] <= nums[i - 1]) return false;
        return left < 1 || right > nums.length - 2 || nums[right + 1] > nums[left - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def incremovableSubarrayCount(self, nums: List[int]) -> int:
        return sum(not k or all(k[p] > k[p - 1] for p in range(1, len(k))) for k in [nums[:i] + nums[j:] for i in range(len(nums)) for j in range(i + 1, len(nums) + 1)])
```
