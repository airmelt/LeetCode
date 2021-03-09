# 494 Target Sum 目标和

__Description__:
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

__Example:__

Example 1:

Input: nums is [1, 1, 1, 1, 1], S is 3.
Output: 5
Explanation:

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.

__Constraints:__

The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.

__题目描述__:
给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

__示例 :__

输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。

__提示:__

数组非空，且长度不会超过 20 。
初始的数组的和不会超过 1000 。
保证返回的最终结果能被 32 位整数存下。

__思路__:

```text
S = sum(P) + sum(N), 其中 P表示正子集, N表示负子集
sum(nums) = sum(P) - sum(N)
S + sum(P) - sum(N) = sum(P) + sum(N) + sum(P) - sum(N)
S + sum(nums) = 2 * sum(P)
```

转化为 01背包问题, 即求 nums中有多少个 sum(P) = (S + sum(nums)) / 2
动态规划
dp[i] = j表示 nums的前 i个数中有 dp[i]种组合满足 sum(nums[:i]) = j的数量
dp[i] = dp[i] + dp[i - nums[j]]
时间复杂度O(n * sum), 空间复杂度O(sum), 其中 sum表示 nums中所有数的和

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findTargetSumWays(vector<int>& nums, int S) 
    {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum < S or ((sum + S) & 1)) return 0;
        int w = ((sum + S) >> 1);
        vector<int> dp(w + 1);
        dp[0] = 1;
        for (int num : nums) for (int i = w; i >= num; i--) dp[i] += dp[i - num];
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum < S || ((sum + S) & 1) != 0) return 0;
        int w = ((sum + S) >>> 1);
        int[] dp = new int[w + 1];
        dp[0] = 1;
        for (int num : nums) for (int i = w; i >= num; i--) dp[i] += dp[i - num];
        return dp[w];
    }
}
```

__Python__:

```Python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        if (_sum := sum(nums)) < S or (_sum + S) & 1:
            return 0
        dp = [1] + [0] * (w := (_sum + S) >> 1)
        for num in nums:
            for j in range(w, num - 1, -1):
                dp[j] += dp[j - num]
        return dp[w]
```
