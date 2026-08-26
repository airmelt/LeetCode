# 3144 Minimum Substring Partition of Equal Character Frequency 分割字符频率相等的最少子字符串

__Description:__

Given a string `s`, you need to partition it into one or more __balanced substrings__. For example, if `s == "ababcc"` then `("abab", "c", "c")`, `("ab", "abc", "c")`, and `("ababcc")` are all valid partitions, but `("a", "bab", "cc")`, `("aba", "bc", "c")`, and `("ab", "abcc")` are not. The unbalanced substrings are bolded.

Return the __minimum__ number of substrings that you can partition `s` into.

__Note:__
A __balanced__ string is a string where each character in the string occurs the same number of times.

__Example:__

Example 1:

```text
Input: s = "fabccddg"

Output: 3

Explanation:

We can partition the string s into 3 substrings in one of the following ways: ("fab, "ccdd", "g"), or ("fabc", "cd", "dg").
```

Example 2:

```text
Input: s = "abababaccddb"

Output: 2

Explanation:

We can partition the string s into 2 substrings like so: ("abab", "abaccddb").
```

__Constraints:__

- `1 <= s.length <= 1000`
- `s` consists only of English lowercase letters.

__题目描述:__

给你一个字符串 `s` ，你需要将它分割成一个或者更多的 __平衡__ 子字符串。比方说，`s == "ababcc"` 那么 `("abab", "c", "c")` ，`("ab", "abc", "c")` 和 `("ababcc")` 都是合法分割，但是 `("a", "bab", "cc")` ，`("aba", "bc", "c")` 和 `("ab", "abcc")` 不是，不平衡的子字符串用粗体表示。

请你返回 `s` __最少__ 能分割成多少个平衡子字符串。

__注意：__
一个 __平衡__ 字符串指的是字符串中所有字符出现的次数都相同。

__示例:__

示例 1：

```text
输入：s = "fabccddg"

输出：3

解释：

我们可以将 s 分割成 3 个子字符串：("fab, "ccdd", "g") 或者 ("fabc", "cd", "dg") 。
```

示例 2：

```text
输入：s = "abababaccddb"

输出：2

解释：

我们可以将 s 分割成 2 个子字符串：("abab", "abaccddb") 。
```

__提示：__

- `1 <= s.length <= 1000`
- `s` 只包含小写英文字母。

__思路:__

```text
动态规划
设 dp[i + 1] 表示 s[:i] 分割成平衡字符串的最少个数
由于单个的字符一定平衡
所以一定可以将字符串分割成 n 个平衡字符串
初始化 dp[0] = 0 表示空串能分割成 0 个平衡字符串, dp[i] = n, i > 0
对于 -1 < j <= i, 倒序遍历 s[j:i + 1]
将 s[j] 加入哈希表中, 记录子字符串中出现的最多字符的次数为 mx
如果哈希表的长度和 mx 的乘积为子字符串的长度, 即 len(map) * mx == i - j + 1
则说明 s[j:i + 1] 为一个平衡字符串
那么 dp[i + 1] 更新为 min(dp[i + 1], dp[j] + 1)
最后返回 dp[n] 即可
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumSubstringsInPartition(string s) 
    {
        int n = s.size();
        vector<int> dp(n + 1, n);
        dp.front() = 0;
        for (int i = 0, mx = 0; i < n; i++, mx = 0) 
        {
            unordered_map<int, int> m;
            for (int j = i; j > -1; j--) if ((mx = max(mx, ++m[s[j]])) * m.size() == i - j + 1) dp[i + 1] = min(dp[i + 1], dp[j] + 1);
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumSubstringsInPartition(String s) {
        int n = s.length(), dp[] = new int[n + 1];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for (int i = 0, mx = 0; i < n; i++, mx = 0) {
            var map = new HashMap<Character, Integer>();
            for (int j = i; j > -1; j--) if ((mx = Math.max(mx, map.merge(s.charAt(j), 1, Integer::sum))) * map.size() == i - j + 1) dp[i + 1] = Math.min(dp[i + 1], dp[j] + 1);
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def minimumSubstringsInPartition(self, s: str) -> int:
        dp = [0] + [n := len(s)] * n
        for i in range(n):
            c, mx = Counter(), 0
            for j in range(i, -1, -1):
                c[s[j]] += 1
                if (mx := max(c[s[j]], mx)) * len(c) == i - j + 1:
                    dp[i + 1] = min(dp[j] + 1, dp[i + 1])
        return dp[n]
```
