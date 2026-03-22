# 1223 Dice Roll Simulation 掷骰子模拟

__Description:__

A die simulator generates a random number from 1 to 6 for each roll. You introduced a constraint to the generator such that it cannot roll the number i more than rollMax[i] (1-indexed) consecutive times.

Given an array of integers rollMax and an integer n, return the number of distinct sequences that can be obtained with exact n rolls. Since the answer may be too large, return it modulo 109 + 7.

Two sequences are considered different if at least one element differs from each other.

__Example:__

Example 1:

Input: n = 2, rollMax = [1,1,2,2,2,3]
Output: 34
Explanation: There will be 2 rolls of die, if there are no constraints on the die, there are 6 * 6 = 36 possible combinations. In this case, looking at rollMax array, the numbers 1 and 2 appear at most once consecutively, therefore sequences (1,1) and (2,2) cannot occur, so the final answer is 36-2 = 34.

Example 2:

Input: n = 2, rollMax = [1,1,1,1,1,1]
Output: 30

Example 3:

Input: n = 3, rollMax = [1,1,1,2,2,3]
Output: 181

__Constraints:__

1 <= n <= 5000
rollMax.length == 6
1 <= rollMax[i] <= 15

__题目描述：__

有一个骰子模拟器会每次投掷的时候生成一个 1 到 6 的随机数。

不过我们在使用它时有个约束，就是使得投掷骰子时，连续 掷出数字 i 的次数不能超过 rollMax[i]（i 从 1 开始编号）。

现在，给你一个整数数组 rollMax 和一个整数 n，请你来计算掷 n 次骰子可得到的不同点数序列的数量。

假如两个序列中至少存在一个元素不同，就认为这两个序列是不同的。由于答案可能很大，所以请返回 模 10^9 + 7 之后的结果。

__示例：__

示例 1：

输入：n = 2, rollMax = [1,1,2,2,2,3]
输出：34
解释：我们掷 2 次骰子，如果没有约束的话，共有 6 * 6 = 36 种可能的组合。但是根据 rollMax 数组，数字 1 和 2 最多连续出现一次，所以不会出现序列 (1,1) 和 (2,2)。因此，最终答案是 36-2 = 34。

示例 2：

输入：n = 2, rollMax = [1,1,1,1,1,1]
输出：30

示例 3：

输入：n = 3, rollMax = [1,1,1,2,2,3]
输出：181

__提示：__

1 <= n <= 5000
rollMax.length == 6
1 <= rollMax[i] <= 15

__思路：__

动态规划
设 dp[k][i] 表示第 k 次投骰子为 i 的次数, dp[k][0] 为第 k 次投骰子所有次数之和
如果第 k - 1 次没有投出 i, 则第 k 次一定是第一次投出 i, 所以可以先将非 i 的 k - 1 次结果直接加到第 k 次, 即 dp[k][i] = dp[k - 1][0] - dp[k - 1][i]
若第 k - 1 次投出 i, 如果 k <= rollMax[i], 说明投出的次数 k 还没有达到 rollMax[i], 满足题目要求, 即 dp[k][i] += dp[k - 1][i]
否则说明一定有不满足题意的情况, 比如 rollMax[1] = 4, k = 5, 那么连续的 11111 就不满足题意, 并且在计算第 4 次时一定没有将连续的 1 算进去, 否则在之前的一次就已经超过了连续的最大次数, 所以 第 4 次以 1 结尾的结果只有 2, 3, 4, 5, 6 几种情况, 所以需要将 dp[k - rollMax[i] - 1][0] 减去并加上 dp[k - rollMax[i] - 1][i], 也就是说将 k - rollMax[i] - 1 次中的除了 i 的情况都减去, 类似滑动窗口, 注意到当 k == rollMax[i] 实际上只需要减去 1 种情况, 就是全 i 的情况, 单独讨论
时间复杂度为 O(n), 空间复杂度为 O(n), 骰子 6 个面的计算为 O(1)

__代码__:

__C++__:

```C++
class Solution 
{
private:
    static constexpr long MOD = 1e9 + 7;
public:
    int dieSimulator(int n, vector<int>& rollMax) 
    {
        vector<vector<long>> dp(n, vector<long>(6));
        fill(dp.front().begin(), dp.front().end(), 1);
        for (int i = 1; i < n; i++)
        {
            for (int j = 0; j < 6; j++)
            {
                dp[i][j] = accumulate(dp[i - 1].begin(), dp[i - 1].end(), MOD) % MOD;
                if (i == rollMax[j]) --dp[i][j];
                else if (i > rollMax[j]) for (int k = 0; k < 6; k++) if (j != k) dp[i][j] = (dp[i][j] - dp[i - rollMax[j] - 1][k] + MOD) % MOD;
            }
        }
        return accumulate(dp.back().begin(), dp.back().end(), MOD) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    private static int MOD = 1_000_000_007;
    
    public int dieSimulator(int n, int[] rollMax) {
        int[][] dp = new int[n][6];
        Arrays.fill(dp[0], 1);
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < 6; j++) {
                dp[i][j] = Arrays.stream(dp[i - 1]).reduce(0, (a, b) -> (a + b) % MOD);
                if (i == rollMax[j]) --dp[i][j];
                else if (i > rollMax[j]) for (int k = 0; k < 6; k++) if (j != k) dp[i][j] = (dp[i][j] - dp[i - rollMax[j] - 1][k] + MOD) % MOD;
            }
        }
        return Arrays.stream(dp[n - 1]).reduce(0, (a, b) -> (a + b) % MOD);
    }
}
```

__Python__:

```Python
class Solution:
    def dieSimulator(self, n: int, rollMax: List[int]) -> int:
        dp, mod = [[0] * 7 for _ in range(n + 1)], 10 ** 9 + 7
        dp[1][0], dp[0][0] = 6, 1
        for i in range(1, 7):
            dp[1][i] = 1
        for i in range(2, n + 1):
            for j in range(1, 7):
                dp[i][j] += dp[i - 1][0] - dp[i - 1][j] * ((cur := rollMax[j - 1]) < i) + (dp[i - 1][j] - dp[max(i - cur - 1, 0)][0] + dp[max(i - cur - 1, 0)][j]) * (cur < i and cur > 1)
                dp[i][0] += dp[i][j]
        return dp[n][0] % mod
```
