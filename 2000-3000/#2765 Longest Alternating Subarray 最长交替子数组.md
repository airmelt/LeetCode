# 2765 Longest Alternating Subarray 最长交替子数组

__Description:__

You are given a __0-indexed__ integer array `nums`. A subarray `s` of length `m` is called __alternating__ if:

- `m` is greater than `1`.
- `s1 = s0 + 1`.
- The __0-indexed__ subarray `s` looks like `[s0, s1, s0, s1,...,s(m-1) % 2]`. In other words, `s1 - s0 = 1`, `s2 - s1 = -1`, `s3 - s2 = 1`, `s4 - s3 = -1`, and so on up to `s[m - 1] - s[m - 2] = (-1) ^ m`.

Return _the maximum length of all __alternating__ subarrays present in_ `nums` _or_ `-1` _if no such subarray exists__._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [2,3,4,3,4]

Output: 4

Explanation:

The alternating subarrays are [2, 3], [3,4], [3,4,3], and [3,4,3,4]. The longest of these is `[3,4,3,4]`, which is of length 4.
```

Example 2:

```text
Input: nums = [4,5,6]

Output: 2

Explanation:

[4,5] and [5,6] are the only two alternating subarrays. They are both of length 2.
```

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 4`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果 `nums` 中长度为 `m` 的子数组 `s` 满足以下条件，我们称它是一个 __交替子数组__ :

- `m` 大于 `1` 。
- `s1 = s0 + 1` 。
- 下标从 __0__ 开始的子数组 `s` 与数组 `[s0, s1, s0, s1,...,s(m-1) % 2]` 一样。也就是说， `s1 - s0 = 1` ， `s2 - s1 = -1` ， `s3 - s2 = 1` ， `s4 - s3 = -1` ，以此类推，直到 `s[m - 1] - s[m - 2] = (-1) ^ m` 。

请你返回 `nums` 中所有 __交替__ 子数组中，最长的长度，如果不存在交替子数组，请你返回 `-1` 。

子数组是一个数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,3,4,3,4]
输出：4
解释：交替子数组有 [2,3]，[3,4]，[3,4,3] 和 [3,4,3,4]。最长的子数组为 [3,4,3,4]，长度为 4。
```

示例 2：

```text
输入：nums = [4,5,6]
输出：2
解释：[4,5] 和 [5,6] 是仅有的两个交替子数组。它们长度都为 2 。
```

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i] <= 10 ^ 4`

__思路:__

```text
1. 暴力法
检查所有子数组是否满足题意
选择最长的子数组
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
2. 分组
从 0 开始遍历数组
如果一开始就不满足下一个和当前值差值不为 1 就直接跳到下一个遍历
否则记录一下开始位置
从第 3 个元素开始检查是否和前面第 2 个元素相等
因为数组是 1, -1, 1, -1 摆动的
所以当前元素和前面第 2 个元素必相等
直到跳出循环
此时 [start, i - 1] 是满足题意的子数组
长度为 i - start
然后从 i - 1 开始继续查找下一个子数组
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    int alternatingSubarray(vector<int>& nums)
    {
        int i = 0, result = -1, n = nums.size();
        while (i < n - 1)
        {
            if (nums[i + 1] - nums[i] != 1)
            {
                ++i;
                continue;
            }
            int start = i;
            i += 2;
            while (i < n and nums[i] == nums[i - 2]) ++i;
            result = max(result, i-- - start);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int alternatingSubarray(int[] nums) {
        int n = nums.length, i = 0, result = -1;
        while (i < n - 1) {
            if (nums[i + 1] - nums[i] != 1) {
                ++i;
                continue;
            }
            int start = i;
            i += 2;
            while (i < n && nums[i] == nums[i - 2]) ++i;
            result = Math.max(result, i-- - start);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def alternatingSubarray(self, nums: List[int]) -> int:
        return max([len(s) for i in range(len(nums)) for j in range(i + 1, len(nums)) if (s := nums[i:j + 1]) and all(s[k] - s[k - 1] == (-1) ** (k + 1) for k in range(1, len(s)))], default=-1)
```
