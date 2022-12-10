# 1416 Restore The Array 恢复数组

__Description:__

A program was supposed to print an array of integers. The program forgot to print whitespaces and the array is printed as a string of digits `s` and all we know is that all integers in the array were in the range `[1, k]` and there are no leading zeros in the array.

Given the string `s` and the integer `k`, return _the number of the possible arrays that can be printed as_ `s` _using the mentioned program_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: s = "1000", k = 10000
Output: 1
Explanation: The only possible array is [1000]
```

Example 2:

```text
Input: s = "1000", k = 10
Output: 0
Explanation: There cannot be an array that was printed this way and has all integer >= 1 and <= 10.
```

Example 3:

```text
Input: s = "1317", k = 2000
Output: 8
Explanation: Possible arrays are [1317],[131,7],[13,17],[1,317],[13,1,7],[1,31,7],[1,3,17],[1,3,1,7]
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only digits and does not contain leading zeros.
- `1 <= k <= 10 ^ 9`

__题目描述:__

某个程序本来应该输出一个整数数组。但是这个程序忘记输出空格了以致输出了一个数字字符串，我们所知道的信息只有：数组中所有整数都在 `[1, k]` 之间，且数组中的数字都没有前导 0 。

给你字符串 `s` 和整数 `k` 。可能会有多种不同的数组恢复结果。

按照上述程序，请你返回所有可能输出字符串 `s` 的数组方案数。

由于数组方案数可能会很大，请你返回它对 `10 ^ 9 + 7` __取余__ 后的结果。

__示例:__

示例 1：

```text
输入：s = "1000", k = 10000
输出：1
解释：唯一一种可能的数组方案是 [1000]
```

示例 2：

```text
输入：s = "1000", k = 10
输出：0
解释：不存在任何数组方案满足所有整数都 >= 1 且 <= 10 同时输出结果为 s 。
```

示例 3：

```text
输入：s = "1317", k = 2000
输出：8
解释：可行的数组方案为 [1317]，[131,7]，[13,17]，[1,317]，[13,1,7]，[1,31,7]，[1,3,17]，[1,3,1,7]
```

示例 4：

```text
输入：s = "2020", k = 30
输出：1
解释：唯一可能的数组方案是 [20,20] 。 [2020] 不是可行的数组方案，原因是 2020 > 30 。 [2,020] 也不是可行的数组方案，因为 020 含有前导 0 。
```

示例 5：

```text
输入：s = "1234567890", k = 90
输出：34
```

__提示：__

- `1 <= s.length <= 10 ^ 5`.
- `s` 只包含数字且不包含前导 0 。
- `1 <= k <= 10 ^ 9`.

__思路:__

```text
动态规划
设 dp[i] 表示 s[:i] 的方案数
dp[i] = sum(dp[j]), 其中 1 <= j <= i, s[j:i] 不包含前导 0 且 int(s[j:i]) <= k
由于需要用到更大的数更新, 遍历 j 的时候需要采用倒序的方式
时间复杂度为 O(NlogK), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfArrays(string s, int k) 
    {
        int n = s.size(), mod = 1e9 + 7, length = to_string(k).size();
        vector<int> dp(n + 1);
        dp.front() = 1;
        for (int i = 1; i <= n; i++)
        {
            long long cur = 0, base = 1;
            for (int j = i, c = 1; j >= 1 and c <= length; c++, j--, base *= 10)
            {
                cur += (s[j - 1] - '0') * base;
                if (0 < cur and cur <= k and s[j - 1] != '0') dp[i] = (dp[i] + dp[j - 1]) % mod;
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfArrays(String s, int k) {
        int n = s.length(), mod = 1_000_000_007, length = String.valueOf(k).length(), dp[] = new int[n + 1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            long cur = 0, base = 1;
            for (int j = i, c = 1; j >= 1 && c <= length; c++, j--, base *= 10)
            {
                cur += (s.charAt(j - 1) - '0') * base;
                if (0 < cur && cur <= k && s.charAt(j - 1) != '0') dp[i] = (dp[i] + dp[j - 1]) % mod;
            }
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfArrays(self, s: str, k: int) -> int:
        s, n, max_value, cur, dp, base, mod = list(map(int, s)) + [1], len(s), 10 ** (m := len(str(k))), 0, deque([0] * 10 + [1]), 1, 10 ** 9 + 7
        for i in range(n):
            cur = cur * 10 + s[i] if i < m else cur * 10 + s[i] - max_value * s[i - m]
            base += (result := 0 if not s[i + 1] else base - dp[-m - (cur <= k)])
            dp.append(base) 
        return result % mod
```
