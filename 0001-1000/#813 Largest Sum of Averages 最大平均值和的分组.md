# 813 Largest Sum of Averages 最大平均值和的分组

__Description__:
You are given an integer array nums and an integer k. You can partition the array into at most k non-empty adjacent subarrays. The score of a partition is the sum of the averages of each subarray.

Note that the partition must use every integer in nums, and that the score is not necessarily an integer.

Return the maximum score you can achieve of all the possible partitions. Answers within 10-6 of the actual answer will be accepted.

__Example:__

Example 1:

Input: nums = [9,1,2,3,9], k = 3
Output: 20.00000
Explanation:
The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned nums into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.

Example 2:

Input: nums = [1,2,3,4,5,6,7], k = 4
Output: 20.50000

__Constraints:__

1 <= nums.length <= 100
1 <= nums[i] <= 10^4
1 <= k <= nums.length

__题目描述__:
我们将给定的数组 A 分成 K 个相邻的非空子数组 ，我们的分数由每个子数组内的平均值的总和构成。计算我们所能得到的最大分数是多少。

注意我们必须使用 A 数组中的每一个数进行分组，并且分数不一定需要是整数。

__示例 :__

输入:
A = [9,1,2,3,9]
K = 3
输出: 20
解释:
A 的最优分组是[9], [1, 2, 3], [9]. 得到的分数是 9 + (1 + 2 + 3) / 3 + 9 = 20.
我们也可以把 A 分成[9, 1], [2], [3, 9].
这样的分组得到的分数为 5 + 2 + 6 = 13, 但不是最大值.

__说明:__

1 <= A.length <= 100.
1 <= A[i] <= 10000.
1 <= K <= A.length.
答案误差在 10^-6 内被视为是正确的。

__思路__:

动态规划
用前缀和记录数组的前缀和方便快速计算
设 dp[i][k] 表示前 i 个元素构成 k 个数组的最大平均值, 对 0 <= j <= k - 1
dp[i][k] = max(dp[i][k], dp[j][k - 1] + (nums[j + 1] + nums[j + 2] + ... + nums[i]) / (i - j))
即 dp[i][k] = max(dp[i][k] + avg(sum(nums[j + 1:i + 1]))
由于 dp[i][k] 只与 k - 1 有关, 可以将空间复杂度降低至 O(n)
时间复杂度为 O(kn ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double largestSumOfAverages(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<double> s(n + 1, 0.0), dp(n, 0.0);
        for (int i = 0; i < n; i++) s[i + 1] = s[i] + nums[i];
        for (int i = 0; i < n; ++i) dp[i] = (s[n] - s[i]) / (n - i);
        while (--k > 0) for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) dp[i] = max(dp[i], (s[j] - s[i]) / (j - i) + dp[j]);
        return dp.front();
    }
};
```

__Java__:

```Java
class Solution {
    public double largestSumOfAverages(int[] nums, int k) {
        double dp[][] = new double[nums.length + 1][k + 1], s[] = new double[nums.length + 1];
        for (int i = 1; i <= nums.length; i++) {
            s[i] = s[i - 1] + nums[i - 1];
            dp[i][1] = s[i] / i;
        }
        for (int i = 1; i <= nums.length; i++) for (int p = 2; p <= k; p++) for(int q = 0; q < i; q++) dp[i][p] = Math.max(dp[i][p], dp[q][p - 1] + (s[i] - s[q]) / (i - q));
        return dp[nums.length][k];
    }
}
```

__Python__:

```Python
class Solution:
    def largestSumOfAverages(self, nums: List[int], k: int) -> float:
        dp, s = [[0] * ((n := len(nums)) + 1) for _ in range(k + 1)], [0] * (n + 1)
        for i in range(1, n + 1):
            s[i] = s[i - 1] + nums[i - 1]
            dp[1][i] = s[i] / i
        for i in range(1, n + 1):
            for p in range(2, k + 1):
                for q in range(i):
                    dp[p][i] = max(dp[p][i], dp[p - 1][q] + (s[i] - s[q]) / (i - q))
        return dp[-1][-1]
```
