# 1745 Palindrome Partitioning IV 分割回文串 IV

__Description:__

Given a string `s`, return `true` _if it is possible to split the string_ `s` _into three __non-empty__ palindromic substrings. Otherwise, return_ `false`.​​​​​

A string is said to be palindrome if it the same string when reversed.

__Example:__

Example 1:

```text
Input: s = "abcbdd"
Output: true
Explanation: "abcbdd" = "a" + "bcb" + "dd", and all three substrings are palindromes.
```

Example 2:

```text
Input: s = "bcbddxy"
Output: false
Explanation: s cannot be split into 3 palindromes.
```

__Constraints:__

- `3 <= s.length <= 2000`
- `s`​​​​​​ consists only of lowercase English letters.

__题目描述:__

给你一个字符串 `s` ，如果可以将它分割成三个 __非空__ 回文子字符串，那么返回 `true` ，否则返回 `false` 。

当一个字符串正着读和反着读是一模一样的，就称其为 回文字符串 。

__示例:__

示例 1：

```text
输入：s = "abcbdd"
输出：true
解释："abcbdd" = "a" + "bcb" + "dd"，三个子字符串都是回文的。
```

示例 2：

```text
输入：s = "bcbddxy"
输出：false
解释：s 没办法被分割成 3 个回文子字符串。
```

__提示：__

- `3 <= s.length <= 2000`
- `s`​​​​​​ 只包含小写英文字母。

__思路:__

```text
动态规划
dp[i][j] 表示 s[i:j + 1] 是否为回文串
当子串长度为 1 时, s[i:i + 1] 必为回文串
当子串长度为 2 时, 需要比较 s[i] 是否等于 s[i + 1]
当子串长度大于 2 时, 需要比较 s[i] 是否等于 s[j] 并且 s[i + 1:j] 为回文串
最后需要在 [0, i - 1], [i, j - 1], [j, n - 1] 中找到两个分割点使得三个子串都为回文串
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool checkPartitioning(string s) 
    {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 1; i <= n; i++) for (int j = 0; j + i - 1 < n; j++) if (s[j] == s[j + i - 1] and (i < 3 or dp[j + 1][i + j - 2])) dp[j][i + j - 1] = 1;
        for (int i = 1; i < n - 1; i++) for (int j = i; j < n - 1; j++) if (dp[0][i - 1] and dp[i][j] and dp[j + 1][n - 1]) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean checkPartitioning(String s) {
        int n = s.length(), dp[] = new int[n + 1];
        dp[0] = 0x01;
        for (int i = 0; i < n; i++) {
            for (int l : new int[] {i - 1, i}) {
                int r = i;
                while (l > -1 && r < n && s.charAt(l) == s.charAt(r)) dp[r++ + 1] |= dp[l--] << 1;
            }
        }
        return (dp[n] & 0x08) == 0x08;
    }
}
```

__Python__:

```Python
class Solution:
    def checkPartitioning(self, s: str) -> bool:
        n = len(s)
        dp = [[False] * n for _ in range(n)]
        for i in range(1, n + 1):
            for j in range(n - i + 1):
                dp[j][i + j - 1] = s[j] == s[i + j - 1] and (i < 3 or dp[j + 1][i + j - 2])
        return any(dp[0][i - 1] and dp[i][j] and dp[j + 1][n - 1] for i in range(1, n - 1) for j in range(i, n - 1))
```
