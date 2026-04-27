# 2364 Count Number of Bad Pairs 统计坏数对的数目

__Description:__

You are given a __0-indexed__ integer array `nums`. A pair of indices `(i, j)` is a __bad pair__ if `i < j` and `j - i != nums[j] - nums[i]`.

Return _the total number of __bad pairs__ in_ `nums`.

__Example:__

Example 1:

```text
Input: nums = [4,1,3,3]
Output: 5
Explanation: The pair (0, 1) is a bad pair since 1 - 0 != 1 - 4.
The pair (0, 2) is a bad pair since 2 - 0 != 3 - 4, 2 != -1.
The pair (0, 3) is a bad pair since 3 - 0 != 3 - 4, 3 != -1.
The pair (1, 2) is a bad pair since 2 - 1 != 3 - 1, 1 != 2.
The pair (2, 3) is a bad pair since 3 - 2 != 3 - 3, 1 != 0.
There are a total of 5 bad pairs, so we return 5.
```

Example 2:

```text
Input: nums = [1,2,3,4,5]
Output: 0
Explanation: There are no bad pairs.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `nums` 。如果 `i < j` 且 `j - i != nums[j] - nums[i]` ，那么我们称 `(i, j)` 是一个 __坏数对__ 。

请你返回 `nums` 中 __坏数对__ 的总数目。

__示例:__

示例 1：

```text
输入：nums = [4,1,3,3]
输出：5
解释：数对 (0, 1) 是坏数对，因为 1 - 0 != 1 - 4 。
数对 (0, 2) 是坏数对，因为 2 - 0 != 3 - 4, 2 != -1 。
数对 (0, 3) 是坏数对，因为 3 - 0 != 3 - 4, 3 != -1 。
数对 (1, 2) 是坏数对，因为 2 - 1 != 3 - 1, 1 != 2 。
数对 (2, 3) 是坏数对，因为 3 - 2 != 3 - 3, 1 != 0 。
总共有 5 个坏数对，所以我们返回 5 。
```

示例 2：

```text
输入：nums = [1,2,3,4,5]
输出：0
解释：没有坏数对。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
哈希表
由于需要统计 i - j != nums[i] - nums[j] 的数目
移项可得 i - nums[i] != j - nums[j]
实际上可以统计 i - nums[i] 的数目
然后根据排列组合的知识，假设有 v 个 i - nums[i]，那么好数对的数目为 v * (v + 1) / 2
总数目为 N * (N + 1) / 2
坏数对的数目为总数目减去好数对的数目
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countBadPairs(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        long long cur = 0LL, total = (long)nums.size() * (nums.size() + 1LL);
        for (int i = 0, n = nums.size(); i < n; i++) ++m[i - nums[i]];
        for (const auto& [_, v] : m) cur += (long long)v * (v + 1LL);
        return (total - cur) >> 1LL;
    }
};
```

__Java__:

```Java
class Solution {
    public long countBadPairs(int[] nums) {
        var map = new HashMap<Integer, Integer>();
        long cur = 0L, total = (long)nums.length * (nums.length + 1L);
        for (int i = 0, n = nums.length; i < n; i++) map.merge(i - nums[i], 1, Integer::sum);
        for (var e : map.entrySet()) cur += (long)e.getValue() * (e.getValue() + 1L);
        return (total - cur) >> 1;
    }
}
```

__Python__:

```Python
class Solution:
    def countBadPairs(self, nums: List[int]) -> int:
        return (len(nums) * (len(nums) + 1) - sum(v * (v + 1) for v in Counter([i - v for i, v in enumerate(nums)]).values())) >> 1
```
