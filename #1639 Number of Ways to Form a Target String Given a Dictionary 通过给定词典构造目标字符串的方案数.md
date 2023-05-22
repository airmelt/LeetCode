# 1639 Number of Ways to Form a Target String Given a Dictionary 通过给定词典构造目标字符串的方案数

__Description:__

You are given a list of strings of the __same length__ `words` and a string `target`.

Your task is to form `target` using the given `words` under the following rules:

- `target` should be formed from left to right.
- To form the `i ^ th` character (__0-indexed__) of `target`, you can choose the `k ^ th` character of the `j ^ th` string in `words` if `target[i] = words[j][k]`.
- Once you use the `k ^ th` character of the `j ^ th` string of `words`, you __can no longer__ use the `x ^ th` character of any string in `words` where `x <= k`. In other words, all characters to the left of or at index `k` become unusuable for every string.
- Repeat the process until you form the string `target`.

__Notice__ that you can use __multiple characters__ from the __same string__ in `words` provided the conditions above are met.

Return _the number of ways to form `target` from `words`_. Since the answer may be too large, return it __modulo__ `10 ^ 9 + 7`.

__Example:__

Example 1:

```text
Input: words = ["acca","bbbb","caca"], target = "aba"
Output: 6
Explanation: There are 6 ways to form target.
"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("caca")
"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("caca")
"aba" -> index 0 ("acca"), index 1 ("bbbb"), index 3 ("acca")
"aba" -> index 0 ("acca"), index 2 ("bbbb"), index 3 ("acca")
"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("acca")
"aba" -> index 1 ("caca"), index 2 ("bbbb"), index 3 ("caca")
```

Example 2:

```text
Input: words = ["abba","baab"], target = "bab"
Output: 4
Explanation: There are 4 ways to form target.
"bab" -> index 0 ("baab"), index 1 ("baab"), index 2 ("abba")
"bab" -> index 0 ("baab"), index 1 ("baab"), index 3 ("baab")
"bab" -> index 0 ("baab"), index 2 ("baab"), index 3 ("baab")
"bab" -> index 1 ("abba"), index 2 ("baab"), index 3 ("baab")
```

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- All strings in `words` have the same length.
- `1 <= target.length <= 1000`
- `words[i]` and `target` contain only lowercase English letters.

__题目描述:__

给你一个字符串列表 `words` 和一个目标字符串 `target` 。 `words` 中所有字符串都 __长度相同__ 。

你的目标是使用给定的 `words` 字符串列表按照下述规则构造 `target` :

- 从左到右依次构造 `target` 的每一个字符。
- 为了得到 `target` 第 `i` 个字符（下标从 __0__ 开始），当 `target[i] = words[j][k]` 时，你可以使用 `words` 列表中第 `j` 个字符串的第 `k` 个字符。
- 一旦你使用了 `words` 中第 `j` 个字符串的第 `k` 个字符，你不能再使用 `words` 字符串列表中任意单词的第 `x` 个字符（ `x <= k`）。也就是说，所有单词下标小于等于 `k` 的字符都不能再被使用。
- 请你重复此过程直到得到目标字符串 `target` 。

__请注意__， 在构造目标字符串的过程中，你可以按照上述规定使用 `words` 列表中 __同一个字符串__ 的 __多个字符__ 。

请你返回使用 `words` 构造 `target` 的方案数。由于答案可能会很大，请对 `10 ^ 9 + 7` __取余__ 后返回。

__示例:__

（译者注:此题目求的是有多少个不同的 `k` 序列，详情请见示例。）

示例 1：

```text
输入：words = ["acca","bbbb","caca"], target = "aba"
输出：6
解释：总共有 6 种方法构造目标串。
"aba" -> 下标为 0 ("acca")，下标为 1 ("bbbb")，下标为 3 ("caca")
"aba" -> 下标为 0 ("acca")，下标为 2 ("bbbb")，下标为 3 ("caca")
"aba" -> 下标为 0 ("acca")，下标为 1 ("bbbb")，下标为 3 ("acca")
"aba" -> 下标为 0 ("acca")，下标为 2 ("bbbb")，下标为 3 ("acca")
"aba" -> 下标为 1 ("caca")，下标为 2 ("bbbb")，下标为 3 ("acca")
"aba" -> 下标为 1 ("caca")，下标为 2 ("bbbb")，下标为 3 ("caca")
```

示例 2：

```text
输入：words = ["abba","baab"], target = "bab"
输出：4
解释：总共有 4 种不同形成 target 的方法。
"bab" -> 下标为 0 ("baab")，下标为 1 ("baab")，下标为 2 ("abba")
"bab" -> 下标为 0 ("baab")，下标为 1 ("baab")，下标为 3 ("baab")
"bab" -> 下标为 0 ("baab")，下标为 2 ("baab")，下标为 3 ("baab")
"bab" -> 下标为 1 ("abba")，下标为 2 ("baab")，下标为 3 ("baab")
```

示例 3：

```text
输入：words = ["abcd"], target = "abcd"
输出：1
```

示例 4：

```text
输入：words = ["abab","baba","abba","baab"], target = "abba"
输出：16
```

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 1000`
- `words` 中所有单词长度相同。
- `1 <= target.length <= 1000`
- `words[i]` 和 `target` 都仅包含小写英文字母。

__思路:__

```text
动态规划
由于只需要记录对应位置的字符个数
可以先将所有字符对应的位置的数量记录下来
设 dp[i][j] 表示到 target[:j] 为止使用 words[:i] 中的字符串可以构成的结果数
初始化 dp[i][0] = 1, 表示空字符串有 1 种构造方式
转移方程为 dp[i][j + 1] = dp[i - 1][j + 1] + dp[i - 1][j] * count[i - 1][target[j]]
因为对于每个 words 中的字符, 如果不选新加入的字符串 words[i] 那就是 dp[i - 1][j + 1], 如果需要选择 words[i] 根据乘法原理有 dp[i - 1][j] * count[i - 1][target[j]] 种方式可以选择
注意如果 target 的长度大于 words 中字符串的长度是不能构造结果的, 可以剪枝直接返回 0
由于 dp[i] 只取决于 dp[i - 1], 可以用滚动数组优化空间复杂度
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numWays(vector<string>& words, string target) 
    {
        int m = words.front().size(), n = target.size(), MOD = 1e9 + 7, count[m][26];
        if (m < n) return 0;
        memset(count, 0, sizeof count);
        for (int i = 0, l = words.size(); i < l; i++) for (int j = 0; j < m; j++) ++count[j][words[i][j] - 'a'];
        vector<long long> dp(n + 1);
        dp.front() = 1;
        for (int i = 1; i <= m; i++) 
        {
            vector<long long> cur(n + 1);
            cur.front() = 1;
            for (int j = 0, l = min(i, n); j < l; j++) cur[j + 1] = (dp[j + 1] + dp[j] * count[i - 1][target[j] - 'a']) % MOD;
            swap(dp, cur);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numWays(String[] words, String target) {
        int m = words[0].length(), n = target.length(), MOD = 1_000_000_007, count[][] = new int[m][26];
        for (int i = 0, l = words.length; i < l; i++) for (int j = 0; j < m; j++) ++count[j][words[i].charAt(j) - 'a'];
        long[][] dp = new long[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = 1;
        for (int i = 1; i <= m; i++) for (int j = 0; j < Math.min(i, n); j++) dp[i][j + 1] = (dp[i - 1][j + 1] + dp[i - 1][j] * count[i - 1][target.charAt(j) - 'a']) % MOD;
        return (int)dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def numWays(self, words: List[str], target: str) -> int:
        MOD, lt, lw, f = 10 ** 9 + 7, len(target), len(words[0]), lru_cache(None)(lambda c, t: sum(w[t] == c for w in words))
        
        @lru_cache(None)
        def dfs(start: int, cur: int) -> int:
            return 1 if start >= lt else 0 if cur >= lw else (dfs(start, cur + 1) + f(target[start], cur) * dfs(start + 1, cur + 1)) % MOD
        return dfs(0, 0)
```
