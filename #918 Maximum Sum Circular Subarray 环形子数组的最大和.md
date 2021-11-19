# 918 Maximum Sum Circular Subarray 环形子数组的最大和

__Description__:
Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray of nums.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of nums[i] is nums[(i + 1) % n] and the previous element of nums[i] is nums[(i - 1 + n) % n].

A subarray may only include each element of the fixed buffer nums at most once. Formally, for a subarray nums[i], nums[i + 1], ..., nums[j], there does not exist i <= k1, k2 <= j with k1 % n == k2 % n.

__Example:__

Example 1:

Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3

Example 2:

Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10

Example 3:

Input: nums = [3,-1,2,-1]
Output: 4
Explanation: Subarray [2,-1,3] has maximum sum 2 + (-1) + 3 = 4

Example 4:

Input: nums = [3,-2,2,-3]
Output: 3
Explanation: Subarray [3] and [3,-2,2] both have maximum sum 3

Example 5:

Input: nums = [-2,-3,-1]
Output: -1
Explanation: Subarray [-1] has maximum sum -1

__Constraints:__

n == nums.length
1 <= n <= 3 \* 10^4
-3 \* 10^4 <= nums[i] <= 3 \* 10^4

__题目描述__:
给定一个由整数数组 A 表示的环形数组 C，求 C 的非空子数组的最大可能和。

在此处，环形数组意味着数组的末端将会与开头相连呈环状。（形式上，当0 <= i < A.length 时 C[i] = A[i]，且当 i >= 0 时 C[i+A.length] = C[i]）

此外，子数组最多只能包含固定缓冲区 A 中的每个元素一次。（形式上，对于子数组 C[i], C[i+1], ..., C[j]，不存在 i <= k1, k2 <= j 其中 k1 % A.length = k2 % A.length）

__示例 :__

示例 1：

输入：[1,-2,3,-2]
输出：3
解释：从子数组 [3] 得到最大和 3

示例 2：

输入：[5,-3,5]
输出：10
解释：从子数组 [5,5] 得到最大和 5 + 5 = 10

示例 3：

输入：[3,-1,2,-1]
输出：4
解释：从子数组 [2,-1,3] 得到最大和 2 + (-1) + 3 = 4

示例 4：

输入：[3,-2,2,-3]
输出：3
解释：从子数组 [3] 和 [3,-2,2] 都可以得到最大和 3

示例 5：

输入：[-2,-3,-1]
输出：-1
解释：从子数组 [-1] 得到最大和 -1

__提示:__

-30000 <= A[i] <= 30000
1 <= A.length <= 30000

__思路__:

参考[LeetCode #53 Maximum Subarray 最大子序和](https://www.jianshu.com/p/8cac126c5d0d)
遍历数组的同时, 记录数组当前的最大值/最小值和子序和的最大值/最小值, 以及数组的和
如果数组中的值都是负的则直接返回数组的最大值
否则返回子序和的最大值数组和与子序和的最小值的差中较大的那个
上述两个值分别对应最大子序和及跨越边界的最大子序和
时间复杂度为 O(n), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxSubarraySumCircular(vector<int>& nums) 
    {
        int n = nums.size(), cur_max = nums.front(), max_value = nums.front(), cur_min = nums.front(), min_value = nums.front(), s = accumulate(nums.begin(), nums.end(), 0);
        for (int i = 1; i < n; i++) 
        {
            cur_max = cur_max > 0 ? cur_max + nums[i] : nums[i];
            max_value = max(max_value, cur_max);
            cur_min = cur_min < 0 ? cur_min + nums[i] : nums[i];
            min_value = min(min_value, cur_min);
        }
        return max_value < 0 ? max_value : max(max_value, s - min_value);
    }
};
```

__Java__:

```Java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int n = nums.length, curMax = nums[0], maxValue = nums[0], curMin = nums[0], minValue = nums[0], s = nums[0];
        for (int i = 1; i < n; i++) {
            s += nums[i];
            curMax = curMax > 0 ? curMax + nums[i] : nums[i];
            maxValue = Math.max(maxValue, curMax);
            curMin = curMin < 0 ? curMin + nums[i] : nums[i];
            minValue = Math.min(minValue, curMin);
        }
        return maxValue < 0 ? maxValue : Math.max(maxValue, s - minValue);
    }
}
```

__Python__:

```Python
class Solution:
    def maxSubarraySumCircular(self, nums: List[int]) -> int:
        cur_max = max_value = cur_min = min_value = s = nums[0]
        for i in range(1, len(nums)):
            s += nums[i]
            max_value, min_value = max((cur_max := cur_max + nums[i] if cur_max > 0 else nums[i], max_value)), min((cur_min := cur_min + nums[i] if cur_min < 0 else nums[i]), min_value)
        return max_value if max_value < 0 else max(max_value, s - min_value)
```
