# 2401 Longest Nice Subarray 最长优雅子数组

__Description:__

You are given an array `nums` consisting of __positive__ integers.

We call a subarray of `nums` __nice__ if the bitwise __AND__ of every pair of elements that are in __different__ positions in the subarray is equal to `0`.

Return the length of the longest nice subarray.

A subarray is a contiguous part of an array.

__Note__ that subarrays of length `1` are always considered nice.

__Example:__

Example 1:

```text
Input: nums = [1,3,8,48,10]
Output: 3
Explanation: The longest nice subarray is [3,8,48]. This subarray satisfies the conditions:
```

- 3 AND 8 = 0.
- 3 AND 48 = 0.
- 8 AND 48 = 0.

It can be proven that no longer nice subarray can be obtained, so we return 3.

Example 2:

```text
Input: nums = [3,1,5,11,13]
Output: 1
Explanation: The length of the longest nice subarray is 1. Any subarray of length 1 can be chosen.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个由 __正__ 整数组成的数组 `nums` 。

如果 `nums` 的子数组中位于 __不同__ 位置的每对元素按位 __与（AND）__运算的结果等于 `0` ，则称该子数组为 __优雅__ 子数组。

返回 最长 的优雅子数组的长度。

子数组 是数组中的一个 连续 部分。

__注意:__长度为 `1` 的子数组始终视作优雅子数组。

__示例:__

示例 1：

```text
输入：nums = [1,3,8,48,10]
输出：3
解释：最长的优雅子数组是 [3,8,48] 。子数组满足题目条件：
```

- 3 AND 8 = 0
- 3 AND 48 = 0
- 8 AND 48 = 0

可以证明不存在更长的优雅子数组，所以返回 3 。

示例 2：

```text
输入：nums = [3,1,5,11,13]
输出：1
解释：最长的优雅子数组长度为 1 ，任何长度为 1 的子数组都满足题目条件。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
滑动窗口
记录窗口内所有的数字的或的结果 cur
如果 cur 和当前遍历的值 x 有交叉
说明与运算肯定不为 0, 移动窗口的左边界
记录窗口的最大值即可
使用异或来增加窗口内的值或者弹出值
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestNiceSubarray(vector<int>& nums) 
    {
        int result = 1, left = 0, cur = 0, n = nums.size();
        for (int right = 0; right < n; right++) 
        {
            while (cur & nums[right]) cur ^= nums[left++];
            cur |= nums[right];
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestNiceSubarray(int[] nums) {
        int result = 1, left = 0, cur = 0, n = nums.length;
        for (int right = 0; right < n; right++) {
            while ((cur & nums[right]) != 0) cur ^= nums[left++];
            cur |= nums[right];
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestNiceSubarray(self, nums: List[int]) -> int:
        result = left = cur = 0
        for right, x in enumerate(nums):
            while cur & x:
                cur ^= nums[left]
                left += 1
            cur |= x
            result = max(result, right - left + 1)
        return result
```
