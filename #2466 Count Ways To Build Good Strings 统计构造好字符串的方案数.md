# 2466 Count Ways To Build Good Strings 统计构造好字符串的方案数

__Description:__

Given the integers `zero`, `one`, `low`, and `high`, we can construct a string by starting with an empty string, and then at each step perform either of the following:

- Append the character `'0'` `zero` times.
- Append the character `'1'` `one` times.

This can be performed any number of times.

A __good__ string is a string constructed by the above process having a __length__ between `low` and `high` (__inclusive__).

Return _the number of __different__ good strings that can be constructed satisfying these properties._ Since the answer can be large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: low = 3, high = 3, zero = 1, one = 1
Output: 8
Explanation: 
One possible valid good string is "011". 
It can be constructed as follows: "" -> "0" -> "01" -> "011". 
All binary strings from "000" to "111" are good strings in this example.
```

Example 2:

```text
Input: low = 2, high = 3, zero = 1, one = 2
Output: 5
Explanation: The good strings are "00", "11", "000", "110", and "011".
```

__Constraints:__

- `1 <= low <= high <= 10 ^ 5`
- `1 <= zero, one <= low`

__题目描述:__

给你整数 `zero` ， `one` ， `low` 和 `high` ，我们从空字符串开始构造一个字符串，每一步执行下面操作中的一种:

- 将 `'0'` 在字符串末尾添加 `zero` 次。
- 将 `'1'` 在字符串末尾添加 `one` 次。

以上操作可以执行任意次。

如果通过以上过程得到一个 __长度__ 在 `low` 和 `high` 之间（包含上下边界）的字符串，那么这个字符串我们称为 __好__ 字符串。

请你返回满足以上要求的 __不同__ 好字符串数目。由于答案可能很大，请将结果对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

示例 1：

```text
输入：low = 3, high = 3, zero = 1, one = 1
输出：8
解释：
一个可能的好字符串是 "011" 。
可以这样构造得到："" -> "0" -> "01" -> "011" 。
从 "000" 到 "111" 之间所有的二进制字符串都是好字符串。
```

示例 2：

```text
输入：low = 2, high = 3, zero = 1, one = 2
输出：5
解释：好字符串为 "00" ，"11" ，"000" ，"110" 和 "011" 。
```

__提示：__

- `1 <= low <= high <= 10 ^ 5`
- `1 <= zero, one <= low`

__思路:__

```text
动态规划
设 dp[i] 表示到第 i 个字符时的方案数
如果可以加 '0'，则 dp[i] += dp[i - zero], i >= zero
如果可以加 '1'，则 dp[i] += dp[i - one], i >= one
最后返回 dp[low] + dp[low + 1] + ... + dp[high]
时间复杂度为 O(N), 空间复杂度为 O(N), N 为 high
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countGoodStrings(int low, int high, int zero, int one) 
    {
        long long dp[high + 1], MOD = 1e9 + 7LL;
        memset(dp, 0LL, sizeof(dp));
        dp[0] = 1LL;
        for (int i = 1; i <= high; i++) 
        {
            if (i >= zero) dp[i] = dp[i - zero];
            if (i >= one) dp[i] = (dp[i] + dp[i - one]) % MOD;
        }
        return accumulate(dp + low, dp + high + 1, 0LL) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int countGoodStrings(int low, int high, int zero, int one) {
        int MOD = 1_000_000_007, dp[] = new int[high + 1], result = 0;
        dp[0] = 1;
        for (int i = 1; i <= high; i++) {
            if (i >= zero) dp[i] = dp[i - zero];
            if (i >= one) dp[i] = (dp[i] + dp[i - one]) % MOD;
        }
        for (int i = low; i <= high; i++) result = (result + dp[i]) % MOD;
        return result;
    }   
}
```

__Python__:

```Python
class Solution:
    def countGoodStrings(self, low: int, high: int, zero: int, one: int) -> int:
        dp, MOD = [1] + [0] * high, 10 ** 9 + 7
        for i in range(1, high + 1):
            if i >= zero:
                dp[i] = dp[i - zero]
            if i >= one:
                dp[i] = (dp[i] + dp[i - one]) % MOD
        return sum(dp[low:]) % MOD
```
