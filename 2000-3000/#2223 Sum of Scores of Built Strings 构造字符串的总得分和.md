# 2223 Sum of Scores of Built Strings 构造字符串的总得分和

__Description:__

You are __building__ a string `s` of length `n` __one__ character at a time, __prepending__ each new character to the __front__ of the string. The strings are labeled from `1` to `n`, where the string with length `i` is labeled `si`.

- For example, for `s = "abaca"`, `s1 == "a"`, `s2 == "ca"`, `s3 == "aca"`, etc.

The __score__ of `si` is the length of the __longest common prefix__ between `si` and `sn` (Note that `s == sn`).

Given the final string `s`, return _the __sum__ of the __score__ of every_ `si`.

__Example:__

Example 1:

```text
Input: s = "babab"
Output: 9
Explanation:
For s1 == "b", the longest common prefix is "b" which has a score of 1.
For s2 == "ab", there is no common prefix so the score is 0.
For s3 == "bab", the longest common prefix is "bab" which has a score of 3.
For s4 == "abab", there is no common prefix so the score is 0.
For s5 == "babab", the longest common prefix is "babab" which has a score of 5.
The sum of the scores is 1 + 0 + 3 + 0 + 5 = 9, so we return 9.
```

Example 2:

```text
Input: s = "azbazbzaz"
Output: 14
Explanation: 
For s2 == "az", the longest common prefix is "az" which has a score of 2.
For s6 == "azbzaz", the longest common prefix is "azb" which has a score of 3.
For s9 == "azbazbzaz", the longest common prefix is "azbazbzaz" which has a score of 9.
For all other si, the score is 0.
The sum of the scores is 2 + 3 + 9 = 14, so we return 14.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.

__题目描述:__

你需要从空字符串开始 __构造__ 一个长度为 `n` 的字符串 `s` ，构造的过程为每次给当前字符串 __前面__ 添加 __一个__ 字符。构造过程中得到的所有字符串编号为 `1` 到 `n` ，其中长度为 `i` 的字符串编号为 `si` 。

- 比方说， `s = "abaca"` ， `s1 == "a"` ， `s2 == "ca"` ， `s3 == "aca"` 依次类推。

`si` 的 __得分__ 为 `si` 和 `sn` 的 __最长公共前缀__ 的长度（注意 `s == sn` ）。

给你最终的字符串 `s` ，请你返回每一个 `si` 的 __得分之和__ 。

__示例:__

示例 1：

```text
输入：s = "babab"
输出：9
解释：
s1 == "b" ，最长公共前缀是 "b" ，得分为 1 。
s2 == "ab" ，没有公共前缀，得分为 0 。
s3 == "bab" ，最长公共前缀为 "bab" ，得分为 3 。
s4 == "abab" ，没有公共前缀，得分为 0 。
s5 == "babab" ，最长公共前缀为 "babab" ，得分为 5 。
得分和为 1 + 0 + 3 + 0 + 5 = 9 ，所以我们返回 9 。
```

示例 2 ：

```text
输入：s = "azbazbzaz"
输出：14
解释：
s2 == "az" ，最长公共前缀为 "az" ，得分为 2 。
s6 == "azbzaz" ，最长公共前缀为 "azb" ，得分为 3 。
s9 == "azbazbzaz" ，最长公共前缀为 "azbazbzaz" ，得分为 9 。
其他 si 得分均为 0 。
得分和为 2 + 3 + 9 = 14 ，所以我们返回 14 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
Z 函数 (扩展 KMP)
对于一个长度为 n 的字符串 s, 定义函数 z[i] 表示 s 和 s[i:] (即以 s[i] 开头的后缀) 的最长公共前缀 (LCP) 的长度, 则 z 被称为 s 的 Z 函数. 特别地, z[0] = 0
z[i] 的值满足 s[0:z[i]] == s[i:i + z[i]] 且 z[i] 最大
从 1 开始计算 s 的 Z 函数, 用 l 和 r 表示当前计算的 z[i] 的左右边界
定义 [i, i + z[i] - 1] ([l, r]) 为当前的最长匹配区间, 即 z-box
l <= i, 初始化 l = r = 0
z[i] = min(z[i - l], r - i + 1)
如果 s[z[i]] == s[i + z[i]], 则 z[i]++ 直到不满足条件
并将 l, r 更新, l = i, r = i + z[i] - 1
最后求出 z 的和加上 s 的长度即为答案
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long sumScores(string s) 
    {
        int n = s.size();
        vector<long long> z(n);
        for (int i = 1, l = 0, r = 0; i < n; i++) 
        {
            z[i] = max(min(z[i - l], r - i + 1LL), 0LL);
            while (i + z[i] < n and s[z[i]] == s[i + z[i]])
            {
                l = i;
                r = i + z[i]++;
            }
        }
        return accumulate(z.begin(), z.end(), 0LL) + n;
    }
};
```

__Java__:

```Java
class Solution {
    public long sumScores(String s) {
        int n = s.length();
        long[] z = new long[n];
        for (int i = 1, l = 0, r = 0; i < n; i++) {
            z[i] = Math.max(Math.min(z[i - l], r - i + 1L), 0L);
            while (i + z[i] < n && s.charAt((int)z[i]) == s.charAt(i + (int)z[i])) {
                l = i;
                r = i + (int)z[i]++;
            }
        }
        return Arrays.stream(z).sum() + n;
    }
}
```

__Python__:

```Python
class Solution:
    def sumScores(self, s: str) -> int:
        z, l, r = [0] * (n := len(s)), 0, 0
        for i in range(1, n):
            z[i] = max(min(z[i - l], r - i + 1), 0)
            while i + z[i] < n and s[z[i]] == s[i + z[i]]:
                l, r = i, i + z[i]
                z[i] += 1
        return n + sum(z)
```
