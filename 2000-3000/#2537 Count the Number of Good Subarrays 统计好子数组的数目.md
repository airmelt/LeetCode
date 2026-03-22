# 2537 Count the Number of Good Subarrays 统计好子数组的数目

__Description:__

Given an integer array `nums` and an integer `k`, return _the number of __good__ subarrays of_ `nums`.

A subarray `arr` is __good__ if there are __at least__ `k` pairs of indices `(i, j)` such that `i < j` and `arr[i] == arr[j]`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,1,1,1,1], k = 10
Output: 1
Explanation: The only good subarray is the array nums itself.
```

Example 2:

```text
Input: nums = [3,1,4,3,2,2,4], k = 2
Output: 4
Explanation: There are 4 different good subarrays:
```

- [3,1,4,3,2,2] that has 2 pairs.
- [3,1,4,3,2,2,4] that has 3 pairs.
- [1,4,3,2,2,4] that has 2 pairs.
- [4,3,2,2,4] that has 2 pairs.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回 `nums` 中 __好__ 子数组的数目。

一个子数组 `arr` 如果有 __至少__ `k` 对下标 `(i, j)` 满足 `i < j` 且 `arr[i] == arr[j]` ，那么称它是一个 __好__ 子数组。

子数组 是原数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,1,1,1,1], k = 10
输出：1
解释：唯一的好子数组是这个数组本身。
```

示例 2：

```text
输入：nums = [3,1,4,3,2,2,4], k = 2
输出：4
解释：总共有 4 个不同的好子数组：
```

- [3,1,4,3,2,2] 有 2 对。
- [3,1,4,3,2,2,4] 有 3 对。
- [1,4,3,2,2,4] 有 2 对。
- [4,3,2,2,4] 有 2 对。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i], k <= 10 ^ 9`

__思路:__

```text
滑动窗口
每次将右边界加入窗口
固定右边界, 如果当前窗口的好子数组数目大于等于 k, 则将左边界向右移动
直到当前窗口的好子数组数目小于 k
那么从 [0, left) 的所有子数组都是好子数组
所以 result 需要加上 left
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countGood(vector<int>& nums, int k) 
    {
        unordered_map<int, int> m;
        long long result = 0LL, cur = 0LL, left = 0LL;
        for (const auto& num : nums) 
        {
            cur += m[num]++;
            while (cur >= k) cur -= --m[nums[left++]];
            result += left;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countGood(int[] nums, int k) {
        var map = new HashMap<Long, Long>();
        long result = 0L, cur = 0L, left = 0L;
        for (int num : nums) {
            cur += map.merge((long)num, 1L, Long::sum) - 1;
            while (cur >= k) cur -= map.merge((long)nums[(int)left++], -1L, Long::sum);
            result += left;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countGood(self, nums: List[int], k: int) -> int:
        d, result, left, cur = defaultdict(int), 0, 0, 0
        for num in nums:
            cur += d[num]
            d[num] += 1
            while cur >= k:
                d[nums[left]] -= 1
                cur -= d[nums[left]]
                left += 1
            result += left
        return result
```
