# 3026 Maximum Good Subarray Sum 最大好子数组和

__Description:__

You are given an array `nums` of length `n` and a __positive__ integer `k`.

A subarray of `nums` is called __good__ if the __absolute difference__ between its first and last element is __exactly__ `k`, in other words, the subarray `nums[i..j]` is good if `|nums[i] - nums[j]| == k`.

Return _the __maximum__ sum of a __good__ subarray of_ `nums`. _If there are no good subarrays_ _, return_ `0`.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4,5,6], k = 1
Output: 11
Explanation: The absolute difference between the first and last element must be 1 for a good subarray. All the good subarrays are: [1,2], [2,3], [3,4], [4,5], and [5,6]. The maximum subarray sum is 11 for the subarray [5,6].
```

Example 2:

```text
Input: nums = [-1,3,2,4,5], k = 3
Output: 11
Explanation: The absolute difference between the first and last element must be 3 for a good subarray. All the good subarrays are: [-1,3,2], and [2,4,5]. The maximum subarray sum is 11 for the subarray [2,4,5].
```

Example 3:

```text
Input: nums = [-1,-2,-3,-4], k = 2
Output: -6
Explanation: The absolute difference between the first and last element must be 2 for a good subarray. All the good subarrays are: [-1,-2,-3], and [-2,-3,-4]. The maximum subarray sum is -6 for the subarray [-1,-2,-3].
```

__Constraints:__

- `2 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__题目描述:__

给你一个长度为 `n` 的数组 `nums` 和一个 __正__ 整数 `k` 。

如果 `nums` 的一个子数组中，第一个元素和最后一个元素 __差的绝对值恰好__ 为 `k` ，我们称这个子数组为 __好__ 的。换句话说，如果子数组 `nums[i..j]` 满足 `|nums[i] - nums[j]| == k` ，那么它是一个好子数组。

请你返回 `nums` 中 __好__ 子数组的 __最大__ 和，如果没有好子数组，返回 `0` 。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4,5,6], k = 1
输出：11
解释：好子数组中第一个元素和最后一个元素的差的绝对值必须为 1 。好子数组有 [1,2] ，[2,3] ，[3,4] ，[4,5] 和 [5,6] 。最大子数组和为 11 ，对应的子数组为 [5,6] 。
```

示例 2：

```text
输入：nums = [-1,3,2,4,5], k = 3
输出：11
解释：好子数组中第一个元素和最后一个元素的差的绝对值必须为 3 。好子数组有 [-1,3,2] 和 [2,4,5] 。最大子数组和为 11 ，对应的子数组为 [2,4,5] 。
```

示例 3：

```text
输入：nums = [-1,-2,-3,-4], k = 2
输出：-6
解释：好子数组中第一个元素和最后一个元素的差的绝对值必须为 2 。好子数组有 [-1,-2,-3] 和 [-2,-3,-4] 。最大子数组和为 -6 ，对应的子数组为 [-1,-2,-3] 。
```

__提示：__

- `2 <= nums.length <= 10 ^ 5`
- `-10 ^ 9 <= nums[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
前缀和 ➕ 哈希表
将 |nums[i] - nums[j]| == k 化简得
nums[i] = nums[j] + k or nums[i] = nums[j] - k
所以如果找到 nums[j] + k 或者 nums[j] - k 就可以用前缀和
pre[i + 1] - pre[j] 得到子数组的和
为了求出 pre[j] 对应的最小值
可以使用哈希表记录当前元素 num 对应的前缀和的最小值
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
        unordered_map<int, long long> m;
        long long result = LLONG_MIN, s = 0LL;
        for (const auto& num : nums) 
        {
            auto it = m.find(num + k);
            if (it != m.end()) result = max(result, s + num - it -> second);
            it = m.find(num - k);
            if (it != m.end()) result = max(result, s + num - it -> second);
            it = m.find(num);
            if (it == m.end() or s < it -> second) m[num] = s;
            s += num;
        }
        return result == LLONG_MIN ? 0 : result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        var map = new HashMap<Integer, Long>();
        long s = 0L, result = Long.MIN_VALUE, inf = 0x3f3f3f3f3f3f3f3fL;
        for (int num : nums) {
            result = Math.max(result, s + num - Math.min(map.getOrDefault(num - k, inf), map.getOrDefault(num + k, inf)));
            map.merge(num, s, Math::min);
            s += num;
        }
        return result > -(inf >>> 1L) ? result : 0L;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        s, c, result = 0, defaultdict(lambda: inf), -inf
        for i, num in enumerate(nums):
            result, c[num] = max(result, s + num - min(c[num + k], c[num - k])), min(c[num], s)
            s += num
        return result if result > -inf else 0
```
