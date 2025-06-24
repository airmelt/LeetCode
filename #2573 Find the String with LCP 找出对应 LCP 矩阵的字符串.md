# 2573 Find the String with LCP 找出对应 LCP 矩阵的字符串

__Description:__

We define the `lcp` matrix of any __0-indexed__ string `word` of `n` lowercase English letters as an `n x n` grid such that:

- `lcp[i][j]` is equal to the length of the __longest common prefix__ between the substrings `word[i,n-1]` and `word[j,n-1]`.

Given an `n x n` matrix `lcp`, return the alphabetically smallest string `word` that corresponds to `lcp`. If there is no such string, return an empty string.

A string `a` is lexicographically smaller than a string `b` (of the same length) if in the first position where `a` and `b` differ, string `a` has a letter that appears earlier in the alphabet than the corresponding letter in `b`. For example, `"aabd"` is lexicographically smaller than `"aaca"` because the first position they differ is at the third letter, and `'b'` comes before `'c'`.

__Example:__

Example 1:

```text
Input: lcp = [[4,0,2,0],[0,3,0,1],[2,0,2,0],[0,1,0,1]]
Output: "abab"
Explanation: lcp corresponds to any 4 letter string with two alternating letters. The lexicographically smallest of them is "abab".
```

Example 2:

```text
Input: lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,1]]
Output: "aaaa"
Explanation: lcp corresponds to any 4 letter string with a single distinct letter. The lexicographically smallest of them is "aaaa".
```

Example 3:

```text
Input: lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,3]]
Output: ""
Explanation: lcp[3][3] cannot be equal to 3 since word[3,...,3] consists of only a single letter; Thus, no answer exists.
```

__Constraints:__

- `1 <= n == lcp.length == lcp[i].length <= 1000`
- `0 <= lcp[i][j] <= n`

__题目描述:__

对任一由 `n` 个小写英文字母组成的字符串 `word` ，我们可以定义一个 `n x n` 的矩阵，并满足:

- `lcp[i][j]` 等于子字符串 `word[i,...,n-1]` 和 `word[j,...,n-1]` 之间的最长公共前缀的长度。

给你一个 `n x n` 的矩阵 `lcp` 。返回与 `lcp` 对应的、按字典序最小的字符串 `word` 。如果不存在这样的字符串，则返回空字符串。

对于长度相同的两个字符串 `a` 和 `b` ，如果在 `a` 和 `b` 不同的第一个位置，字符串 `a` 的字母在字母表中出现的顺序先于 `b` 中的对应字母，则认为字符串 `a` 按字典序比字符串 `b` 小。例如， `"aabd"` 在字典上小于 `"aaca"` ，因为二者不同的第一位置是第三个字母，而 `'b'` 先于 `'c'` 出现。

__示例:__

示例 1：

```text
输入：lcp = [[4,0,2,0],[0,3,0,1],[2,0,2,0],[0,1,0,1]]
输出："abab"
解释：lcp 对应由两个交替字母组成的任意 4 字母字符串，字典序最小的是 "abab" 。
```

示例 2：

```text
输入：lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,1]]
输出："aaaa"
解释：lcp 对应只有一个不同字母的任意 4 字母字符串，字典序最小的是 "aaaa" 。
```

示例 3：

```text
输入：lcp = [[4,3,2,1],[3,3,2,1],[2,2,2,1],[1,1,1,3]]
输出：""
解释：lcp[3][3] 无法等于 3 ，因为 word[3,...,3] 仅由单个字母组成；因此，不存在答案。
```

__提示：__

- `1 <= n == lcp.length == lcp[i].length <= 1000`
- `0 <= lcp[i][j] <= n`

__思路:__

```text
贪心
尝试从 'a' 开始构造字符串，直到无法继续
根据 lcp[i][j] 的值判断 s[i] 和 s[j] 的关系
s[0] 一定是 'a'
当 lcp[0][j] > 0 时，s[j] 一定是 'a', 否则一定有 s[0] != s[j], 那么尝试填入 'b'，如果不行就填入 'c'，依次类推
如果 s 中还有没有构建完的字符说明不能构成字符串, 返回空字符串
最后检查 lcp[i][j] 是否满足条件
if s[i] != s[j] 则 lcp[i][j] 必须为 0
if s[i] == s[j] 则 lcp[i][j] 必须为 lcp[i + 1][j + 1] + 1 或者 1 (i == n - 1 or j == n - 1)
如果不满足则返回空字符串
最后返回构建好的字符串
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1), 不计返回值的空间
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string findTheString(vector<vector<int>>& lcp) 
    {
        int n = lcp.size(), i = 0, j = 0;
        string s(n, 0);
        for (char c = 'a'; c <= 'z' and i < n; c++)
        {
            while (i < n and s[i]) ++i;
            for (j = i; j < n; j++) if (lcp[i][j]) s[j] = c;
        }
        while (i < n) if (!s[i++]) return "";
        for (i = n - 1; i > -1; i--) for (j = n - 1; j > -1; j--) if (lcp[i][j] != (s[i] != s[j] ? 0 : (i == n - 1 or j == n - 1 ? 1 : lcp[i + 1][j + 1] + 1))) return "";
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String findTheString(int[][] lcp) {
        int n = lcp.length, i = 0, j = 0;
        char[] s = new char[n];
        for (char c = 'a'; c <= 'z' && i < n; c++) {
            while (i < n && s[i] > 0) ++i;
            for (j = i; j < n; j++) if (lcp[i][j] != 0) s[j] = c; 
        }
        while (i < n) if (s[i++] == 0) return "";
        for (i = n - 1; i > -1; i--) for (j = n - 1; j > -1; j--) if (lcp[i][j] != (s[i] != s[j] ? 0 : (i == n - 1 || j == n - 1 ? 1 : lcp[i + 1][j + 1] + 1))) return "";
        return new String(s);
    }
}
```

__Python__:

```Python
class Solution:
    def findTheString(self, lcp: List[List[int]]) -> str:
        i, s = 0, [None] * (n := len(lcp))
        for c in ascii_lowercase:
            while i < n and s[i]:
                i += 1
            if i == n:
                break
            for j in range(i, n):
                if lcp[i][j]:
                    s[j] = c
        if None in s:
            return ""
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, -1, -1):
                if lcp[i][j] != (0 if s[i] != s[j] else 1 if i == n - 1 or j == n - 1 else lcp[i + 1][j + 1] + 1):
                    return ""
        return ''.join(s)
```
