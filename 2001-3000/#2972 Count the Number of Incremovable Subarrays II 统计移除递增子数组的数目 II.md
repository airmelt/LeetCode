# 2972 Count the Number of Incremovable Subarrays II 统计移除递增子数组的数目 II

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

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的 _正_ 整数数组 `nums` 。

如果 `nums` 的一个子数组满足:移除这个子数组后剩余元素 __严格递增__ ，那么我们称这个子数组为 __移除递增__ 子数组。比方说， `[5, 3, 4, 6, 7]` 中的 `[3, 4]` 是一个移除递增子数组，因为移除该子数组后， `[5, 3, 4, 6, 7]` 变为 `[5, 6, 7]` ，是严格递增的。

请你返回 `nums` 中 _移除递增_ 子数组的总数目。

注意 ，剩余元素为空的数组也视为是递增的。

子数组 指的是一个数组中一段连续的元素序列。

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

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
双指针
先找到移除后缀仍然能够递增的最后一个元素
比如 [6, 5, 7, 8]
i = 0 时取得递增子数组 [6]
那么可以移除 [5, 7, 8], [6, 5, 7, 8]
即 i + 2 个数组
这种情况如果 i == n - 1, 说明数组本身已经是递增数组
可以移除任意非空子数组, 共 n * (n + 1) / 2 个
另一种情况是移除中间的子数组
此时前缀和后缀需要组成递增数组
那么前缀的最后一个元素 i 需要小于后缀的第一个元素 j
且后缀需要为递增数组
注意 i 已经是递增数组的最后一个元素, 所以前缀必然是递增的
所以需要保证 nums[i] > nums[j]
并且 nums[j] < nums[j + 1] 或者 j == n - 1
j == n - 1 时一定有最后一个元素单独可以构成递增数组
每次找到一个可以构成的子数组
由于前缀是递增数组类似情况 1 的情况结果都需要累加 i + 2
注意 i 可以等于 -1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long incremovableSubarrayCount(vector<int>& nums) 
    {
        long long result = 0LL;
        int n = nums.size(), i = 0, j = n - 1;
        for (; i < n - 1 and nums[i] < nums[i + 1]; i++) ;
        if (i == n - 1) return n * (n + 1LL) >> 1LL;
        for (result = i + 2; j == n - 1 or nums[j] < nums[j + 1]; result += i + 2L, j--) while (i > -1 and nums[i] >= nums[j]) --i;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long incremovableSubarrayCount(int[] nums) {
        long result = 0L;
        int n = nums.length, i = 0, j = n - 1;
        for (; i < n - 1 && nums[i] < nums[i + 1]; i++) ;
        if (i == n - 1) return n * (n + 1L) >>> 1L;
        for (result = i + 2; j == n - 1 || nums[j] < nums[j + 1]; result += i + 2L, j--) while (i > -1 && nums[i] >= nums[j]) --i;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def incremovableSubarrayCount(self, nums: List[int]) -> int:
        n, i = len(nums), 0
        while i < n - 1 and nums[i] < nums[i + 1]:
            i += 1
        if i == n - 1:
            return n * (n + 1) >> 1
        result, j = i + 2, n - 1
        while j == n - 1 or nums[j] < nums[j + 1]:
            while i > -1 and nums[i] >= nums[j]:
                i -= 1
            result += i + 2
            j -= 1
        return result
```
