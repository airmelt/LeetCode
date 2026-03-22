# 1269 Number of Ways to Stay in the Same Place After Some Steps 停在原地的方案数

__Description:__

You have a pointer at index 0 in an array of size arrLen. At each step, you can move 1 position to the left, 1 position to the right in the array, or stay in the same place (The pointer should not be placed outside the array at any time).

Given two integers steps and arrLen, return the number of ways such that your pointer still at index 0 after exactly steps steps. Since the answer may be too large, return it modulo 109 + 7.

__Example:__

Example 1:

Input: steps = 3, arrLen = 2
Output: 4
Explanation: There are 4 differents ways to stay at index 0 after 3 steps.
Right, Left, Stay
Stay, Right, Left
Right, Stay, Left
Stay, Stay, Stay

Example 2:

Input: steps = 2, arrLen = 4
Output: 2
Explanation: There are 2 differents ways to stay at index 0 after 2 steps
Right, Left
Stay, Stay

Example 3:

Input: steps = 4, arrLen = 2
Output: 8

__Constraints:__

1 <= steps <= 500
1 <= arrLen <= 10^6

__题目描述：__

有一个长度为 arrLen 的数组，开始有一个指针在索引 0 处。

每一步操作中，你可以将指针向左或向右移动 1 步，或者停在原地（指针不能被移动到数组范围外）。

给你两个整数 steps 和 arrLen ，请你计算并返回：在恰好执行 steps 次操作以后，指针仍然指向索引 0 处的方案数。

由于答案可能会很大，请返回方案数 模 10^9 + 7 后的结果。

__示例：__

示例 1：

输入：steps = 3, arrLen = 2
输出：4
解释：3 步后，总共有 4 种不同的方法可以停在索引 0 处。
向右，向左，不动
不动，向右，向左
向右，不动，向左
不动，不动，不动

示例  2：

输入：steps = 2, arrLen = 4
输出：2
解释：2 步后，总共有 2 种不同的方法可以停在索引 0 处。
向右，向左
不动，不动

示例 3：

输入：steps = 4, arrLen = 2
输出：8

__提示：__

1 <= steps <= 500
1 <= arrLen <= 10^6

__思路：__

动态规划
设 dp[i][j] 表示第 i 步时移动到位置 j 的方案数
初始化 dp[0][0] = 1
dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1] + dp[i - 1][j + 1], 其中 0 < j < arrLen, 边界单独讨论
注意到 dp[i][j] 只与 dp[i - 1][j] 有关, 可以将空间复杂度压缩至 O(m)
由于需要返回 0, 所以可以进一步将空间复杂度压缩至 min((n >> 1) + 1, m)
最多能到达 steps >> 1 位置处
时间复杂度为 O(mn), 空间复杂度为 O(min(m, n)), 其中 n 为 steps, m 为 arrLen

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int numWays(int steps, int arrLen) 
    {
        int col = min((steps >> 1), arrLen - 1), mod = 1e9 + 7;
        vector<int> dp(col + 1);
        dp.front() = 1;
        for (int i = 0; i < steps; i++) 
        {
            vector<int> cur(col + 1);
            for (int j = 0; j <= col; j++) cur[j] = (((dp[j] + (j ? dp[j - 1] : 0)) % mod + (j < col ? dp[j + 1] : 0)) % mod);
            dp = cur;
        }
        return dp.front() % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int numWays(int steps, int arrLen) {
        int col = Math.min((steps >> 1), arrLen - 1), dp[] = new int[col + 1], mod = 1_000_000_007;
        dp[0] = 1;
        for (int i = 0; i < steps; i++) {
            int[] cur = new int[col + 1];
            for (int j = 0; j <= col; j++) cur[j] = (((dp[j] + (j > 0 ? dp[j - 1] : 0)) % mod + (j < col ? dp[j + 1] : 0)) % mod);
            dp = cur;
        }
        return dp[0] % mod;
    }
}
```

__Python__:

```Python
class Solution:
    def numWays(self, steps: int, arrLen: int) -> int:
        dp = [1] + [0] * (col := min(arrLen - 1, (steps >> 1)))
        for i in range(steps):
            dp = [dp[j] + (dp[j - 1] if j else 0) + (dp[j + 1] if j < col else 0) for j in range(col + 1)]
        return dp[0] % (10 ** 9 + 7)
```
