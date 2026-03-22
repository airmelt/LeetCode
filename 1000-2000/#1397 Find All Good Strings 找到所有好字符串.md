# 1397 Find All Good Strings 找到所有好字符串

__Description:__

Given the strings s1 and s2 of size n and the string evil, return the number of good strings.

A good string has size n, it is alphabetically greater than or equal to s1, it is alphabetically smaller than or equal to s2, and it does not contain the string evil as a substring. Since the answer can be a huge number, return this modulo 109 + 7.

__Example:__

Example 1:

Input: n = 2, s1 = "aa", s2 = "da", evil = "b"
Output: 51
Explanation: There are 25 good strings starting with 'a': "aa","ac","ad",...,"az". Then there are 25 good strings starting with 'c': "ca","cc","cd",...,"cz" and finally there is one good string starting with 'd': "da".

Example 2:

Input: n = 8, s1 = "leetcode", s2 = "leetgoes", evil = "leet"
Output: 0
Explanation: All strings greater than or equal to s1 and smaller than or equal to s2 start with the prefix "leet", therefore, there is not any good string.

Example 3:

Input: n = 2, s1 = "gx", s2 = "gz", evil = "x"
Output: 2

__Constraints:__

s1.length == n
s2.length == n
s1 <= s2
1 <= n <= 500
1 <= evil.length <= 50
All strings consist of lowercase English letters.

__题目描述:__

给你两个长度为 n 的字符串 s1 和 s2 ，以及一个字符串 evil 。请你返回 好字符串 的数目。

好字符串 的定义为：它的长度为 n ，字典序大于等于 s1 ，字典序小于等于 s2 ，且不包含 evil 为子字符串。

由于答案可能很大，请你返回答案对 10^9 + 7 取余的结果。

__示例:__

示例 1：

输入：n = 2, s1 = "aa", s2 = "da", evil = "b"
输出：51
解释：总共有 25 个以 'a' 开头的好字符串："aa"，"ac"，"ad"，...，"az"。还有 25 个以 'c' 开头的好字符串："ca"，"cc"，"cd"，...，"cz"。最后，还有一个以 'd' 开头的好字符串："da"。

示例 2：

输入：n = 8, s1 = "leetcode", s2 = "leetgoes", evil = "leet"
输出：0
解释：所有字典序大于等于 s1 且小于等于 s2 的字符串都以 evil 字符串 "leet" 开头。所以没有好字符串。

示例 3：

输入：n = 2, s1 = "gx", s2 = "gz", evil = "x"
输出：2

__提示：__

s1.length == n
s2.length == n
s1 <= s2
1 <= n <= 500
1 <= evil.length <= 50
所有字符串都只包含小写英文字母。

__思路:__

```text
数位 DP + KMP
将 s1 看做数字的下界, s2 看做数字的上界
可以转换为求 [s1, s2] 中有多少种排列方式使得字符串中不包含 evil
数位 DP 的通解为 dp[pos][mask][is_limit]
小写字符可以看做 0-25 的 26 进制数
可以转换为求 [空字符串, s1] 以及 [空字符串, s2] 的字符串数目再做差
设 dp[pos][hit] 表示 s[:pos] 中命中 evil[:hit] 的排列数
采用记忆化求取, 当 hit 为 len(evil) 时表示命中所有 evil 中的字符, 这时不能返回字符串，返回 0
如果遍历到 s 的终点, 说明这种情况可以构造 1 个字符串, 返回 1
如果已经访问过, 直接拿 dp[pos][hit]
is_limit 参数表示是否到达上界
比如 'abc', 如果第一个字符取 'a' 那么第二个字符只能取 'a', 'b', 这种情况下, 第二个字符取 'a', 第三个字符可以遍历 'a'-'z' 取值, 如果第二个字符也取上限 'b', 那么第三个字符只能遍历 'a'-'c'
为了快速找到 evil 是否在当前的可能字符串中
预先生成 evil 的 KMP 算法的 next 数组
可以将搜索时间大大缩短
时间复杂度为 O(MN), 空间复杂度为 O(MN), 其中 M, N 分别为字符串 s1/s2 和 evil 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    vector<int> nxt;
    const long long MOD = 1e9 + 7;
    int m, _n;
    string _evil;

    int helper(const string &s)
    {
        int dp[_n][m];
        memset(dp, -1L, sizeof(dp));
        function<int(int, int, bool)> dfs = [&](int pos, int hit, bool is_limit) -> int
        {
            if (hit == m) return 0;
            if (pos == _n) return 1;
            if (!is_limit and dp[pos][hit] > -1) return dp[pos][hit];
            long long result = 0;
            for (char c = 'a', right = is_limit ? s[pos] : 'z'; c <= right; c++)
            {
                int next_hit = hit;
                while (next_hit > 0 and c != _evil[next_hit]) next_hit = nxt[next_hit - 1];
                result += dfs(pos + 1, next_hit + (c == _evil[next_hit]), is_limit and c == right);
                result %= MOD;
            }
            if (!is_limit) dp[pos][hit] = result;
            return result;
        };
        return dfs(0, 0, true);
    }
public:
    int findGoodStrings(int n, string s1, string s2, string evil) 
    {
        _n = n;
        m = evil.size();
        _evil = evil;
        int j = 0;
        nxt.resize(m);
        for (int i = 1; i < m; i++)
        {
            while (j > 0 and evil[j] != evil[i]) j = nxt[j - 1];
            nxt[i] = evil[j] == evil[i] ? ++j : j;
        }
        return (helper(s2) - helper(s1) + MOD) % MOD + (s1.find(evil) == -1);
    }
};
```

__Java__:

```Java
class Solution {
    private int[] nxt;

    private int m, _n;

    private long[][] dp;

    private static long MOD = 1_000_000_007L;

    String _s, _evil;

    public int findGoodStrings(int n, String s1, String s2, String evil) {
        _evil = evil;
        int j = 0;
        _n = n;
        m = evil.length();
        nxt = new int[m];
        dp = new long[_n][m];
        for (int i = 1; i < m; i++) {
            while (j > 0 && evil.charAt(i) != evil.charAt(j)) j = nxt[j - 1];
            nxt[i] = evil.charAt(i) == evil.charAt(j) ? ++j : j;
        }
        return (int)(((helper(s2) - helper(s1)) + MOD) % MOD) + (s1.indexOf(evil) != -1 ? 0 : 1);
    }

    private long helper(String s) {
        _s = s;
        for (int i = 0; i < _n; i++) Arrays.fill(dp[i], -1L);
        return dfs(0, 0, true);
    }

    private long dfs(int pos, int hit, boolean isLimit) {
        if (hit == m) return 0;
        if (pos == _n) return 1;
        if (!isLimit && dp[pos][hit] > -1) return dp[pos][hit];
        long result = 0;
        for (char c = 'a', right = isLimit ? _s.charAt(pos) : 'z'; c <= right; c++) {
            int nextHit = hit;
            while (nextHit > 0 && c != _evil.charAt(nextHit)) nextHit = nxt[nextHit - 1];
            result += dfs(pos + 1, nextHit + (c == _evil.charAt(nextHit) ? 1 : 0), isLimit && c == right);
            result %= MOD;
        }
        if (!isLimit) dp[pos][hit] = result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findGoodStrings(self, n: int, s1: str, s2: str, evil: str) -> int:
        MOD, nxt, j = 10 ** 9 + 7, [0] * (m := len(evil)), 0

        for i in range(1, m):
            while j and evil[j] != evil[i]:
                j = nxt[j - 1]
            nxt[i] = (j := j + (evil[j] == evil[i]))
            
        def helper(upper: str) -> int:
            @lru_cache(None)
            def dfs(pos: int, bound: bool, hit: int) -> int:
                if hit == m:
                    return 0
                if pos == n:
                    return 1
                result, right = 0, upper[pos] if bound else 'z'
                for c in range(ord('a'), ord(right) + 1):
                    next_hit = hit
                    while next_hit and chr(c) != evil[next_hit]:
                        next_hit = nxt[next_hit - 1]
                    result += dfs(pos + 1, bound and chr(c) == right, next_hit + (chr(c) == evil[next_hit]))
                return result % MOD
            return dfs(0, True, 0)
        return (helper(s2) - helper(s1) + (evil not in s1)) % MOD
```
