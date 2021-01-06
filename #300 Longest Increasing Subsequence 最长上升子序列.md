# 300 Longest Increasing Subsequence 最长上升子序列

__Description__:
Given an unsorted array of integers, find the length of longest increasing subsequence.

__Example:__

Input: [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

__Note:__
There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n2) complexity.

__Follow up:__
Could you improve it to O(n log n) time complexity?

__题目描述__:
给定一个无序的整数数组，找到其中最长上升子序列的长度。

__示例 :__

输入: [10,9,2,5,3,7,101,18]
输出: 4
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。

__说明:__
可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。

__进阶：__
你能将算法的时间复杂度降低到 O(n log n) 吗?

__思路__:

1. 动态规划
设 dp[i]表示以 nums[i]结尾的最长子序列
遍历 nums[0] ~ nums[i], 如果 nums[i] > nums[j](j = 0, 1, 2, ..., i - 1), dp[i] = max(dp[i], dp[j] + 1)
时间复杂度O(n ^ 2), 空间复杂度O(n)
2. 动态规划 ➕ 二分查找
设 dp[i]表示当长度为 i + 1时, 以 nums中的某个数结尾的最小的上升子序列

例如 [4, 5, 6, 3],

- size == 1, [4], [5], [6], [3], 最小就是 dp[0] = 3
- size == 2, [4, 5], [5, 6], 最小的就是 dp[1] = 5
- size == 3, [4, 5, 6], 最小的就是 dp[2] = 6
返回 size即可, 注意 dp不是最长上升子序列, 只是一个临时数组
遍历一次 nums数组, 用二分查找找 num的位置, 如果 num的位置在 dp外, 则插入, 否则更新
最后得到的 dp数组中的元素一定是一个递增的数组
如 4(插入), 5(插入), 6(插入), 3(更新), 这时, dp中的元素刚好是 3, 5, 6
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lengthOfLIS(vector<int>& nums) 
    {
        vector<int> dp;
        for (auto& num : nums) 
        {
            auto i = lower_bound(dp.begin(), dp.end(), num) - dp.begin();
            if (i >= dp.size()) dp.emplace_back(num);
            else dp[i] = num;
        }
        return dp.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int dp[] = new int[nums.length], result = nums.length == 0 ? 0 : 1;
        Arrays.fill(dp, 1);
        for (int i = 1; i < nums.length; i++) for (int j = 0; j < i; j++) if (nums[i] > nums[j]) dp[i] = Math.max(dp[i], dp[j] + 1);
        for (int i : dp) result = Math.max(result, i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp, result = [1] * len(nums), 1 if nums else 0
        for i in range(1, len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i], result = max(dp[i], dp[j] + 1), max(result, dp[i], dp[j] + 1)
        return result
```
