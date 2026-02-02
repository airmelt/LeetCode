# 2871 Split Array Into Maximum Number of Subarrays 将数组分割成最多数目的子数组

__Description:__

You are given an array `nums` consisting of __non-negative__ integers.

We define the score of subarray `nums[l..r]` such that `l <= r` as `nums[l] AND nums[l + 1] AND ... AND nums[r]` where __AND__ is the bitwise `AND` operation.

Consider splitting the array into one or more subarrays such that the following conditions are satisfied:

- __E____ach__ element of the array belongs to __exactly__ one subarray.
- The sum of scores of the subarrays is the __minimum__ possible.

Return the maximum number of subarrays in a split that satisfies the conditions above.

A subarray is a contiguous part of an array.

__Example:__

Example 1:

```text
Input: nums = [1,0,2,0,1,2]
Output: 3
Explanation: We can split the array into the following subarrays:
```

- [1,0]. The score of this subarray is 1 AND 0 = 0.
- [2,0]. The score of this subarray is 2 AND 0 = 0.
- [1,2]. The score of this subarray is 1 AND 2 = 0.

The sum of scores is 0 + 0 + 0 = 0, which is the minimum possible score that we can obtain.
It can be shown that we cannot split the array into more than 3 subarrays with a total score of 0. So we return 3.

Example 2:

```text
Input: nums = [5,7,1,3]
Output: 1
Explanation: We can split the array into one subarray: [5,7,1,3] with a score of 1, which is the minimum possible score that we can obtain.
It can be shown that we cannot split the array into more than 1 subarray with a total score of 1. So we return 1.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个只包含 __非负__ 整数的数组 `nums` 。

我们定义满足 `l <= r` 的子数组 `nums[l..r]` 的分数为 `nums[l] AND nums[l + 1] AND ... AND nums[r]` ，其中 __AND__ 是按位与运算。

请你将数组分割成一个或者更多子数组，满足：

- __每个__ 元素都 __只__ 属于一个子数组。
- 子数组分数之和尽可能 __小__ 。

请你在满足以上要求的条件下，返回 最多 可以得到多少个子数组。

一个 子数组 是一个数组中一段连续的元素。

__示例:__

示例 1：

```text
输入：nums = [1,0,2,0,1,2]
输出：3
解释：我们可以将数组分割成以下子数组：
```

- [1,0] 。子数组分数为 1 AND 0 = 0 。
- [2,0] 。子数组分数为 2 AND 0 = 0 。
- [1,2] 。子数组分数为 1 AND 2 = 0 。

分数之和为 0 + 0 + 0 = 0 ，是我们可以得到的最小分数之和。
在分数之和为 0 的前提下，最多可以将数组分割成 3 个子数组。所以返回 3 。

示例 2：

```text
输入：nums = [5,7,1,3]
输出：1
解释：我们可以将数组分割成一个子数组：[5,7,1,3] ，分数为 1 ，这是可以得到的最小总分数。
在总分数为 1 的前提下，最多可以将数组分割成 1 个子数组。所以返回 1 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__思路:__

```text
位运算
由于与运算不会使得值增加
所以只有两种情况
要么子数组的与运算为 0
要么整个数组不可分割作为一个整体, 这时候返回 1
否则找到所有子数组与运算为 0 的, 立即分组直到结束
可以利用 -1 & a = a 简化运算
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSubarrays(vector<int>& nums) 
    {
        int result = 0, cur = -1;
        for (int num : nums) 
        {
            if (!(cur &= num)) 
            {
                ++result;
                cur = -1;
            }
        }
        return max(result, 1);
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSubarrays(int[] nums) {
        int result = 0, cur = -1;
        for (int num : nums) {
            if ((cur &= num) == 0) {
                ++result;
                cur = -1;
            }
        }
        return Math.max(result, 1);
    }
}
```

__Python__:

```Python
class Solution:
    def maxSubarrays(self, nums: List[int]) -> int:
        result = not (cur := nums[0])
        for num in nums[1:]:
            result += not (cur := (cur & num) if cur else num)
        return int(result) if result else result + 1
```
