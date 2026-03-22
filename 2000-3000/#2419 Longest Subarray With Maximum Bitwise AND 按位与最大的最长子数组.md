# 2419 Longest Subarray With Maximum Bitwise AND 按位与最大的最长子数组

__Description:__

You are given an integer array `nums` of size `n`.

Consider a __non-empty__ subarray from `nums` that has the __maximum__ possible __bitwise AND__.

- In other words, let `k` be the maximum value of the bitwise AND of __any__ subarray of `nums`. Then, only subarrays with a bitwise AND equal to `k` should be considered.

Return the length of the longest such subarray.

The bitwise AND of an array is the bitwise AND of all the numbers in it.

A subarray is a contiguous sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,3,2,2]
Output: 2
Explanation:
The maximum possible bitwise AND of a subarray is 3.
The longest subarray with that value is [3,3], so we return 2.
```

Example 2:

```text
Input: nums = [1,2,3,4]
Output: 1
Explanation:
The maximum possible bitwise AND of a subarray is 4.
The longest subarray with that value is [4], so we return 1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个长度为 `n` 的整数数组 `nums` 。

考虑 `nums` 中进行 __按位与（bitwise AND）__运算得到的值 __最大__ 的 __非空__ 子数组。

- 换句话说，令 `k` 是 `nums` __任意__ 子数组执行按位与运算所能得到的最大值。那么，只需要考虑那些执行一次按位与运算后等于 `k` 的子数组。

返回满足要求的 最长 子数组的长度。

数组的按位与就是对数组中的所有数字进行按位与运算。

子数组 是数组中的一个连续元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,3,2,2]
输出：2
解释：
子数组按位与运算的最大值是 3 。
能得到此结果的最长子数组是 [3,3]，所以返回 2 。
```

示例 2：

```text
输入：nums = [1,2,3,4]
输出：1
解释：
子数组按位与运算的最大值是 4 。 
能得到此结果的最长子数组是 [4]，所以返回 1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 6`

__思路:__

```text
模拟
由于与运算不会增加
所以与运算的最大值一定是数组中的最大值
统计最大值的连续的个数即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestSubarray(vector<int>& nums) 
    {
        int mx = *max_element(nums.begin(), nums.end()), result = 0, cur = 0;
        for (const auto& x : nums) 
        {
            if (x == mx) 
            {
                ++cur;
                result = max(result, cur);
            } 
            else cur = 0;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestSubarray(int[] nums) {
        int mx = Arrays.stream(nums).max().getAsInt(), result = 0, cur = 0;
        for (int x : nums) {
            if (x == mx) {
                ++cur;
                result = Math.max(result, cur);
            } else cur = 0;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        result = mx = cur = 0
        for x in nums:
            if x > mx:
                mx = x
                result = cur = 1
            elif x == mx:
                cur += 1
                if cur > result:
                    result = cur
            else:
                cur = 0
        return result
```
