# 357 Count Numbers with Unique Digits 计算各个位数不同的数字个数

 __Description__:
Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10^n.

__Example:__

Input: 2
Output: 91
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100,
             excluding 11,22,33,44,55,66,77,88,99

__Constraints:__

0 <= n <= 8

__题目描述__:
给定一个非负整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10^n 。

__示例 :__

输入: 2
输出: 91
解释: 答案应为除去 11,22,33,44,55,66,77,88,99 外，在 [0,100) 区间内的所有数字。

__思路__:

排列组合
dp[n]表示 n个不重复的数字
dp[0] = 1, dp[1] = 10
dp[i] = 9 \* 9 \* 8 \* ... \* (11 - i) + dp[i - 1], 因为 dp[i]第一位取 0, 即 dp[i - 1], 如果第一位不取 0, 对于第一位有 9种取法, 第二位有 9种(包括 0) ... 第 i位有 (11 - i)种取法
即 dp[i] = dp[i - 1] + (dp[i - 1] - dp[i - 2]) \* (11 - i)
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int countNumbersWithUniqueDigits(int n) 
    {
        vector<int> dp(n + 2, 1);
        dp[1] = 10;
        for (int i = 2; i < n + 1; i++) dp[i] = dp[i - 1] + (dp[i - 1] - dp[i - 2]) * (11 - i);
        return dp[n];
    }
};
```

__Java__:

```Java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        int dp[] = new int[n + 2];
        dp[0] = 1;
        dp[1] = 10;
        for (int i = 2; i < n + 1; i++) dp[i] = dp[i - 1] + (dp[i - 1] - dp[i - 2]) * (11 - i);
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        return [1, 10, 91, 739, 5275, 32491, 168571, 712891, 2345851, 5611771, 8877691][n]
```
