# 600 Non-negative Integers without Consecutive Ones 不含连续1的非负整数

__Description__:
Given a positive integer n, return the number of the integers in the range [0, n] whose binary representations do not contain consecutive ones.

__Example:__

Example 1:

Input: n = 5
Output: 5
Explanation:
Here are the non-negative integers <= 5 with their corresponding binary representations:

```text
0 : 0
1 : 1
2 : 10
3 : 11
4 : 100
5 : 101
```

Among them, only integer 3 disobeys the rule (two consecutive ones) and the other 5 satisfy the rule.

Example 2:

Input: n = 1
Output: 2

Example 3:

Input: n = 2
Output: 3

__Constraints:__

1 <= n <= 10^9

__题目描述__:

给定一个正整数 n，找出小于或等于 n 的非负整数中，其二进制表示不包含 连续的1 的个数。

__示例 :__

示例 1:

输入: 5
输出: 5
解释:
下面是带有相应二进制表示的非负整数<= 5：

```text
0 : 0
1 : 1
2 : 10
3 : 11
4 : 100
5 : 101
```

其中，只有整数3违反规则（有两个连续的1），其他5个满足规则。
说明: 1 <= n <= 10^9

__思路__:

动态规划
dp[i] 表示 i 位二进制中有 dp[i] 个不含连续 1 的二进制数
比如 dp[2] = 2, 表示 10 和 01 这两个二进制数满足不含连续 1
动态转移方程 dp[i] = dp[i - 1] + dp[i - 2], 因为对第 i 位, 可由 i - 1 位在高位加 0 得到, 也可以由 i - 2 位在高位加 10 得到
现在求 n = 11, 即 n = 1011
每次用一个 mask 取当前位
对前 1 位 1
mask = 1, n & mask > 0, n & (mask >> 1) == 0, result = dp[1], result = 1
对前 2 位 11
mask = 2, n & mask > 0, n & (mask >> 1) > 0, result = dp[2], result = 3
对前 4 位 1011
mask = 8, n & mask = 1, n & (mask >> 1) = 0, result += dp[4], result = 8
然后就可以跳出循环
这样就判断完了 1-10 中的所有数字, 还剩下 11 没有判断
因为如果有连续的 1 时, n & (n - 1) > 0, 最后处理一下就可以了
时间复杂度 O(1), 空间复杂度 O(1), 最多需要 64 次循环, 需要一个长度为 32 的辅助数组

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findIntegers(int n) 
    {
        vector<int> dp(32);
        dp[0] = 1;
        dp[1] = 2;
        for (int i = 2; i < 32; i++) dp[i] = dp[i - 1] + dp[i - 2];
        int result = !(n & (n >> 1));
        for (int i = 1, mask = 1; mask <= n and i < 32; i++, mask <<= 1)
        {
            if (mask & n)
            {
                if ((mask >> 1) & n) result = dp[i];
                else result += dp[i - 1];
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findIntegers(int n) {
        int dp[] = new int[32], result = ((n & (n >> 1)) > 0) ? 0 : 1;
        dp[0] = 1;
        dp[1] = 2;
        for (int i = 2; i < 32; i++) dp[i] = dp[i - 1] + dp[i - 2];
        for (int i = 1, mask = 1; mask <= n && i < 32; i++, mask <<= 1) {
            if ((mask & n) > 0) {
                if (((mask >> 1) & n) > 0) result = dp[i];
                else result += dp[i - 1];
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findIntegers(self, n: int) -> int:
        dp, result, mask = [1, 2] + [0] * 30, not (n >> 1 & n), 1
        for i in range(2, 32):
            dp[i] = dp[i - 1] + dp[i - 2]
        for i in range(1, 32):
            if mask > n:
                break
            if (mask & n):
                if ((mask >> 1) & n):
                    result = dp[i]
                else:
                    result += dp[i - 1]
            mask <<= 1
        return result
```
