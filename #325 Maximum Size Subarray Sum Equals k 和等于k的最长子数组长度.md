# 325 Maximum Size Subarray Sum Equals k 和等于k的最长子数组长度

__Description:__

Given an integer array `nums` and an integer `k`, return _the maximum length of a subarray that sums to_ `k`. If there is not one, return `0` instead.

__Example:__

Example 1:

```text
Input: nums = [1,-1,5,-2,3], k = 3
Output: 4
Explanation: The subarray [1, -1, 5, -2] sums to 3 and is the longest.
```

Example 2:

```text
Input: nums = [-2,-1,2,1], k = 1
Output: 2
Explanation: The subarray [-1, 2] sums to 1 and is the longest.
```

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`
- `-10 ^ 9 <= k <= 10 ^ 9`

__题目描述:__

给定一个数组 `nums` 和一个目标值 `k`，找到和等于 _`k`_ 的最长连续子数组长度。如果不存在任意一个符合要求的子数组，则返回 `0`。

__示例:__

示例 1:

```text
输入: nums = [1,-1,5,-2,3], k = 3
输出: 4 
解释: 子数组 [1, -1, 5, -2] 和等于 3，且长度最长。
```

示例 2:

```text
输入: nums = [-2,-1,2,1], k = 1
输出: 2 
解释: 子数组 [-1, 2] 和等于 1，且长度最长。
```

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 5`
- `-10 ^ 4 <= nums[i] <= 10 ^ 4`
- `-10 ^ 9 <= k <= 10 ^ 9`

__思路:__

```text
前缀和 ➕ 哈希
将前缀和存放在哈希表中
如果 total - k 在哈希表中能找到, 说明这一段子数组的和就为 k
更新最长的子数组
为了保证最长, 只需要存储第一次出现前缀和的位置
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSubArrayLen(vector<int>& nums, int k) 
    {
        int n = nums.size(), total = 0, result = 0;
        unordered_map<long, int> record;
        record[0] = -1;
        for (int i = 0; i < n; i++) 
        {
            total += nums[i];
            if (record.count((long)total - k)) result = max(result, i - record[total - k]);
            if (!record.count(total)) record[total] = i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        int n = nums.length, total = 0, result = 0;
        Map<Integer, Integer> record = new HashMap<>();
        record.put(0, -1);
        for (int i = 0; i < n; i++) {
            total += nums[i];
            if (record.containsKey(total - k)) result = Math.max(result, i - record.get(total - k));
            if (!record.containsKey(total)) record.put(total, i);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        record, result, total = {0: -1}, 0, 0
        for i, v in enumerate(nums):
            total += v
            if total - k in record:
                result = max(result, i - record[total - k])
            if total not in record:
                record[total] = i
        return result
```
