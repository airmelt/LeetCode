# 2430 Maximum Deletions on a String 对字母串可执行的最大删除数

__Description:__

You are given a string `s` consisting of only lowercase English letters. In one operation, you can:

- Delete __the entire string__ `s`, or
- Delete the __first__ `i` letters of `s` if the first `i` letters of `s` are __equal__ to the following `i` letters in `s`, for any `i` in the range `1 <= i <= s.length / 2`.

For example, if `s = "ababc"`, then in one operation, you could delete the first two letters of `s` to get `"abc"`, since the first two letters of `s` and the following two letters of `s` are both equal to `"ab"`.

Return _the __maximum__ number of operations needed to delete all of_ `s`.

__Example:__

Example 1:

```text
Input: s = "abcabcdabc"
Output: 2
Explanation:
```

- Delete the first 3 letters ("abc") since the next 3 letters are equal. Now, s = "abcdabc".
- Delete all the letters.

We used 2 operations so return 2. It can be proven that 2 is the maximum number of operations needed.
Note that in the second operation we cannot delete "abc" again because the next occurrence of "abc" does not happen in the next 3 letters.

Example 2:

```text
Input: s = "aaabaab"
Output: 4
Explanation:
```

- Delete the first letter ("a") since the next letter is equal. Now, s = "aabaab".
- Delete the first 3 letters ("aab") since the next 3 letters are equal. Now, s = "aab".
- Delete the first letter ("a") since the next letter is equal. Now, s = "ab".
- Delete all the letters.

We used 4 operations so return 4. It can be proven that 4 is the maximum number of operations needed.

Example 3:

```text
Input: s = "aaaaa"
Output: 5
Explanation: In each operation, we can delete the first letter of s.
```

__Constraints:__

- `1 <= s.length <= 4000`
- `s` consists only of lowercase English letters.

__题目描述:__

给你一个仅由小写英文字母组成的字符串 `s` 。在一步操作中，你可以:

- 删除 __整个字符串__ `s` ，或者
- 对于满足 `1 <= i <= s.length / 2` 的任意 `i` ，如果 `s` 中的 __前__ `i` 个字母和接下来的 `i` 个字母 __相等__ ，删除 __前__ `i` 个字母。

例如，如果 `s = "ababc"` ，那么在一步操作中，你可以删除 `s` 的前两个字母得到 `"abc"` ，因为 `s` 的前两个字母和接下来的两个字母都等于 `"ab"` 。

返回删除 `s` 所需的最大操作数。

__示例:__

示例 1：

```text
输入：s = "abcabcdabc"
输出：2
解释：
```

- 删除前 3 个字母（"abc"），因为它们和接下来 3 个字母相等。现在，s = "abcdabc"。
- 删除全部字母。

一共用了 2 步操作，所以返回 2 。可以证明 2 是所需的最大操作数。
注意，在第二步操作中无法再次删除 "abc" ，因为 "abc" 的下一次出现并不是位于接下来的 3 个字母。

示例 2：

```text
输入：s = "aaabaab"
输出：4
解释：
```

- 删除第一个字母（"a"），因为它和接下来的字母相等。现在，s = "aabaab"。
- 删除前 3 个字母（"aab"），因为它们和接下来 3 个字母相等。现在，s = "aab"。
- 删除第一个字母（"a"），因为它和接下来的字母相等。现在，s = "ab"。
- 删除全部字母。

一共用了 4 步操作，所以返回 4 。可以证明 4 是所需的最大操作数。

示例 3：

```text
输入：s = "aaaaa"
输出：5
解释：在每一步操作中，都可以仅删除 s 的第一个字母。
```

__提示：__

- `1 <= s.length <= 4000`
- `s` 仅由小写英文字母组成

__思路:__

```text
动态规划
设 dp[i] 表示从第 i 个字母开始的最大操作数
如果 s[i:i + j] == s[i + j:i + (j << 1)], 那么 dp[i] = max(dp[i], dp[i + j]) + 1
为了快速计算 s[i:i + j] == s[i + j:i + (j << 1)], 可以使用 lcp 数组
即最长公共前缀, lcp[i][j] 表示 s[i] 和 s[j] 开始的最长公共前缀
若 s[i] == s[j], 那么 lcp[i][j] = lcp[i + 1][j + 1] + 1
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int deleteString(string s) 
    {
        int n = s.size(), lcp[n + 1][n + 1], dp[n];
        if (equal(s.begin() + 1, s.end(), s.begin())) return n;
        memset(lcp, 0, sizeof(lcp));
        memset(dp, 0, sizeof(dp));
        for (int i = n - 1; i > -1; i--) for (int j = n - 1; j > i; j--) if (s[i] == s[j]) lcp[i][j] = lcp[i + 1][j + 1] + 1;
        for (int i = n - 1; i > -1; i--) {
            for (int j = 1; i + (j << 1) <= n; j++) if (lcp[i][i + j] >= j) dp[i] = max(dp[i], dp[i + j]);
            ++dp[i];
        }
        return dp[0];
    }
};
```

__Java__:

```Java
class Solution {
    public int deleteString(String s) {
        int n = s.length(), lcp[][] = new int[n + 1][n + 1], dp[] = new int[n];
        if (check(s)) return n;
        for (int i = n - 1; i > -1; i--) for (int j = n - 1; j > i; j--) if (s.charAt(i) == s.charAt(j)) lcp[i][j] = lcp[i + 1][j + 1] + 1;
        for (int i = n - 1; i > -1; i--) {
            for (int j = 1; i + (j << 1) <= n; j++) if (lcp[i][i + j] >= j) dp[i] = Math.max(dp[i], dp[i + j]);
            ++dp[i];
        }
        return dp[0];
    }
    
    private boolean check(String s) {
        for (int i = 1, n = s.length(); i < n; i++) if (s.charAt(i) != s.charAt(0)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def deleteString(self, s: str) -> int:
        n = len(s)
        if len(set(s)) == 1: 
            return n
        lcp, dp = [[0] * (n + 1) for _ in range(n + 1)], [0] * n
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, i, -1):
                if s[i] == s[j]:
                    lcp[i][j] = lcp[i + 1][j + 1] + 1
        for i in range(n - 1, -1, -1):
            for j in range(1, ((n - i) >> 1) + 1):
                if lcp[i][i + j] >= j:
                    dp[i] = max(dp[i], dp[i + j])
            dp[i] += 1
        return dp[0]
```
