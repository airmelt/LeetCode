# 808 Soup Servings 分汤

__Description__:
There are two types of soup: type A and type B. Initially, we have n ml of each type of soup. There are four kinds of operations:

Serve 100 ml of soup A and 0 ml of soup B,
Serve 75 ml of soup A and 25 ml of soup B,
Serve 50 ml of soup A and 50 ml of soup B, and
Serve 25 ml of soup A and 75 ml of soup B.
When we serve some soup, we give it to someone, and we no longer have it. Each turn, we will choose from the four operations with an equal probability 0.25. If the remaining volume of soup is not enough to complete the operation, we will serve as much as possible. We stop once we no longer have some quantity of both types of soup.

Note that we do not have an operation where all 100 ml's of soup B are used first.

Return the probability that soup A will be empty first, plus half the probability that A and B become empty at the same time. Answers within 10-5 of the actual answer will be accepted.

__Example:__

Example 1:

Input: n = 50
Output: 0.62500
Explanation: If we choose the first two operations, A will become empty first.
For the third operation, A and B will become empty at the same time.
For the fourth operation, B will become empty first.
So the total probability of A becoming empty first plus half the probability that A and B become empty at the same time, is 0.25 * (1 + 1 + 0.5 + 0) = 0.625.

Example 2:

Input: n = 100
Output: 0.71875

__Constraints:__

0 <= n <= 10^9

__题目描述__:
有 A 和 B 两种类型的汤。一开始每种类型的汤有 N 毫升。有四种分配操作：

提供 100ml 的汤A 和 0ml 的汤B。
提供 75ml 的汤A 和 25ml 的汤B。
提供 50ml 的汤A 和 50ml 的汤B。
提供 25ml 的汤A 和 75ml 的汤B。
当我们把汤分配给某人之后，汤就没有了。每个回合，我们将从四种概率同为0.25的操作中进行分配选择。如果汤的剩余量不足以完成某次操作，我们将尽可能分配。当两种类型的汤都分配完时，停止操作。

注意不存在先分配100 ml汤B的操作。

需要返回的值： 汤A先分配完的概率 + 汤A和汤B同时分配完的概率 / 2。

__示例 :__

输入: N = 50
输出: 0.625
解释:
如果我们选择前两个操作，A将首先变为空。对于第三个操作，A和B会同时变为空。对于第四个操作，B将首先变为空。
所以A变为空的总概率加上A和B同时变为空的概率的一半是 0.25 *(1 + 1 + 0.5 + 0)= 0.625。

__注释:__

0 <= N <= 10^9。
返回值在 10^-6 的范围将被认为是正确的。

__思路__:

动态规划
题目中说了不足 25 的按 25 计算
所以可以将 n 直接除 25 向上取整
设 dp[i][j] 表示还剩 i 份 a 和 j 份 b 时的概率值
dp[i][j] = 0.25 * (dp[i - 4][j] + dp[i - 3][j - 1] + dp[i - 2][j - 2] + dp[i][j - 4])
边界条件
dp[i][j] = 0.5   if i <= 0 and j <= 0
dp[i][j] = 1.0   if i <= 0 and j > 0
dp[i][j] = 0.0   if i > 0 and j <= 0
因为 a 比 b 取的多, 所以 n 大于一定值的时候 a 比 b 概率上先取完
可以通过取一定的 n 值大概判断下 n 的最值只要概率超过 0.999999 就认为是 1.0
时间复杂度为 O(1), 空间复杂度为 O(1), n < 500

__代码__:
__C++__:

```C++
class Solution 
{
public:
    double soupServings(int n) 
    {
        n = n / 25 + !!(n % 25);
        if (n > 500) return 1.0;
        vector<vector<double>> dp(n + 1, vector<double>(n + 1));
        for (int i = 0; i <= (n << 1); i++)
        {
            for (int j = 0; j <= n; j++)
            {
                int k = i - j;
                if (k < 0 or k > n) continue;
                double result = 0.0;
                if (!j) result = 1.0;
                if (!j and !k) result = 0.5;
                if (j > 0 and k > 0) result = 0.25 * (dp[max(j - 4, 0)][k] + dp[max(j - 3, 0)][max(k - 1, 0)] + dp[max(j - 2, 0)][max(k - 2, 0)] + dp[max(j - 1, 0)][max(k - 3, 0)]);
                dp[j][k] = result;
            } 
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public double soupServings(int n) {
        n = n / 25 + (n % 25 == 0 ? 0 : 1);
        if (n > 500) return 1.0;
        double dp[][] = new double[n + 1][n + 1];
        for (int i = 0; i <= (n << 1); i++) {
            for (int j = 0; j <= n; j++) {
                int k = i - j;
                if (k < 0 || k > n) continue;
                double result = 0.0;
                if (j == 0) result = 1.0;
                if (j == 0 && k == 0) result = 0.5;
                if (j > 0 && k > 0) result = 0.25 * (dp[Math.max(j - 4, 0)][k] + dp[Math.max(j - 3, 0)][Math.max(k - 1, 0)] + dp[Math.max(j - 2, 0)][Math.max(k - 2, 0)] + dp[Math.max(j - 1, 0)][Math.max(k - 3, 0)]);
                dp[j][k] = result;
            } 
        }
        return dp[n][n];
    }
}
```

__Python__:

```Python
class Solution:
    def soupServings(self, n: int) -> float:
        n = n // 25 + (n % 25 != 0)
        if n >= 500:
            return 1.0
        dp = [[0.0] * (n + 1) for _ in range(n + 1)]
        for i in range((n << 1) + 1):
            for j in range(n + 1):
                k = i - j
                if k < 0 or k > n:
                    continue
                result = 0.0 if j else 1.0
                if not j and not k:
                    result = 0.5
                if j > 0 and k > 0:
                    result = 0.25 * (dp[max(j - 4, 0)][k] + dp[max(j - 3, 0)][max(k - 1, 0)] + dp[max(j - 2, 0)][max(k - 2, 0)] + dp[max(j - 1, 0)][max(k - 3, 0)]);
                dp[j][k] = result
        return dp[-1][-1]
```
