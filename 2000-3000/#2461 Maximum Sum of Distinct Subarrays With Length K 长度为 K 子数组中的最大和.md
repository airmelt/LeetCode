# 2461 Maximum Sum of Distinct Subarrays With Length K 长度为 K 子数组中的最大和

__Description:__

You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:

- The length of the subarray is `k`, and
- All the elements of the subarray are __distinct__.

Return _the maximum subarray sum of all the subarrays that meet the conditions__._ If no subarray meets the conditions, return `0`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,5,4,2,9,9,9], k = 3
Output: 15
Explanation: The subarrays of nums with length 3 are:
```

- [1,5,4] which meets the requirements and has a sum of 10.
- [5,4,2] which meets the requirements and has a sum of 11.
- [4,2,9] which meets the requirements and has a sum of 15.
- [2,9,9] which does not meet the requirements because the element 9 is repeated.
- [9,9,9] which does not meet the requirements because the element 9 is repeated.

We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions

Example 2:

```text
Input: nums = [4,4,4], k = 3
Output: 0
Explanation: The subarrays of nums with length 3 are:
- [4,4,4] which does not meet the requirements because the element 4 is repeated.
We return 0 because no subarrays meet the conditions.
```

__Constraints:__

- `1 <= k <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。请你从 `nums` 中满足下述条件的全部子数组中找出最大子数组和:

- 子数组的长度是 `k`，且
- 子数组中的所有元素 __各不相同 。__

返回满足题面要求的最大子数组和。如果不存在子数组满足这些条件，返回 `0` 。

子数组 是数组中一段连续非空的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,5,4,2,9,9,9], k = 3
输出：15
解释：nums 中长度为 3 的子数组是：
```

- [1,5,4] 满足全部条件，和为 10 。
- [5,4,2] 满足全部条件，和为 11 。
- [4,2,9] 满足全部条件，和为 15 。
- [2,9,9] 不满足全部条件，因为元素 9 出现重复。
- [9,9,9] 不满足全部条件，因为元素 9 出现重复。

因为 15 是满足全部条件的所有子数组中的最大子数组和，所以返回 15 。

示例 2：

```text
输入：nums = [4,4,4], k = 3
输出：0
解释：nums 中长度为 3 的子数组是：
- [4,4,4] 不满足全部条件，因为元素 4 出现重复。
因为不存在满足全部条件的子数组，所以返回 0 。
```

__提示：__

- `1 <= k <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`

__思路:__

```text
滑动窗口
用一个哈希表记录窗口中的元素个数
如果窗口中的元素个数等于 k, 计算窗口中的和
并更新最大和
如果左指针指向的元素为 0, 删除该元素
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maximumSubarraySum(vector<int>& nums, int k) 
    {
        long long result = 0LL, s = 0LL;
        unordered_map<int, int> m;
        for (int i = 0; i < k - 1; i++) 
        {
            ++m[nums[i]];
            s += nums[i];
        }
        for (int i = k - 1, n = nums.size(); i < n; i++) 
        {
            ++m[nums[i]];
            s += nums[i];
            if (m.size() == k and s > result) result = s;
            if (!--m[nums[i + 1 - k]]) m.erase(nums[i + 1 - k]);
            s -= nums[i + 1 - k];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        long result = 0L, s = 0L;
        var map = new HashMap<Integer, Integer>();
        for (int i = 0; i < k - 1; i++) {
            map.merge(nums[i], 1, Integer::sum);
            s += nums[i];
        }
        for (int i = k - 1, n = nums.length; i < n; i++) {
            map.merge(nums[i], 1, Integer::sum);
            s += nums[i];
            if (map.size() == k && s > result) result = s;
            map.merge(nums[i + 1 - k], -1, Integer::sum);
            if (map.get(nums[i + 1 - k]) == 0) map.remove(nums[i + 1 - k]);
            s -= nums[i + 1 - k];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        pre, result, n = list(accumulate(nums)), 0 if len(visited := Counter(nums[:k])) < k else (pre := list(accumulate(nums)))[k - 1], len(nums)
        for i in range(k, n):
            visited[nums[i - k]] -= 1
            if not visited[nums[i - k]]:
                del visited[nums[i - k]]
            visited[nums[i]] += 1
            result = max(result, pre[i] - pre[i - k]) if len(visited) == k else result
        return result
```
