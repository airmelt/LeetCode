# 312 Burst Balloons 戳气球

__Description__:
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] \* nums[i] \* nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

__Note:__

You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

__Example:__

Input: [3,1,5,8]
Output: 167
Explanation:

```text
nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

__题目描述__:
有 n 个气球，编号为0 到 n-1，每个气球上都标有一个数字，这些数字存在数组 nums 中。

现在要求你戳破所有的气球。如果你戳破气球 i ，就可以获得 nums[left] \* nums[i] \* nums[right] 个硬币。 这里的 left 和 right 代表和 i 相邻的两个气球的序号。注意当你戳破了气球 i 后，气球 left 和气球 right 就变成了相邻的气球。

求所能获得硬币的最大数量。

__说明:__

你可以假设 nums[-1] = nums[n] = 1，但注意它们不是真实存在的所以并不能被戳破。
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

__示例 :__

输入: [3,1,5,8]
输出: 167
解释:

```text
nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

__思路__:

```text
动态规划
这道题类似矩阵链乘法
首先按照题意, 可以给 nums数组的首尾加上一个 1, 用来表示端点值, 这两个气球不参与戳气球
定义 dp[i][j]表示戳破区间 (i, j)后能得到的最大得分
base case为 dp[i][j] = 0, 其中 i >= j
假定最后一次戳破的气球为 nums[k]
状态转移方程为: dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + nums[i] \* nums[k] \* nums[j])
上面这个表示, 最后一次如果戳破 nums[k], 则得分应该不变或者是 (i, k), (k, j)这两个区间的得分加上戳破气球的得分
时间复杂度O(n ^ 2), 空间复杂度O(n)
```

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maxCoins(vector<int>& nums) 
    {
        nums.emplace_back(1);
        nums.emplace(nums.begin(), 1);
        vector<vector<int>> dp(nums.size(), vector<int>(nums.size()));
        for (int i = nums.size() - 2; i > -1; i--) for (int j = i + 2; j < nums.size(); j++) for (int k = i + 1; k < j; k++) dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j]);
        return dp.front().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxCoins(int[] nums) {
        int points[] = new int[nums.length + 2], n = nums.length + 2, dp[][] = new int [nums.length + 2][nums.length + 2];
        for (int i = 1; i < n - 1; i++) points[i] = nums[i - 1];
        points[0] = points[n - 1] = 1;
        for (int i = n - 2; i > -1; i--) for (int j = i + 2; j < n; j++) for (int k = i + 1; k < j; k++) dp[i][j] = Math.max(dp[i][j], dp[i][k] + dp[k][j] + points[i] * points[k] * points[j]);
        return dp[0][n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums, dp = [1] + nums + [1], [[0] * (len(nums) + 2) for _ in range(len(nums) + 2)]
        for i in range(len(nums) - 1, -1, -1):
            for j in range(i + 2, len(nums)):
                for k in range(i + 1, j):
                    dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + nums[i] * nums[j] * nums[k])
        return dp[0][-1]
```
