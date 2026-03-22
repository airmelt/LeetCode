# 2060 Check if an Original String Exists Given Two Encoded Strings 同源字符串检测

__Description:__

An original string, consisting of lowercase English letters, can be encoded by the following steps:

- Arbitrarily __split__ it into a __sequence__ of some number of __non-empty__ substrings.
- Arbitrarily choose some elements (possibly none) of the sequence, and __replace__ each with __its length__ (as a numeric string).
- __Concatenate__ the sequence as the encoded string.

For example, __one way__ to encode an original string `"abcdefghijklmnop"` might be:

- Split it as a sequence: `["ab", "cdefghijklmn", "o", "p"]`.
- Choose the second and third elements to be replaced by their lengths, respectively. The sequence becomes `["ab", "12", "1", "p"]`.
- Concatenate the elements of the sequence to get the encoded string: `"ab121p"`.

Given two encoded strings `s1` and `s2`, consisting of lowercase English letters and digits `1-9` (inclusive), return `true` _if there exists an original string that could be encoded as __both___ `s1` _and_ `s2`_. Otherwise, return_ `false`.

__Note__: The test cases are generated such that the number of consecutive digits in `s1` and `s2` does not exceed `3`.

__Example:__

Example 1:

```text
Input: s1 = "internationalization", s2 = "i18n"
Output: true
Explanation: It is possible that "internationalization" was the original string.
```

- "internationalization"
  -> Split:       ["internationalization"]
  -> Do not replace any element
  -> Concatenate:  "internationalization", which is s1.
- "internationalization"
  -> Split:       ["i", "nternationalizatio", "n"]
  -> Replace:     ["i", "18",                 "n"]
  -> Concatenate:  "i18n", which is s2

Example 2:

```text
Input: s1 = "l123e", s2 = "44"
Output: true
Explanation: It is possible that "leetcode" was the original string.
```

- "leetcode"
  -> Split:      ["l", "e", "et", "cod", "e"]
  -> Replace:    ["l", "1", "2",  "3",   "e"]
  -> Concatenate: "l123e", which is s1.
- "leetcode"
  -> Split:      ["leet", "code"]
  -> Replace:    ["4",    "4"]
  -> Concatenate: "44", which is s2.

Example 3:

```text
Input: s1 = "a5b", s2 = "c5b"
Output: false
Explanation: It is impossible.
```

- The original string encoded as s1 must start with the letter 'a'.
- The original string encoded as s2 must start with the letter 'c'.

__Constraints:__

- `1 <= s1.length, s2.length <= 40`
- `s1` and `s2` consist of digits `1-9` (inclusive), and lowercase English letters only.
- The number of consecutive digits in `s1` and `s2` does not exceed `3`.

__题目描述:__

原字符串由小写字母组成，可以按下述步骤编码：

- 任意将其 __分割__ 为由若干 __非空__ 子字符串组成的一个 __序列__ 。
- 任意选择序列中的一些元素（也可能不选择），然后将这些元素替换为元素各自的长度（作为一个数字型的字符串）。
- 重新 __顺次连接__ 序列，得到编码后的字符串。

例如，编码 `"abcdefghijklmnop"` 的一种方法可以描述为:

- 将原字符串分割得到一个序列: `["ab", "cdefghijklmn", "o", "p"]` 。
- 选出其中第二个和第三个元素并分别替换为它们自身的长度。序列变为 `["ab", "12", "1", "p"]` 。
- 重新顺次连接序列中的元素，得到编码后的字符串: `"ab121p"` 。

给你两个编码后的字符串 `s1` 和 `s2` ，由小写英文字母和数字 `1-9` 组成。如果存在能够同时编码得到 `s1` 和 `s2` 原字符串，返回 `true` ；否则，返回 `false`。

__注意:__生成的测试用例满足 `s1` 和 `s2` 中连续数字数不超过 `3` 。

__示例:__

示例 1：

```text
输入：s1 = "internationalization", s2 = "i18n"
输出：true
解释："internationalization" 可以作为原字符串
```

- "internationalization"
  -> 分割：      ["internationalization"]
  -> 不替换任何元素
  -> 连接：      "internationalization"，得到 s1
- "internationalization"
  -> 分割：      ["i", "nternationalizatio", "n"]
  -> 替换：      ["i", "18",                 "n"]
  -> 连接：      "i18n"，得到 s2

示例 2：

```text
输入：s1 = "l123e", s2 = "44"
输出：true
解释："leetcode" 可以作为原字符串
```

- "leetcode"
  -> 分割：       ["l", "e", "et", "cod", "e"]
  -> 替换：       ["l", "1", "2",  "3",   "e"]
  -> 连接：       "l123e"，得到 s1
- "leetcode"
  -> 分割：       ["leet", "code"]
  -> 替换：       ["4",    "4"]
  -> 连接：       "44"，得到 s2

示例 3：

```text
输入：s1 = "a5b", s2 = "c5b"
输出：false
解释：不存在这样的原字符串
```

- 编码为 s1 的字符串必须以字母 'a' 开头
- 编码为 s2 的字符串必须以字母 'c' 开头

示例 4：

```text
输入：s1 = "112s", s2 = "g841"
输出：true
解释："gaaaaaaaaaaaas" 可以作为原字符串
```

- "gaaaaaaaaaaaas"
  -> 分割：       ["g", "aaaaaaaaaaaa", "s"]
  -> 替换：       ["1", "12",           "s"]
  -> 连接：       "112s"，得到 s1
- "gaaaaaaaaaaaas"
  -> 分割：       ["g", "aaaaaaaa", "aaaa", "s"]
  -> 替换：       ["g", "8",        "4",    "1"]
  -> 连接         "g841"，得到 s2

示例 5：

```text
输入：s1 = "ab", s2 = "a2"
输出：false
解释：不存在这样的原字符串
```

- 编码为 s1 的字符串由两个字母组成
- 编码为 s2 的字符串由三个字母组成

__提示：__

- `1 <= s1.length, s2.length <= 40`
- `s1` 和 `s2` 仅由数字 `1-9` 和小写英文字母组成
- `s1` 和 `s2` 中连续数字数不超过 `3`

__思路:__

```text
动态规划
dp[i][j][d] 表示 s1[i:] 和 s2[j:] 是否可以匹配，其中 d 表示 s1[i] 和 s2[j] 之间的数字差
若 d > 0, 表示 s1[i] 需要匹配 s2[j] 的数字，d 为数字的长度
若 d < 0, 表示 s2[j] 需要匹配 s1[i] 的数字，d 为数字的长度
若 d = 0, 表示需要 s1[i] 和 s2[j], 或者将 s1[i] 和 s2[j] 的数字展开匹配
第一种情况 i == m, j == n, d == 0, 说明两个字符串完全匹配
第二种情况 d == 0, i < m, j < n, s1[i] == s2[j], 递归匹配下一个字符
第三种情况 d > 0, i < m, s1[i] 为字母，递归匹配下一个字符
第四种情况 d < 0, j < n, s2[j] 为字母，递归匹配下一个字符
第五种情况 d >= 0, s1 和 s2 的数字展开匹配
第六种情况 d <= 0, s1 和 s2 的数字展开匹配
时间复杂度为 O(MND * 10 ^ D), 空间复杂度为 O(MN * 10 ^ D), 其中 M 为 s1 的长度，N 为 s2 的长度，D 为连续数字的长度, 本题中 D = 3
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool possiblyEquals(string s1, string s2) 
    {
        int m = s1.size(), n = s2.size(), memo[m + 1][n + 1][2005];
        memset(memo, -1, sizeof(memo));
        function<int(int, int, int)> f = [&](int i, int j, int d) -> int 
        {
            if (memo[i][j][d + 1000] != -1) return memo[i][j][d + 1000];
            int result = (i == m and j == n and d == 0);
            result |= result or d == 0 and i < m and j < n and s1[i] == s2[j] and f(i + 1, j + 1, 0);
            result |= result or d > 0 and i < m and isalpha(s1[i]) and f(i + 1, j, d - 1);
            result |= result or d < 0 and j < n and isalpha(s2[j]) and f(i, j + 1, d + 1);
            if (!result and d >= 0) for (int p = i, v = 0; p < m and isdigit(s1[p]) and !result; p++) result |= f(p + 1, j, d - (v = v * 10 + s1[p] - '0'));
            if (!result and d <= 0) for (int p = j, v = 0; p < n and isdigit(s2[p]) and !result; p++) result |= f(i, p + 1, d + (v = v * 10 + s2[p] - '0'));
            return memo[i][j][d + 1000] = result;
        };
        return f(0, 0, 0);
    }
};
```

__Java__:

```Java
class Solution {
    private int m, n;
    private String s1, s2;

    public boolean possiblyEquals(String s1, String s2) {
        m = s1.length();
        n = s2.length();
        this.s1 = s1;
        this.s2 = s2;
        int memo[][][] = new int[m + 1][n + 1][2005];
        for (int i = 0; i <= m; i++) for (int j = 0; j <= n; j++) Arrays.fill(memo[i][j], -1);
        return f(0, 0, 0, memo);
    }

    private boolean f(int i, int j, int d, int[][][] memo) {
        if (memo[i][j][d + 1000] != -1) return memo[i][j][d + 1000] != 0;
        boolean result = i == m && j == n && d == 0;
        result |= result || d == 0 && i < m && j < n && s1.charAt(i) == s2.charAt(j) && f(i + 1, j + 1, 0, memo);
        result |= result || d > 0 && i < m && Character.isAlphabetic(s1.charAt(i)) && f(i + 1, j, d - 1, memo);
        result |= result || d < 0 && j < n && Character.isAlphabetic(s2.charAt(j)) && f(i, j + 1, d + 1, memo);
        if (!result && d >= 0) for (int p = i, v = 0; p < m && Character.isDigit(s1.charAt(p)) && !result; p++) result |= f(p + 1, j, d - (v = v * 10 + (s1.charAt(p) - '0')), memo);
        if (!result && d <= 0) for (int p = j, v = 0; p < n && Character.isDigit(s2.charAt(p)) && !result; p++) result |= f(i, p + 1, d + (v = v * 10 + (s2.charAt(p) - '0')), memo);
        return (memo[i][j][d + 1000] = (result ? 1 : 0)) != 0;
    }
}
```

__Python__:

```Python
class Solution:
    def possiblyEquals(self, s1: str, s2: str) -> bool:
        m, n = len(s1), len(s2)
        
        @lru_cache(None)
        def f(i: int, j: int, d: int) -> bool:
            result = i == m and j == n and d == 0
            result |= result or d == 0 and i < m and j < n and s1[i] == s2[j] and f(i + 1, j + 1, 0)
            result |= result or d > 0 and i < m and 'a' <= s1[i] <= 'z' and f(i + 1, j, d - 1)
            result |= result or d < 0 and j < n and 'a' <= s2[j] <= 'z' and f(i, j + 1, d + 1)
            if not result and d >= 0:
                v = 0
                for p in range(i, m):
                    if not '0' <= s1[p] <= '9' or result:
                        break
                    v = v * 10 + ord(s1[p]) - ord('0')
                    result |= f(p + 1, j, d - v)
            if not result and d <= 0:
                v = 0
                for p in range(j, n):
                    if not '0' <= s2[p] <= '9' or result:
                        break
                    v = v * 10 + ord(s2[p]) - ord('0')
                    result |= f(i, p + 1, d + v)
            return result
        return f(0, 0, 0)
```
