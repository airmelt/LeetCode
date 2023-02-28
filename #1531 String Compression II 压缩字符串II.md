# 1531 String Compression II 压缩字符串II

__Description:__

[Run-length encoding](http://en.wikipedia.org/wiki/Run-length_encoding) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string `"aabccc"` we replace  `"aa"` by  `"a2"` and replace  `"ccc"` by  `"c3"`. Thus the compressed string becomes  `"a2bc3"`.

Notice that in this problem, we are not adding `'1'` after single characters.

Given a string `s` and an integer `k`. You need to delete __at most__ `k` characters from `s` such that the run-length encoded version of `s` has minimum length.

Find the _minimum length of the run-length encoded version of_ `s` _after deleting at most_ `k` _characters_.

__Example:__

Example 1:

```text
Input: s = "aaabcccd", k = 2
Output: 4
Explanation: Compressing s without deleting anything will give us "a3bc3d" of length 6. Deleting any of the characters 'a' or 'c' would at most decrease the length of the compressed string to 5, for instance delete 2 'a' then we will have s = "abcccd" which compressed is abc3d. Therefore, the optimal way is to delete 'b' and 'd', then the compressed version of s will be "a3c3" of length 4.
```

Example 2:

```text
Input: s = "aabbaa", k = 2
Output: 2
Explanation: If we delete both 'b' characters, the resulting compressed string would be "a4" of length 2.
```

Example 3:

```text
Input: s = "aaaaaaaaaaa", k = 0
Output: 3
Explanation: Since k is zero, we cannot delete anything. The compressed string is "a11" of length 3.
```

__Constraints:__

- `1 <= s.length <= 100`
- `0 <= k <= s.length`
- `s` contains only lowercase English letters.

__题目描述:__

[行程长度编码](https://baike.baidu.com/item/%E8%A1%8C%E7%A8%8B%E9%95%BF%E5%BA%A6%E7%BC%96%E7%A0%81/2931940?fr=aladdin) 是一种常用的字符串压缩方法，它将连续的相同字符（重复 2 次或更多次）替换为字符和表示字符计数的数字（行程长度）。例如，用此方法压缩字符串 `"aabccc"` ，将 `"aa"` 替换为 `"a2"` ， `"ccc"` 替换为`"c3"` 。因此压缩后的字符串变为 `"a2bc3"` 。

注意，本问题中，压缩时没有在单个字符后附加计数 `'1'` 。

给你一个字符串 `s` 和一个整数 `k` 。你需要从字符串 `s` 中删除最多 `k` 个字符，以使 `s` 的行程长度编码长度最小。

请你返回删除最多 `k` 个字符后， `s` __行程长度编码的最小长度__ 。

__示例:__

示例 1：

```text
输入：s = "aaabcccd", k = 2
输出：4
解释：在不删除任何内容的情况下，压缩后的字符串是 "a3bc3d" ，长度为 6 。最优的方案是删除 'b' 和 'd'，这样一来，压缩后的字符串为 "a3c3" ，长度是 4 。
```

示例 2：

```text
输入：s = "aabbaa", k = 2
输出：2
解释：如果删去两个 'b' 字符，那么压缩后的字符串是长度为 2 的 "a4" 。
```

示例 3：

```text
输入：s = "aaaaaaaaaaa", k = 0
输出：3
解释：由于 k 等于 0 ，不能删去任何字符。压缩后的字符串是 "a11" ，长度为 3 。
```

__提示：__

- `1 <= s.length <= 100`
- `0 <= k <= s.length`
- `s` 仅包含小写英文字母

__思路:__

```text
动态规划
设 dp[i][j] 表示 s[0..i] 剩余可删除 j 个字符，压缩后的最小长度
dp[0] 全部初始化为 0 表示不删除任何字符最短长度为 0
删除最后一个字符, dp[i][j] = dp[i - 1][j - 1], j != 0
不删除最后一个字符, 从后往前依次尝试删除字符, dp[i][j] = min(dp[i][j], dp[p - 1][j - diff] + helper(count)), 其中 p 从 i 开始往前遍历, 记录出现的不等于 s[i - 1] 的字符数, 删除之后就能得到新的压缩字符串, helper 函数统计 count 的数目, 也就是 s[i - 1] 的个数
时间复杂度为 O(KN ^ 2), 空间复杂度为 O(KN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int getLengthOfOptimalCompression(string s, int k) 
    {
        int n = s.size(), INF = 0x3f3f3f3f;
        vector<vector<int>> dp(n + 1, vector<int>(k + 1, INF));
        dp.front().front() = 0;
        for (int i = 1; i <= n; i++) 
        {
            for (int j = 0; j <= min(k, i); j++) 
            {
                if (j != 0) dp[i][j] = dp[i - 1][j - 1];
                int count = 0, diff = 0;
                for (int p = i; p >= 1 and diff <= j; --p) 
                {
                    if (s[p - 1] == s[i - 1]) 
                    {
                        ++count;
                        dp[i][j] = min(dp[i][j], dp[p - 1][j - diff] + (count == 1 ? 1 : (count < 10 ? 2 : (count < 100 ? 3 : 4))));
                    } 
                    else ++diff;
                }
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int getLengthOfOptimalCompression(String s, int k) {
        int n = s.length(), dp[][] = new int[n + 1][k + 1], INF = 0x3f3f3f3f;
        for (int i = 1; i <= n; i++) Arrays.fill(dp[i], INF);
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= Math.min(k, i); j++) {
                if (j != 0) dp[i][j] = dp[i - 1][j - 1];
                int count = 0, diff = 0;
                for (int p = i; p >= 1 && diff <= j; --p) {
                    if (s.charAt(p - 1) == s.charAt(i - 1)) {
                        ++count;
                        dp[i][j] = Math.min(dp[i][j], dp[p - 1][j - diff] + (count == 1 ? 1 : (count < 10 ? 2 : (count < 100 ? 3 : 4))));
                    } else ++diff;
                }
            }
        }
        return dp[n][k];
    }
}
```

__Python__:

```Python
class Solution:
    def getLengthOfOptimalCompression(self, s: str, k: int) -> int:
        n = len(s)
        @functools.lru_cache(None)
        def dp(start: int, k: int) -> int:
            if start == n: 
                return 0
            result, c = dp(start + 1, k - 1) if k else inf, 1
            for i in range(start + 1, n + 1):
                cur = dp(i, k) + 1
                cur += 3 if c >= 100 else (2 if c >= 10 else (1 if c > 1 else 0))
                result = min(result, cur)
                if i == n: 
                    break
                if s[i] == s[start]:
                    c += 1
                else:
                    if not k: 
                        break
                    k -= 1
            return result
        return dp(0, k)
```
