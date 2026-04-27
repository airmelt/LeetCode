# 2767 Partition String Into Minimum Beautiful Substrings 将字符串分割为最少的美丽子字符串

__Description:__

Given a binary string `s`, partition the string into one or more __substrings__ such that each substring is __beautiful__.

A string is beautiful if:

- It doesn't contain leading zeros.
- It's the __binary__ representation of a number that is a power of `5`.

Return _the __minimum__ number of substrings in such partition._ If it is impossible to partition the string `s` into beautiful substrings, return `-1`.

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: s = "1011"
Output: 2
Explanation: We can partition the given string into ["101", "1"].
```

- The string "101" does not contain leading zeros and is the binary representation of integer 51 = 5.
- The string "1" does not contain leading zeros and is the binary representation of integer 50 = 1.

It can be shown that 2 is the minimum number of beautiful substrings that s can be partitioned into.

Example 2:

```text
Input: s = "111"
Output: 3
Explanation: We can partition the given string into ["1", "1", "1"].
```

- The string "1" does not contain leading zeros and is the binary representation of integer 50 = 1.

It can be shown that 3 is the minimum number of beautiful substrings that s can be partitioned into.

Example 3:

```text
Input: s = "0"
Output: -1
Explanation: We can not partition the given string into beautiful substrings.
```

__Constraints:__

- `1 <= s.length <= 15`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个二进制字符串 `s` ，你需要将字符串分割成一个或者多个 __子字符串__ ，使每个子字符串都是 __美丽__ 的。

如果一个字符串满足以下条件，我们称它是 美丽 的：

- 它不包含前导 0 。
- 它是 `5` 的幂的 __二进制__ 表示。

请你返回分割后的子字符串的 __最少__ 数目。如果无法将字符串 `s` 分割成美丽子字符串，请你返回 `-1` 。

子字符串是一个字符串中一段连续的字符序列。

__示例:__

示例 1：

```text
输入：s = "1011"
输出：2
解释：我们可以将输入字符串分成 ["101", "1"] 。
```

- 字符串 "101" 不包含前导 0 ，且它是整数 51 = 5 的二进制表示。
- 字符串 "1" 不包含前导 0 ，且它是整数 50 = 1 的二进制表示。

最少可以将 s 分成 2 个美丽子字符串。

示例 2：

```text
输入：s = "111"
输出：3
解释：我们可以将输入字符串分成 ["1", "1", "1"] 。
```

- 字符串 "1" 不包含前导 0 ，且它是整数 50 = 1 的二进制表示。

最少可以将 s 分成 3 个美丽子字符串。

示例 3：

```text
输入：s = "0"
输出：-1
解释：无法将给定字符串分成任何美丽子字符串。
```

__提示：__

- `1 <= s.length <= 15`
- `s[i]` 要么是 `'0'` 要么是 `'1'` 。

__思路:__

```text
动态规划
预处理长度为 15 以内的 5 的幂的二进制
或者记录最大的数为 15625
保证每一次遍历的数开头不能为 0
然后检查当前字符串是否为 5 的幂
如果是 dp[i] = dp[j] + 1
最后需要检查是否能将所有二进制转化为 5 的幂如果不行返回 -1
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumBeautifulSubstrings(string s) 
    {
        int max_five = 15625, n = s.size(), INF = 0x3f3f3f3f;
        vector<int> dp(n + 1, INF);
        dp.front() = 0;
        for (int i = 0; i <= n; i++) for (int j = i - 1; j > -1; j--) if (s.at(j) != '0' and !(max_five % stoi(s.substr(j, i - j), 0, 2))) dp[i] = min(dp[i], dp[j] + 1);
        return dp.back() == INF ? -1 : dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumBeautifulSubstrings(String s) {
        int maxFive = 15625, n = s.length(), dp[] = new int[n + 1], INF = 0x3f3f3f3f;
        Arrays.fill(dp, INF);
        dp[0] = 0;
        for (int i = 0; i <= n; i++) for (int j = i - 1; j > -1; j--) if (s.charAt(j) != '0' && maxFive % Integer.parseInt(s.substring(j, i), 2) == 0) dp[i] = Math.min(dp[i], dp[j] + 1);
        return dp[n] == INF ? -1 : dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumBeautifulSubstrings(self, s: str) -> int:
        fives, n = set([bin(5 ** i)[2:] for i in range(7)]), len(s)
        
        @lru_cache(None)
        def dfs(start: int) -> int:
            return 0 if start == n else inf if s[start] == '0' else min(dfs(start + len(five)) + 1 for five in fives if start + len(five) <= n and s[start:start + len(five)] == five)
        return result if (result := dfs(0)) < inf else -1
```
