# 1977 Number of Ways to Separate Numbers 划分数字的方案数

__Description:__

You wrote down many __positive__ integers in a string called `num`. However, you realized that you forgot to add commas to seperate the different numbers. You remember that the list of integers was __non-decreasing__ and that __no__ integer had leading zeros.

Return _the __number of possible lists of integers__ that you could have written down to get the string_ `num`. Since the answer may be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: num = "327"
Output: 2
Explanation: You could have written down the numbers:
3, 27
327
```

Example 2:

```text
Input: num = "094"
Output: 0
Explanation: No numbers can have leading zeros and all numbers must be positive.
```

Example 3:

```text
Input: num = "0"
Output: 0
Explanation: No numbers can have leading zeros and all numbers must be positive.
```

__Constraints:__

- `1 <= num.length <= 3500`
- `num` consists of digits `'0'` through `'9'`.

__题目描述:__

你写下了若干 __正整数__ ，并将它们连接成了一个字符串 `num` 。但是你忘记给这些数字之间加逗号了。你只记得这一列数字是 __非递减__ 的且 __没有__ 任何数字有前导 0 。

请你返回有多少种可能的 __正整数数组__ 可以得到字符串 `num` 。由于答案可能很大，将结果对 `10 ^ 9 + 7` _取余_ 后返回。

__示例:__

示例 1：

```text
输入：num = "327"
输出：2
解释：以下为可能的方案：
3, 27
327
```

示例 2：

```text
输入：num = "094"
输出：0
解释：不能有数字有前导 0 ，且所有数字均为正数。
```

示例 3：

```text
输入：num = "0"
输出：0
解释：不能有数字有前导 0 ，且所有数字均为正数。
```

示例 4：

```text
输入：num = "9999999999999"
输出：101
```

__提示：__

- `1 <= num.length <= 3500`
- `num` 只含有数字 `'0'` 到 `'9'` 。

__思路:__

```text
动态规划
先排除特殊情况, 即 num[0] == '0', 直接返回 0
dp[i][j] 表示以 num[i] 结尾的长度为 j 的数字的方案数
对于上一个结尾应该为 dp[i - j][k]
注意到两个数位数大的一定更大
因此 k < j 可以直接加到 dp[i][j] 中, 这部分可以使用前缀和优化
当 k == j 时, 需要判断两个数的大小
设 lcp[i][j] 表示两个子串分别从 i 和 j 开始, 且第一个子串不大于第二个子串的最长长度
lcp[i][j] 可以由 lcp[i + 1][j + 1] 得到
比较时只需要比较 lcp[i - 2 * j][i - j] >= j 即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfCombinations(string num) 
    {
        if (num.front() == '0') return 0;
        int n = num.size(), MOD = 1e9 + 7;
        vector<vector<long long>> dp(n + 1, vector<long long>(n + 1));
        dp[1] = vector<long long>(n + 1, 1);
        dp[1][0] = 0L;
        vector<vector<int>> lcp(n, vector<int>(n));
        for (int i = 1; i <= n; i++) for (int j = n - i - 1; j > -1; j--) lcp[j][i + j] = num[j] < num[i + j] ? n - i - j : (num[j] > num[i + j] ? 0 : (i + j == n - 1 ? 1 : lcp[j + 1][i + j + 1] + 1));
        for (int i = 2; i <= n; i++) 
        {
            dp[i][i] = 1L;
            for (int j = 1; j < i; j++) if (num[i - j] != '0') dp[i][j] = (dp[i][j] + (i - (j << 1) >= 0 && lcp[i - (j << 1)][i - j] >= j ? dp[i - j][j] : dp[i - j][j - 1])) % MOD;
            for (int j = 1; j <= n; j++) dp[i][j] = (dp[i][j] + dp[i][j - 1]) % MOD;
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfCombinations(String num) {
        if (num.charAt(0) == '0') return 0;
        int n = num.length(), lcp[][] = new int[n][n];
        long dp[][] = new long[n + 1][n + 1], MOD = 1_000_000_007L;
        Arrays.fill(dp[1], 1L);
        dp[1][0] = 0L;
        for (int i = 1; i <= n; i++) for (int j = n - i - 1; j > -1; j--) lcp[j][i + j] = num.charAt(j) < num.charAt(i + j) ? n - i - j : (num.charAt(j) > num.charAt(i + j) ? 0 : (i + j == n - 1 ? 1 : lcp[j + 1][i + j + 1] + 1));
        for (int i = 2; i <= n; i++) {
            dp[i][i] = 1L;
            for (int j = 1; j < i; j++) if (num.charAt(i - j) != '0') dp[i][j] = (dp[i][j] + (i - (j << 1) >= 0 && lcp[i - (j << 1)][i - j] >= j ? dp[i - j][j] : dp[i - j][j - 1])) % MOD;
            for (int j = 1; j <= n; j++) dp[i][j] = (dp[i][j] + dp[i][j - 1]) % MOD;
        }
        return (int)dp[n][n];
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfCombinations(self, num: str) -> int:
        if num[0] == '0':
            return 0
        n, MOD = len(num), 10 ** 9 + 7
        dp, lcp = [[0] * (n + 1) for _ in range(n + 1)], [[0] * n for _ in range(n)]
        dp[1] = [0] + [1] * n
        for i in range(1, n + 1):
            for j in range(n - 1 - i, -1, -1):
                lcp[j][i + j] = n - j - i if num[j] < num[i + j] else 1 if num[j] == num[i + j] and i + j == n - 1 else lcp[j + 1][i + j + 1] + 1 if num[j] == num[i + j] else 0
        for i in range(2, n + 1):
            dp[i][i] = 1
            for j in range(1, i):
                if num[i - j] != '0':
                    dp[i][j] += dp[i - j][j] if i - (j << 1) >= 0 and lcp[i - (j << 1)][i - j] >= j else dp[i - j][j - 1]
            for j in range(1, n + 1):
                dp[i][j] = (dp[i][j] + dp[i][j - 1]) % MOD
        return dp[-1][-1] % MOD
```
