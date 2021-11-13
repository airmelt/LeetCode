# 910 Smallest Range II 最小差值 II

__Description__:
You are given an integer array nums and an integer k.

For each index i where 0 <= i < nums.length, change nums[i] to be either nums[i] + k or nums[i] - k.

The score of nums is the difference between the maximum and minimum elements in nums.

Return the minimum score of nums after changing the values at each index.

__Example:__

Example 1:

Input: nums = [1], k = 0
Output: 0
Explanation: The score is max(nums) - min(nums) = 1 - 1 = 0.

Example 2:

Input: nums = [0,10], k = 2
Output: 6
Explanation: Change nums to be [2, 8]. The score is max(nums) - min(nums) = 8 - 2 = 6.

Example 3:

Input: nums = [1,3,6], k = 3
Output: 3
Explanation: Change nums to be [4, 6, 3]. The score is max(nums) - min(nums) = 6 - 3 = 3.

__Constraints:__

1 <= nums.length <= 10^4
0 <= nums[i] <= 10^4
0 <= k <= 10^4

__题目描述__:
给你一个整数数组 A，对于每个整数 A[i]，可以选择 x = -K 或是 x = K （K 总是非负整数），并将 x 加到 A[i] 中。

在此过程之后，得到数组 B。

返回 B 的最大值和 B 的最小值之间可能存在的最小差值。

__示例 :__

示例 1：

输入：A = [1], K = 0
输出：0
解释：B = [1]

示例 2：

输入：A = [0,10], K = 2
输出：6
解释：B = [2,8]

示例 3：

输入：A = [1,3,6], K = 3
输出：3
解释：B = [4,6,3]

__提示:__

1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000

__思路__:

排序
将 nums 分成两个部分, 前一半需要加上 k, 后一半需要减去 k
取上述两个部分的最小差值即可
时间复杂度为 O(nlgn), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int smallestRangeII(vector<int>& nums, int k) 
    {
        sort(nums.begin(), nums.end());
        int n = nums.size(), result = nums.back() - nums.front();
        for (int i = 1; i < n; i++) result = min(result, max(nums[i - 1] + k, nums.back() - k) - min(nums.front() + k, nums[i] - k));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int smallestRangeII(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length, result = nums[n - 1] - nums[0];
        for (int i = 1; i < n; i++) result = Math.min(result, Math.max(nums[i - 1] + k, nums[n - 1] - k) - Math.min(nums[0] + k, nums[i] - k));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def smallestRangeII(self, nums: List[int], k: int) -> int:
        nums.sort()
        n, result = len(nums), nums[-1] - nums[0]
        for i in range(1, n):
            result = min(result, max(nums[i - 1] + k, nums[-1] - k) - min(nums[0] + k, nums[i] - k))
        return result
```
