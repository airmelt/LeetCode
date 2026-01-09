# 2841 Maximum Sum of Almost Unique Subarray 几乎唯一子数组的最大和

__Description:__

You are given an integer array `nums` and two positive integers `m` and `k`.

Return _the __maximum sum__ out of all __almost unique__ subarrays of length_ `k` _of_ `nums`. If no such subarray exists, return `0`.

A subarray of `nums` is __almost unique__ if it contains at least `m` distinct elements.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input:  nums = [2,6,7,3,1,7], m = 3, k = 4
Output:  18
Explanation:  There are 3 almost unique subarrays of size k = 4. These subarrays are [2, 6, 7, 3], [6, 7, 3, 1], and [7, 3, 1, 7]. Among these subarrays, the one with the maximum sum is [2, 6, 7, 3] which has a sum of 18.
```

Example 2:

```text
Input: nums = [5,9,9,2,4,5,4], m = 1, k = 3
Output: 23
Explanation: There are 5 almost unique subarrays of size k. These subarrays are [5, 9, 9], [9, 9, 2], [9, 2, 4], [2, 4, 5], and [4, 5, 4]. Among these subarrays, the one with the maximum sum is [5, 9, 9] which has a sum of 23.
```

Example 3:

```text
Input:  nums = [1,2,1,2,1,2,1], m = 3, k = 3
Output:  0
Explanation:  There are no subarrays of size k = 3 that contain at least m = 3 distinct elements in the given array [1,2,1,2,1,2,1]. Therefore, no almost unique subarrays exist, and the maximum sum is 0.
```

__Constraints:__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `1 <= m <= k <= nums.length`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个整数数组 `nums` 和两个正整数 `m` 和 `k` 。

请你返回 `nums` 中长度为 `k` 的 __几乎唯一__ 子数组的 __最大和__ ，如果不存在几乎唯一子数组，请你返回 `0` 。

如果 `nums` 的一个子数组有至少 `m` 个互不相同的元素，我们称它是 __几乎唯一__ 子数组。

子数组指的是一个数组中一段连续 非空 的元素序列。

__示例:__

示例 1：

```text
输入：nums = [2,6,7,3,1,7], m = 3, k = 4
输出：18
解释：总共有 3 个长度为 k = 4 的几乎唯一子数组。分别为 [2, 6, 7, 3] ，[6, 7, 3, 1] 和 [7, 3, 1, 7] 。这些子数组中，和最大的是 [2, 6, 7, 3] ，和为 18 。
```

示例 2：

```text
输入：nums = [5,9,9,2,4,5,4], m = 1, k = 3
输出：23
解释：总共有 5 个长度为 k = 3 的几乎唯一子数组。分别为 [5, 9, 9] ，[9, 9, 2] ，[9, 2, 4] ，[2, 4, 5] 和 [4, 5, 4] 。这些子数组中，和最大的是 [5, 9, 9] ，和为 23 。
```

示例 3：

```text
输入: nums = [1,2,1,2,1,2,1], m = 3, k = 3
输出: 0
解释: 输入数组中不存在长度为 k = 3 的子数组含有至少 m = 3 个互不相同元素的子数组。所以不存在几乎唯一子数组，最大和为 0 。
```

__提示：__

- `1 <= nums.length <= 2 * 10 ^ 4`
- `1 <= m <= k <= nums.length`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
滑动窗口
当窗口里的元素个数为 k 时
检查窗口里的不同元素个数是否大于等于 m
如果是, 更新结果为窗口内的和
时间复杂度为 O(N), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxSum(vector<int>& nums, int m, int k) 
    {
        long long result = 0LL, s = 0LL;
        unordered_map<int, int> visited;
        for (int i = 0, n = nums.size(), left = 0; i < n; i++) 
        {
            s += nums[i];
            ++visited[nums[i]];
            if ((left = i - k + 1) > -1) 
            {
                if (visited.size() >= m) result = max(result, s);
                s -= nums[left];
                if (!(--visited[nums[left]])) visited.erase(nums[left]);
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxSum(List<Integer> nums, int m, int k) {
        long result = 0L, s = 0L;
        var visited = new HashMap<Integer, Integer>();
        for (int i = 0, n = nums.size(), left = 0; i < n; i++) {
            s += nums.get(i);
            visited.merge(nums.get(i), 1, Integer::sum);
            if ((left = i - k + 1) > -1) {
                if (visited.size() >= m) result = Math.max(result, s);
                s -= nums.get(left);
                if (visited.merge(nums.get(left), -1, Integer::sum) == 0) visited.remove(nums.get(left));
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSum(self, nums: List[int], m: int, k: int) -> int:
        pre, n, result = list(accumulate(nums)), len(nums), 0 if len(visited := Counter(nums[:k])) < m else sum(nums[:k])
        for i in range(k, n):
            visited[nums[i - k]] -= 1
            if not visited[nums[i - k]]:
                del visited[nums[i - k]]
            visited[nums[i]] += 1
            if len(visited) >= m:
                result = max(result, pre[i] - pre[i - k])
        return result
```
