# 279 Perfect Squares 完全平方数

__Description__:
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

__Example:__

Example 1:

Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.

Example 2:

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.

__题目描述__:
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

__示例 :__
示例 1:

输入: n = 12
输出: 3
解释: 12 = 4 + 4 + 4.

示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.

__思路__:

动态规划
dp[i]表示能用 i个平方数之和表示该数
令 s[i]表示小于 n的平方数
dp[i] = min(dp[i - s1], dp[i - s2], ..., dp[i - sn])
时间复杂度O(n ^ 3/2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numSquares(int n) 
    {
        vector<int> dp(n + 1, INT_MAX), sq;
        dp[0] = 0;
        for (int i = 1; i < (int)sqrt(n) + 2; i++) sq.push_back(i * i);
        for (int i = 1; i <= n; i++) for (auto s : sq)
        {
            if (i < s) break;
            dp[i] = min(dp[i - s] + 1, dp[i]);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numSquares(int n) {
        int dp[] = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        int sq[] = new int[(int)Math.sqrt(n) + 1];
        for (int i = 1; i < sq.length; ++i) sq[i] = i * i;
        for (int i = 1; i <= n; i++) for (int s = 1; s < sq.length; s++) {
            if (i < sq[s]) break;
            dp[i] = Math.min(dp[i], dp[i - sq[s]] + 1);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def numSquares(self, n: int) -> int:
        sq, dp = [i ** 2 for i in range(int(n ** 0.5) + 1)], [float('inf')] * (n + 1)
        dp[0] = 0
        for i in range(1, n + 1):
            for s in sq:
                if i < s:
                    break
                dp[i] = min(dp[i - s] + 1, dp[i])
        return dp[-1]
```
