__Description__:
Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies:

Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

__Example:__
Example 1:

Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)

Example 2:

Input: [1,2,4,8]
Output: [1,2,4,8]

__题目描述__:
给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。

如果有多个目标子集，返回其中任何一个均可。

__示例 :__
示例 1:

输入: [1,2,3]
输出: [1,2] (当然, [1,3] 也正确)

示例 2:

输入: [1,2,4,8]
输出: [1,2,4,8]

__思路__:
动态规划
dp[i]表示到 0-i的 num[i]最大能整除的个数
比如[1, 2, 3, 4, 5, 6, 7, 8], i = 7, nums[i] = 8, dp[i] = 4(1, 2, 4, 8)
将 dp[i]初始化为 1
动态转移方程: 当 nums[i] % nums[j] == 0时, dp[i] = max(dp[i], dp[j] + 1)
将 nums排序
遍历 nums, 当整除时更新 dp数组
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) 
    {
        vector<int> result, dp(nums.size(), 1);
        int n = nums.size(), max_dp = 1, max_index = 0;
        if (nums.empty()) return result;
        sort(nums.begin(), nums.end());
        for (int i = 1; i < n; i++)
        {
            for (int j = i - 1; j > -1; j--) if (nums[i] % nums[j] == 0) dp[i] = max(dp[i], dp[j] + 1);
            if (dp[i] > max_dp)
            {
                max_dp = dp[i];
                max_index = i;
            }
        }
        for (int i = max_index; i > -1; i--)
        {
            if (nums[max_index] % nums[i] == 0 and dp[i] == max_dp)
            {
                max_index = i;
                result.emplace_back(nums[i]);
                --max_dp;
            }
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        List<Integer> result = new ArrayList<>();
        int n = nums.length, dp[] = new int[nums.length], maxDp = 1, maxIndex = 0;
        if (n == 0) return result;
        Arrays.fill(dp, 1);
        Arrays.sort(nums);
        for (int i = 1; i < n; i++) {
            for (int j = i - 1; j > -1; j--) if (nums[i] % nums[j] == 0) dp[i] = Math.max(dp[i], dp[j] + 1);
            if (dp[i] > maxDp) {
                maxDp = dp[i];
                maxIndex = i;
            }
        }
        for (int i = maxIndex; i > -1; i--) {
            if (nums[maxIndex] % nums[i] == 0 && dp[i] == maxDp) {
                maxIndex = i;
                result.add(0, nums[i]);
                --maxDp;
            }
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
        result = []
        if not nums:
            return result
        nums.sort()
        dp = [[i] for i in nums]
        for i in range(1, len(nums)):
            for j in range(i - 1, -1, -1):
                if not nums[i] % nums[j]:
                    dp[i] = max(dp[j] + [nums[i]], dp[i], key=len)
        return max(dp, key=len)
```