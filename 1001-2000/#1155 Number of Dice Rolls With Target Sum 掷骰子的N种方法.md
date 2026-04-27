# 1155 Number of Dice Rolls With Target Sum 掷骰子的N种方法

__Description__:
You have n dice and each die has k faces numbered from 1 to k.

Given three integers n, k, and target, return the number of possible ways (out of the kn total ways) to roll the dice so the sum of the face-up numbers equals target. Since the answer may be too large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 1, k = 6, target = 3
Output: 1
Explanation: You throw one die with 6 faces.
There is only one way to get a sum of 3.

Example 2:

Input: n = 2, k = 6, target = 7
Output: 6
Explanation: You throw two dice, each with 6 faces.
There are 6 ways to get a sum of 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.

Example 3:

Input: n = 30, k = 30, target = 500
Output: 222616187
Explanation: The answer must be returned modulo 10^9 + 7.

__Constraints:__

1 <= n, k <= 30
1 <= target <= 1000

__题目描述__:
这里有 n 个一样的骰子，每个骰子上都有 k 个面，分别标号为 1 到 k 。

给定三个整数 n ,  k 和 target ，返回可能的方式(从总共 kn 种方式中)滚动骰子的数量，使正面朝上的数字之和等于 target 。

答案可能很大，你需要对 10^9 + 7 取模 。

__示例 :__

示例 1：

输入：n = 1, k = 6, target = 3
输出：1
解释：你扔一个有6张脸的骰子。
得到3的和只有一种方法。

示例 2：

输入：n = 2, k = 6, target = 7
输出：6
解释：你扔两个骰子，每个骰子有6个面。
得到7的和有6种方法1+6 2+5 3+4 4+3 5+2 6+1。

示例 3：

输入：n = 30, k = 30, target = 500
输出：222616187
解释：返回的结果必须是对 10^9 + 7 取模。

__提示:__

1 <= n, k <= 30
1 <= target <= 1000

__思路__:

动态规划
设 dp[i][j] 表示扔 i 个骰子总和为 j 的方案数
dp[i][j] = ∑dp[i - 1][max(0, j - k)], 初始化 dp[0][0] = 1 表示 0 个骰子组成 0 个方案数为 1
注意到 dp 只与上一行有关
可以将 dp 优化为一维数组
dp[j] = ∑[max(0, j - k)], 每次遍历时需要将 dp[j] 置 0
时间复杂度为 O(nkt), 空间复杂度为 O(t)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numRollsToTarget(int n, int k, int target) 
    {
        const int mod = 1e9 + 7;
        vector<int> dp(target + 1);
        dp.front() = 1;
        for (int i = 0; i < n; i++) 
        {
            for (int j = target; j > -1; j--) 
            {
                dp[j] = 0;
                for (int m = 1, r = min(k, j); m <= r; m++) dp[j] = (dp[j] + dp[j - m]) % mod;
            }
        }
        return dp[target] % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int numRollsToTarget(int n, int k, int target) {
        int mod = 1_000_000_007, dp[] = new int[target + 1];
        dp[0] = 1;
        for (int i = 0; i < n; i++) {
            for (int j = target; j > -1; j--) {
                dp[j] = 0;
                for (int m = 1, r = Math.min(k, j); m <= r; m++) dp[j] = (dp[j] + dp[j - m]) % mod;
            }
        }
        return dp[target] % mod;
    }
}
```

__Python__:

```Python
class Solution:
    def numRollsToTarget(self, n: int, k: int, target: int) -> int:
        dp = [1] + [0] * target
        for i in range(n):
            for j in range(target, -1, -1):
                dp[j] = 0
                for m in range(1, min(k, j) + 1):
                    dp[j] += dp[j - m]
        return dp[target] % (10 ** 9 + 7)
```
