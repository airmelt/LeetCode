# 2014 Longest Subsequence Repeated k Times 重复 K 次的最长子序列

__Description:__

You are given a string `s` of length `n`, and an integer `k`. You are tasked to find the __longest subsequence repeated__ `k` times in string `s`.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

A subsequence `seq` is __repeated__ `k` times in the string `s` if `seq * k` is a subsequence of `s`, where `seq * k` represents a string constructed by concatenating `seq` `k` times.

- For example, `"bba"` is repeated `2` times in the string `"bababcba"`, because the string `"bbabba"`, constructed by concatenating `"bba"` `2` times, is a subsequence of the string "__b__ a __b__ __a__ __b__ c __b__ __a__".

Return _the __longest subsequence repeated___ `k` _times in string_ `s`_. If multiple such subsequences are found, return the __lexicographically largest__ one. If there is no such subsequence, return an __empty__ string_.

__Example:__

Example 1:

![2014-1](https://assets.leetcode.com/uploads/2021/08/30/longest-subsequence-repeat-k-times.png)

```text
Input: s = "letsleetcode", k = 2
Output: "let"
Explanation: There are two longest subsequences repeated 2 times: "let" and "ete".
"let" is the lexicographically largest one.
```

Example 2:

```text
Input: s = "bb", k = 2
Output: "b"
Explanation: The longest subsequence repeated 2 times is "b".
```

Example 3:

```text
Input: s = "ab", k = 2
Output: ""
Explanation: There is no subsequence repeated 2 times. Empty string is returned.
```

__Constraints:__

- `n == s.length`
- `2 <= n, k <= 2000`
- `2 <= n < k * 8`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个长度为 `n` 的字符串 `s` ，和一个整数 `k` 。请你找出字符串 `s` 中 __重复__ `k` 次的 __最长子序列__ 。

子序列 是由其他字符串删除某些（或不删除）字符派生而来的一个字符串。

如果 `seq * k` 是 `s` 的一个子序列，其中 `seq * k` 表示一个由 `seq` 串联 `k` 次构造的字符串，那么就称 `seq` 是字符串 `s` 中一个 __重复 `k` 次__ 的子序列。

- 举个例子， `"bba"` 是字符串 `"bababcba"` 中的一个重复 `2` 次的子序列，因为字符串 `"bbabba"` 是由 `"bba"` 串联 `2` 次构造的，而 `"bbabba"` 是字符串 "__b__ a __b__ __a__ __b__ c __b__ __a__" 的一个子序列。

返回字符串 `s` 中 __重复 k 次的最长子序列__ 。如果存在多个满足的子序列，则返回 __字典序最大__ 的那个。如果不存在这样的子序列，返回一个 __空__ 字符串。

__示例:__

示例 1：

![2014-2](https://assets.leetcode.com/uploads/2021/08/30/longest-subsequence-repeat-k-times.png)

```text
输入：s = "letsleetcode", k = 2
输出："let"
解释：存在两个最长子序列重复 2 次：let" 和 "ete" 。
"let" 是其中字典序最大的一个。
```

示例 2：

```text
输入：s = "bb", k = 2
输出："b"
解释：重复 2 次的最长子序列是 "b" 。
```

示例 3：

```text
输入：s = "ab", k = 2
输出：""
解释：不存在重复 2 次的最长子序列。返回空字符串。
```

__提示：__

- `n == s.length`
- `2 <= k <= 2000`
- `2 <= n < k * 8`
- `s` 由小写英文字母组成

__思路:__

```text
贪心
注意到 n < k * 8
所以子序列的长度不会超过 8
并且子序列的字符出现次数必须不小于 k
统计 s 中每个字符出现的次数, 然后按照字典序从大到小的顺序构造 hot
每次从 hot 中取出 i 个字符(i 从 n 遍历到 1), 然后进行全排列，检查是否有子序列满足题目要求
时间复杂度为 O(N), 空间复杂度为 O(1), 统计字符需要 O(N) 的时间复杂度, O(1) 的空间复杂度, 全排列需要 O(C!) 的时间复杂度, O(C!) 的空间复杂度, C 最大为 8, N 为字符串 S 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string longestSubsequenceRepeatedK(string s, int k) 
    {
        vector<int> cnt(26);
        for (const auto& c : s) ++cnt[c - 'a'];
        for (int i = 25; i > -1; i--) for (int j = 0; j < cnt[i] / k; j++) hot += ((char)('a' + i));
        n = hot.size();
        for (int cur = n; cur > 0; cur--) if (dfs(s, 0, cur, k)) return result;
        return "";
    }
private:
    int n;
    string hot;
    string result;
    vector<bool> visited = vector<bool>(9);

    bool dfs(const string& s, int start, int cur, int k) 
    {
        if (start == cur) return check(s, cur, k);
        for (int i = 0; i < n; i++) 
        {
            if (!visited[i]) 
            {
                visited[i] = true;
                result += hot[i];
                if (dfs(s, start + 1, cur, k)) return true;
                result.erase(result.begin() + start);
                visited[i] = false;
            }
        }
        return false;
    }

    bool check(const string& s, int cur, int k) 
    {
        int idx = 0, total = 0;
        for (const auto& c : s)
        {
            if (c == result[idx])
            {
                total += idx == cur - 1;
                idx = idx == cur - 1 ? 0 : idx + 1;
            }
        }
        return total >= k;
    }
};
```

__Java__:

```Java
class Solution {
    private int n;
    private StringBuilder hot = new StringBuilder();
    private StringBuilder result = new StringBuilder();
    private boolean[] visited = new boolean[9];

    public String longestSubsequenceRepeatedK(String s, int k) {
        int count[] = new int[26];
        for (char c : s.toCharArray()) ++count[c - 'a'];
        for (int i = 25; i > -1; i--) for (int j = 0; j < count[i] / k; j++) hot.append((char)('a' + i));
        n = hot.length();
        for (int cur = n; cur > 0; cur--) if (dfs(s, 0, cur, k)) return result.toString();
        return "";
    }

    private boolean dfs(String s, int start, int cur, int k) {
        if (start == cur) return check(s, cur, k);
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                visited[i] = true;
                result.append(hot.charAt(i));
                if (dfs(s, start + 1, cur, k)) return true;
                result.deleteCharAt(start);
                visited[i] = false;
            }
        }
        return false;
    }

    private boolean check(String s, int cur, int k) {
        int idx = 0, total = 0;
        for (char c : s.toCharArray()) {
            if (c == result.charAt(idx)) {
                total += idx == cur - 1 ? 1 : 0;
                idx = idx == cur - 1 ? 0 : idx + 1;
            }
        }
        return total >= k;
    }
}
```

__Python__:

```Python
class Solution:
    def longestSubsequenceRepeatedK(self, s: str, k: int) -> str:
        c = Counter(s)
        hot = ''.join(e * (c[e] // k) for e in sorted(c, reverse=True))
        for i in range(len(hot), 0, -1):
            for item in permutations(hot, i):
                word, ss = ''.join(item), iter(s)
                if all(c in ss for c in word * k):
                    return word
        return ''
```
