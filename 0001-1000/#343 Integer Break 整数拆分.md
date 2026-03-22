# 343 Integer Break 整数拆分

__Description__:
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

__Example:__

Example 1:

Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.

Example 2:

Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
Note: You may assume that n is not less than 2 and not larger than 58.

__题目描述__:
给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

__示例 :__

示例 1:

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。

示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
说明: 你可以假设 n 不小于 2 且不大于 58。

__思路__:

1. 可以将这个整数拆分成最多的 3
可以用数学方法证明此时为最大值
时间复杂度O(1), 空间复杂度O(1)
2. 动态规划
设 dp[i]表示 i分解的最大乘积
动态转移方程 dp[i] = max(dp[i - j] \* (i - j), j \* (i - j)), j = 1 ~ i - 1
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int integerBreak(int n) 
    {
        vector<int> dp(n + 1, 1);
        for (int i = 2; i < n + 1; i++) for (int j = i - 1; j > 0; j--) dp[i] = max({dp[i], dp[j] * (i - j), j * (i - j)});
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int integerBreak(int n) {
        if (n <= 3) return n - 1;
        if (n % 3 == 0) return (int) Math.pow(3, n / 3);
        else if (n % 3 == 1) return (int) Math.pow(3, n / 3 - 1) * 4;
        else return (int) Math.pow(3, n / 3) * 2;
    }
}
```

__Python__:

```Python
class Solution:
    def integerBreak(self, n: int) -> int:
        return [0, 0, 1, 2, 4, 6, 9, 12, 18, 27, 36, 54, 81, 108, 162, 243, 324, 486, 729, 972, 1458, 2187, 2916, 4374, 6561, 8748, 13122, 19683, 26244, 39366, 59049, 78732, 118098, 177147, 236196, 354294, 531441, 708588, 1062882, 1594323, 2125764, 3188646, 4782969, 6377292, 9565938, 14348907, 19131876, 28697814, 43046721, 57395628, 86093442, 129140163, 172186884, 258280326, 387420489, 516560652, 774840978, 1162261467, 1549681956, 2324522934][n]
```
