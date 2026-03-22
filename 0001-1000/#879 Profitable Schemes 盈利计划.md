# 879 Profitable Schemes 盈利计划

__Description__:
There is a group of n members, and a list of various crimes they could commit. The ith crime generates a profit[i] and requires group[i] members to participate in it. If a member participates in one crime, that member can't participate in another crime.

Let's call a profitable scheme any subset of these crimes that generates at least minProfit profit, and the total number of members participating in that subset of crimes is at most n.

Return the number of schemes that can be chosen. Since the answer may be very large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 5, minProfit = 3, group = [2,2], profit = [2,3]
Output: 2
Explanation: To make a profit of at least 3, the group could either commit crimes 0 and 1, or just crime 1.
In total, there are 2 schemes.

Example 2:

Input: n = 10, minProfit = 5, group = [2,3,5], profit = [6,7,8]
Output: 7
Explanation: To make a profit of at least 5, the group could commit any crimes, as long as they commit one.
There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).

__Constraints:__

1 <= n <= 100
0 <= minProfit <= 100
1 <= group.length <= 100
1 <= group[i] <= 100
profit.length == group.length
0 <= profit[i] <= 100

__题目描述__:
集团里有 n 名员工，他们可以完成各种各样的工作创造利润。

第 i 种工作会产生 profit[i] 的利润，它要求 group[i] 名成员共同参与。如果成员参与了其中一项工作，就不能参与另一项工作。

工作的任何至少产生 minProfit 利润的子集称为 盈利计划 。并且工作的成员总数最多为 n 。

有多少种计划可以选择？因为答案很大，所以 返回结果模 10^9 + 7 的值。

__示例 :__

示例 1：

输入：n = 5, minProfit = 3, group = [2,2], profit = [2,3]
输出：2
解释：至少产生 3 的利润，该集团可以完成工作 0 和工作 1 ，或仅完成工作 1 。
总的来说，有两种计划。

示例 2：

输入：n = 10, minProfit = 5, group = [2,3,5], profit = [6,7,8]
输出：7
解释：至少产生 5 的利润，只要完成其中一种工作就行，所以该集团可以完成任何工作。
有 7 种可能的计划：(0)，(1)，(2)，(0,1)，(0,2)，(1,2)，以及 (0,1,2) 。

__提示:__

1 <= n <= 100
0 <= minProfit <= 100
1 <= group.length <= 100
1 <= group[i] <= 100
profit.length == group.length
0 <= profit[i] <= 100

__思路__:

动态规划
记 dp[i][j][k] 表示使用前 i 个项目, 使用 k 个人, 产生利润为 j 的方案数
初始化 dp[0][0][k] = 1, 表示产生 0 的利润有 1 种方案
对于 dp[i + 1][j][k], 如果不选择第 i + 1 项, 那么就有 dp[i][j][k] 种方案
如果选择第 i + 1 项, 那么在保证人数足够的情况下 (k >= group[i]) 有 dp[i][max(0, j - profit[i])][k - group[i]] 种方案
上述两种方案都能实现所以是求和
注意到每次只与前一行有关, 可以用滚动数组的方式优化空间复杂度
时间复杂度为 O(pmn), 空间复杂度为 O(pn), 其中 p = minProfit, m 为 group/profit 的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int profitableSchemes(int n, int minProfit, vector<int>& group, vector<int>& profit) 
    {
        vector<vector<int>> dp(minProfit + 1, vector<int>(n + 1));
        for (int i = 0; i <= n; i++) dp.front()[i] = 1;
        for (int i = 0, m = group.size(), MOD = (int)1e9 + 7; i < m; i++) for (int j = minProfit; j > -1; j--) for (int k = n; k >= group[i]; k--) dp[j][k] = (dp[j][k] + dp[max(0, j - profit[i])][k - group[i]]) % MOD;
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int profitableSchemes(int n, int minProfit, int[] group, int[] profit) {
        int MOD = 1_000_000_007, m = group.length, dp[][][] = new int[m + 1][minProfit + 1][n + 1];
        for (int k = 0; k <= n; k++) dp[0][0][k] = 1;
        for (int i = 0; i < m; i++) for (int j = 0; j <= minProfit; j++) for (int k = 0; k <= n; k++) dp[i + 1][j][k] = (dp[i + 1][j][k] + dp[i][j][k] + (k >= group[i] ? dp[i][Math.max(0, j - profit[i])][k - group[i]] : 0)) % MOD;
        return dp[m][minProfit][n];
    }
}
```

__Python__:

```Python
class Solution:
    def profitableSchemes(self, n: int, minProfit: int, group: List[int], profit: List[int]) -> int:
        m = len(group)
        dp = [[[0] * (minProfit + 1) for _ in range(n + 1)] for _ in range(m + 1)]
        dp[0][0][0] = 1
        for i in range(1, m + 1):
            for j in range(n + 1):
                for k in range(minProfit + 1):
                    dp[i][j][k] = dp[i - 1][j][k] if j < (need := group[i - 1]) else dp[i - 1][j][k] + dp[i - 1][j - need][max(0, k - (p := profit[i - 1]))]
        return sum(dp[-1][j][-1] for j in range(n + 1)) % (10 ** 9 + 7)
```
