# 2760 Longest Even Odd Subarray With Threshold 最长奇偶子数组

__Description:__

You are given a __0-indexed__ integer array `nums` and an integer `threshold`.

Find the length of the __longest subarray__ of `nums` starting at index `l` and ending at index `r` `(0 <= l <= r < nums.length)` that satisfies the following conditions:

- `nums[l] % 2 == 0`
- For all indices `i` in the range `[l, r - 1]`, `nums[i] % 2 != nums[i + 1] % 2`
- For all indices `i` in the range `[l, r]`, `nums[i] <= threshold`

Return an integer denoting the length of the longest such subarray.

Note: A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [3,2,5,4], threshold = 5
Output: 3
Explanation: In this example, we can select the subarray that starts at l = 1 and ends at r = 3 => [2,5,4]. This subarray satisfies the conditions.
Hence, the answer is the length of the subarray, 3. We can show that 3 is the maximum possible achievable length.
```

Example 2:

```text
Input: nums = [1,2], threshold = 2
Output: 1
Explanation: In this example, we can select the subarray that starts at l = 1 and ends at r = 1 => [2]. 
It satisfies all the conditions and we can show that 1 is the maximum possible achievable length.
```

Example 3:

```text
Input: nums = [2,3,4,5], threshold = 4
Output: 3
Explanation: In this example, we can select the subarray that starts at l = 0 and ends at r = 2 => [2,3,4]. 
It satisfies all the conditions.
Hence, the answer is the length of the subarray, 3. We can show that 3 is the maximum possible achievable length.
```

__Constraints:__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `1 <= threshold <= 100`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 和一个整数 `threshold` 。

请你从 `nums` 的子数组中找出以下标 `l` 开头、下标 `r` 结尾 `(0 <= l <= r < nums.length)` 且满足以下条件的 __最长子数组__ :

- `nums[l] % 2 == 0`
- 对于范围 `[l, r - 1]` 内的所有下标 `i` ， `nums[i] % 2 != nums[i + 1] % 2`
- 对于范围 `[l, r]` 内的所有下标 `i` ， `nums[i] <= threshold`

以整数形式返回满足题目要求的最长子数组的长度。

注意：子数组 是数组中的一个连续非空元素序列。

__示例:__

示例 1：

```text
输入：nums = [3,2,5,4], threshold = 5
输出：3
解释：在这个示例中，我们选择从 l = 1 开始、到 r = 3 结束的子数组 => [2,5,4] ，满足上述条件。
因此，答案就是这个子数组的长度 3 。可以证明 3 是满足题目要求的最大长度。
```

示例 2：

```text
输入：nums = [1,2], threshold = 2
输出：1
解释：
在这个示例中，我们选择从 l = 1 开始、到 r = 1 结束的子数组 => [2] 。
该子数组满足上述全部条件。可以证明 1 是满足题目要求的最大长度。
```

示例 3：

```text
输入：nums = [2,3,4,5], threshold = 4
输出：3
解释：
在这个示例中，我们选择从 l = 0 开始、到 r = 2 结束的子数组 => [2,3,4] 。 
该子数组满足上述全部条件。
因此，答案就是这个子数组的长度 3 。可以证明 3 是满足题目要求的最大长度。
```

__提示：__

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
- `1 <= threshold <= 100`

__思路:__

```text
滑动窗口
从 0 开始先找到一个 i 使得 nums[i] 不大于 threshold 且是偶数
作为滑动窗口的起点
然后找到结束点使得 nums[i] 大于 threshold 或者相邻两个元素奇偶性相同
此时 [start, i) 中的元素满足题意
长度为 i - start
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestAlternatingSubarray(vector<int>& nums, int threshold) 
    {
        int result = 0, n = nums.size(), start = 0, i = 0;
        while (i < n) {
            if (nums[i] > threshold or (nums[i] & 1)) 
            {
                ++i;
                continue;
            }
            start = i++;
            while (i < n and nums[i] <= threshold and ((nums[i] ^ nums[i - 1]) & 1)) ++i;
            result = max(result, i - start);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestAlternatingSubarray(int[] nums, int threshold) {
        int result = 0, n = nums.length, start = 0, i = 0;
        while (i < n) {
            if (nums[i] > threshold || (nums[i] & 1) == 1) {
                ++i;
                continue;
            }
            start = i++;
            while (i < n && nums[i] <= threshold && ((nums[i] ^ nums[i - 1]) & 1) == 1) ++i;
            result = Math.max(result, i - start);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestAlternatingSubarray(self, nums: List[int], threshold: int) -> int:
        return max([right - left + 1 for left in range(len(nums)) for right in range(left, len(nums)) if not (nums[left] & 1) and all(((x ^ y) & 1 == 1) for x, y in pairwise(nums[left:right + 1])) and all(x <= threshold for x in nums[left:right + 1])], default=0)
```
