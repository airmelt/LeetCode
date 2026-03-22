# 689 Maximum Sum of 3 Non-Overlapping Subarrays 三个无重叠子数组的最大和

__Description__:
Given an integer array nums and an integer k, find three non-overlapping subarrays of length k with maximum sum and return them.

Return the result as a list of indices representing the starting position of each interval (0-indexed). If there are multiple answers, return the lexicographically smallest one.

__Example:__

Example 1:

Input: nums = [1,2,1,2,6,7,5,1], k = 2
Output: [0,3,5]
Explanation: Subarrays [1, 2], [2, 6], [7, 5] correspond to the starting indices [0, 3, 5].
We could have also taken [2, 1], but an answer of [1, 3, 5] would be lexicographically larger.

Example 2:

Input: nums = [1,2,1,2,1,2,1,2,1], k = 2
Output: [0,2,4]

__Constraints:__

1 <= nums.length <= 2 \* 10^4
1 <= nums[i] < 2^16
1 <= k <= floor(nums.length / 3)

__题目描述__:
给定数组 nums 由正整数组成，找到三个互不重叠的子数组的最大和。

每个子数组的长度为k，我们要使这3\*k个项的和最大化。

返回每个区间起始索引的列表（索引从 0 开始）。如果有多个结果，返回字典序最小的一个。

__示例 :__

输入: [1,2,1,2,6,7,5,1], 2
输出: [0, 3, 5]
解释: 子数组 [1, 2], [2, 6], [7, 5] 对应的起始索引为 [0, 3, 5]。
我们也可以取 [2, 1], 但是结果 [1, 3, 5] 在字典序上更大。

__注意:__

nums.length的范围在[1, 20000]之间。
nums[i]的范围在[1, 65535]之间。
k的范围在[1, floor(nums.length / 3)]之间。

__思路__:

动态规划
用一个 window 数组记录所有长度为 k 的子数组的和
用两个数组 left, right 记录 window[i] 的最大值对应的下标
其中 left 数组记录 [0, i] 中 window[i] 的最大值对应的下标
right 数组记录 [i, window.size() - 1] 的最大值对应的下标
最后再遍历所有的可能值, 找到中间下标 i 使得 window[left[i - k]] + window[i] + window[right[i + k]] 最大, 返回对应的 { left[i - k], k, right[i + k] } 数组
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) 
    {
        int n = nums.size(), cur = accumulate(nums.begin(), nums.begin() + k, 0);
        vector<int> window(n - k + 1, cur), left(n - k + 1), right(n - k + 1, n - k), result;
        for (int i = k; i < n; i++) window[i - k + 1] = window[i - k] + nums[i] - nums[i - k];
        for (int i = 1; i < n - k + 1; i++) 
        {
            if (window[i] > window[left[i - 1]]) left[i] = i;
            else left[i] = left[i - 1];
            if (window[n - k - i] >= window[right[n - k - i + 1]]) right[n - k - i] = n - k - i;
            else right[n - k - i] = right[n - k - i + 1];
        }
        cur = 0;
        for (int i = k; i < n - (k << 1) + 1; i++) 
        {
            if (cur < window[i] + window[left[i - k]] + window[right[i + k]]) 
            {
                cur = window[i] + window[left[i - k]] + window[right[i + k]];
                result = vector<int>{ left[i - k], i, right[i + k] };
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length, cur = 0, result[] = new int[3];
        int window[] = new int[n - k + 1], left[] = new int[n - k + 1], right[] = new int[n - k + 1];
        for (int i = 0; i < k; i++) cur += nums[i];
        Arrays.fill(window, cur);
        for (int i = k; i < n; i++) window[i - k + 1] = window[i - k] + nums[i] - nums[i - k];
        Arrays.fill(right, n - k);
        for (int i = 1; i < n - k + 1; i++) {
            if (window[i] > window[left[i - 1]]) left[i] = i;
            else left[i] = left[i - 1];
            if (window[n - k - i] >= window[right[n - k - i + 1]]) right[n - k - i] = n - k - i;
            else right[n - k - i] = right[n - k - i + 1];
        }
        cur = 0;
        for (int i = k; i < n - (k << 1) + 1; i++) {
            if (cur < window[i] + window[left[i - k]] + window[right[i + k]]) {
                cur = window[i] + window[left[i - k]] + window[right[i + k]];
                result = new int[]{ left[i - k], i, right[i + k] };
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxSumOfThreeSubarrays(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        window, left, right, cur, result = [sum(nums[:k])] * (n - k + 1), [0] * (n - k + 1), [n - k] * (n - k + 1), 0, [0] * 3
        for i in range(k, n):
            window[i - k + 1] = window[i - k] - nums[i - k] + nums[i]
        for i in range(1, n - k + 1):
            if window[i] > window[left[i - 1]]:
                left[i] = i
            else:
                left[i] = left[i - 1]
            if window[n - k - i] >= window[right[n - k - i + 1]]:
                right[n - k - i] = n - k - i
            else:
                right[n - k - i] = right[n - k - i + 1]
        for i in range(k, n - k + 1 - k):
            if cur < window[i] + window[left[i - k]] + window[right[i + k]]:
                cur = window[i] + window[left[i - k]] + window[right[i + k]]
                result = [left[i - k], i, right[i + k]]
        return result
```
