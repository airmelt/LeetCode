# 2588 Count the Number of Beautiful Subarrays 统计美丽子数组数目

__Description:__

You are given a __0-indexed__ integer array `nums`. In one operation, you can:

- Choose two different indices `i` and `j` such that `0 <= i, j < nums.length`.
- Choose a non-negative integer `k` such that the `k ^ th` bit (__0-indexed__) in the binary representation of `nums[i]` and `nums[j]` is `1`.
- Subtract `2 ^ k` from `nums[i]` and `nums[j]`.

A subarray is __beautiful__ if it is possible to make all of its elements equal to `0` after applying the above operation any number of times (including zero).

Return _the number of __beautiful subarrays__ in the array_ `nums`.

A subarray is a contiguous non-empty sequence of elements within an array.

Note: Subarrays where all elements are initially 0 are considered beautiful, as no operation is needed.

__Example:__

Example 1:

```text
Input: nums = [4,3,1,2,4]
Output: 2
Explanation: There are 2 beautiful subarrays in nums: [4,3,1,2,4] and [4,3,1,2,4].
```

- We can make all elements in the subarray [3,1,2] equal to 0 in the following way:
  - Choose [3, 1, 2] and k = 1. Subtract 21 from both numbers. The subarray becomes [1, 1, 0].
  - Choose [1, 1, 0] and k = 0. Subtract 20 from both numbers. The subarray becomes [0, 0, 0].
- We can make all elements in the subarray [4,3,1,2,4] equal to 0 in the following way:
  - Choose [4, 3, 1, 2, 4] and k = 2. Subtract 22 from both numbers. The subarray becomes [0, 3, 1, 2, 0].
  - Choose [0, 3, 1, 2, 0] and k = 0. Subtract 20 from both numbers. The subarray becomes [0, 2, 0, 2, 0].
  - Choose [0, 2, 0, 2, 0] and k = 1. Subtract 21 from both numbers. The subarray becomes [0, 0, 0, 0, 0].

Example 2:

```text
Input: nums = [1,10,4]
Output: 0
Explanation: There are no beautiful subarrays in nums.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。每次操作中，你可以:

- 选择两个满足 `0 <= i, j < nums.length` 的不同下标 `i` 和 `j` 。
- 选择一个非负整数 `k` ，满足 `nums[i]` 和 `nums[j]` 在二进制下的第 `k` 位（下标编号从 __0__ 开始）是 `1` 。
- 将 `nums[i]` 和 `nums[j]` 都减去 `2 ^ k` 。

如果一个子数组内执行上述操作若干次（包括 0 次）后，该子数组可以变成一个全为 `0` 的数组，那么我们称它是一个 __美丽__ 的子数组。

请你返回数组 `nums` 中 __美丽子数组__ 的数目。

子数组是一个数组中一段连续 非空 的元素序列。

注意：所有元素最初都是 0 的子数组被认为是美丽的，因为不需要进行任何操作。

__示例:__

示例 1：

```text
输入：nums = [4,3,1,2,4]
输出：2
解释：nums 中有 2 个美丽子数组：[4,3,1,2,4] 和 [4,3,1,2,4] 。
```

- 按照下述步骤，我们可以将子数组 [3,1,2] 中所有元素变成 0 ：
  - 选择 [3, 1, 2] 和 k = 1 。将 2 个数字都减去 21 ，子数组变成 [1, 1, 0] 。
  - 选择 [1, 1, 0] 和 k = 0 。将 2 个数字都减去 20 ，子数组变成 [0, 0, 0] 。
- 按照下述步骤，我们可以将子数组 [4,3,1,2,4] 中所有元素变成 0 ：
  - 选择 [4, 3, 1, 2, 4] 和 k = 2 。将 2 个数字都减去 22 ，子数组变成 [0, 3, 1, 2, 0] 。
  - 选择 [0, 3, 1, 2, 0] 和 k = 0 。将 2 个数字都减去 20 ，子数组变成 [0, 2, 0, 2, 0] 。
  - 选择 [0, 2, 0, 2, 0] 和 k = 1 。将 2 个数字都减去 21 ，子数组变成 [0, 0, 0, 0, 0] 。

示例 2：

```text
输入：nums = [1,10,4]
输出：0
解释：nums 中没有任何美丽子数组。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `0 <= nums[i] <= 10 ^ 6`

__思路:__

```text
位运算
将每个数字转换为二进制形式
使用异或运算来计算前缀和
将 0: 1 加入到哈希表中
遍历数组, 对每个数字进行异或运算, 更新前缀和
如果前缀和已经存在于哈希表中, 则说明有美丽子数组
将前缀和的计数加到结果中
将前缀和加入到哈希表中, 计数加 1
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long beautifulSubarrays(vector<int>& nums) 
    {
        long long result = 0LL, mask = 0LL;
        unordered_map<long long, int> m;
        m[0LL] = 1;
        for (const auto& num : nums) 
        {
            mask ^= num;
            result += m[mask];
            ++m[mask];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long beautifulSubarrays(int[] nums) {
        long result = 0L, mask = 0L;
        var map = new HashMap<Long, Integer>();
        map.put(0L, 1);
        for (int num : nums) {
            mask ^= num;
            result += map.getOrDefault(mask, 0);
            map.merge(mask, 1, Integer::sum);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def beautifulSubarrays(self, nums: List[int]) -> int:
        c, mask, result = {0: 1}, 0, 0
        for x in nums:
            mask ^= x
            result += c.get(mask, 0)
            c[mask] = c.get(mask, 0) + 1
        return result
```
