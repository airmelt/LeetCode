# 2958 Length of Longest Subarray With at Most K Frequency 最多 K 个重复元素的最长子数组

__Description:__

You are given an integer array `nums` and an integer `k`.

The __frequency__ of an element `x` is the number of times it occurs in an array.

An array is called __good__ if the frequency of each element in this array is __less than or equal__ to `k`.

Return _the length of the __longest__ __good__ subarray of_ `nums`_._

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,1,2,3,1,2], k = 2
Output: 6
Explanation: The longest possible good subarray is [1,2,3,1,2,3] since the values 1, 2, and 3 occur at most twice in this subarray. Note that the subarrays [2,3,1,2,3,1] and [3,1,2,3,1,2] are also good.
It can be shown that there are no good subarrays with length more than 6.
```

Example 2:

```text
Input: nums = [1,2,1,2,1,2,1,2], k = 1
Output: 2
Explanation: The longest possible good subarray is [1,2] since the values 1 and 2 occur at most once in this subarray. Note that the subarray [2,1] is also good.
It can be shown that there are no good subarrays with length more than 2.
```

Example 3:

```text
Input: nums = [5,5,5,5,5,5,5], k = 4
Output: 4
Explanation: The longest possible good subarray is [5,5,5,5] since the value 5 occurs 4 times in this subarray.
It can be shown that there are no good subarrays with length more than 4.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= nums.length`

__题目描述:__

给你一个整数数组 `nums` 和一个整数 `k` 。

一个元素 `x` 在数组中的 __频率__ 指的是它在数组中的出现次数。

如果一个数组中所有元素的频率都 __小于等于__ `k` ，那么我们称这个数组是 __好__ 数组。

请你返回 `nums` 中 __最长好__ 子数组的长度。

子数组 指的是一个数组中一段连续非空的元素序列。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,1,2,3,1,2], k = 2
输出：6
解释：最长好子数组是 [1,2,3,1,2,3] ，值 1 ，2 和 3 在子数组中的频率都没有超过 k = 2 。[2,3,1,2,3,1] 和 [3,1,2,3,1,2] 也是好子数组。
最长好子数组的长度为 6 。
```

示例 2：

```text
输入：nums = [1,2,1,2,1,2,1,2], k = 1
输出：2
解释：最长好子数组是 [1,2] ，值 1 和 2 在子数组中的频率都没有超过 k = 1 。[2,1] 也是好子数组。
最长好子数组的长度为 2 。
```

示例 3：

```text
输入：nums = [5,5,5,5,5,5,5], k = 4
输出：4
解释：最长好子数组是 [5,5,5,5] ，值 5 在子数组中的频率没有超过 k = 4 。
最长好子数组的长度为 4 。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`
- `1 <= k <= nums.length`

__思路:__

```text
滑动窗口
用哈希表记录每个元素的出现次数
如果窗口内的元素出现次数大于 k
那么必然是新引入的元素引起的
左边一直滑动到移除这个元素为止
记录最长的窗口长度即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxSubarrayLength(vector<int>& nums, int k) 
    {
        int result = 0, left = 0, n = nums.size();
        unordered_map<int, int> m;
        for (int right = 0; right < n; right++) 
        {
            ++m[nums[right]];
            while (m[nums[right]] > k) --m[nums[left++]];
            result = max(result, right - left + 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSubarrayLength(int[] nums, int k) {
        int result = 0, left = 0, n = nums.length;
        var map = new HashMap<Integer, Integer>();
        for (int right = 0; right < n; right++) {
            map.merge(nums[right], 1, Integer::sum);
            while (map.get(nums[right]) > k) map.merge(nums[left++], -1, Integer::sum);
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSubarrayLength(self, nums: List[int], k: int) -> int:
        result, left, d = 0, 0, defaultdict(int)
        for right, num in enumerate(nums):
            d[num] += 1
            while d[num] > k:
                d[nums[left]] -= 1
                left += 1
            result = max(result, right - left + 1)
        return result
```
