# 2746 Decremental String Concatenation 字符串连接删减字母

__Description:__

You are given a __0-indexed__ array `words` containing `n` strings.

Let's define a __join__ operation `join(x, y)` between two strings `x` and `y` as concatenating them into `xy`. However, if the last character of `x` is equal to the first character of `y`, one of them is __deleted__.

For example `join("ab", "ba") = "aba"` and `join("ab", "cde") = "abcde"`.

You are to perform `n - 1` __join__ operations. Let `str0 = words[0]`. Starting from `i = 1` up to `i = n - 1`, for the `i ^ th` operation, you can do one of the following:

- Make `str_i = join(str_i - 1, words[i])`
- Make `str_i = join(words[i], str_i - 1)`

Your task is to __minimize__ the length of `strn - 1`.

Return _an integer denoting the minimum possible length of_ `strn - 1`.

__Example:__

Example 1:

```text
Input: words = ["aa","ab","bc"]
Output: 4
Explanation: In this example, we can perform join operations in the following order to minimize the length of str2: 
str0 = "aa"
str1 = join(str0, "ab") = "aab"
str2 = join(str1, "bc") = "aabc" 
It can be shown that the minimum possible length of str2 is 4.
```

Example 2:

```text
Input: words = ["ab","b"]
Output: 2
Explanation: In this example, str0 = "ab", there are two ways to get str1: 
join(str0, "b") = "ab" or join("b", str0) = "bab". 
The first string, "ab", has the minimum length. Hence, the answer is 2.
```

Example 3:

```text
Input: words = ["aaa","c","aba"]
Output: 6
Explanation: In this example, we can perform join operations in the following order to minimize the length of str2: 
str0 = "aaa"
str1 = join(str0, "c") = "aaac"
str2 = join("aba", str1) = "abaaac"
It can be shown that the minimum possible length of str2 is 6.
```

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 50`
- Each character in `words[i]` is an English lowercase letter

__题目描述:__

给你一个下标从 __0__ 开始的数组 `words` ，它包含 `n` 个字符串。

定义 __连接__ 操作 `join(x, y)` 表示将字符串 `x` 和 `y` 连在一起，得到 `xy` 。如果 `x` 的最后一个字符与 `y` 的第一个字符相等，连接后两个字符中的一个会被 __删除__ 。

比方说 `join("ab", "ba") = "aba"` ， `join("ab", "cde") = "abcde"` 。

你需要执行 `n - 1` 次 __连接__ 操作。令 `str0 = words[0]` ，从 `i = 1` 直到 `i = n - 1` ，对于第 `i` 个操作，你可以执行以下操作之一:

- 令 `str_i = join(str_i - 1, words[i])`
- 令 `str_i = join(words[i], str_i - 1)`

你的任务是使 `strn - 1` 的长度 __最小__ 。

请你返回一个整数，表示 `strn - 1` 的最小长度。

__示例:__

示例 1：

```text
输入: words = ["aa","ab","bc"]
输出: 4
解释: 这个例子中，我们按以下顺序执行连接操作，得到 str2 的最小长度:
str0 = "aa"
str1 = join(str0, "ab") = "aab"
str2 = join(str1, "bc") = "aabc"
str2 的最小长度为 4 。
```

示例 2：

```text
输入：words = ["ab","b"]
输出：2
解释：这个例子中，str0 = "ab"，可以得到两个不同的 str1：
join(str0, "b") = "ab" 或者 join("b", str0) = "bab" 。
第一个字符串 "ab" 的长度最短，所以答案为 2 。
```

示例 3：

```text
输入: words = ["aaa","c","aba"]
输出: 6
解释: 这个例子中，我们按以下顺序执行连接操作，得到 str2 的最小长度:
str0 = "aaa"
str1 = join(str0, "c") = "aaac"
str2 = join("aba", str1) = "abaaac"
str2 的最小长度为 6 。
```

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 50`
- `words[i]` 中只包含小写英文字母。

__思路:__

```text
动态规划
设 dp[i][j][k] 表示选择前 i 个字符串形成以 j 开头 k 结尾的字符串的长度
则 dp[0][words[0][0]][words[0][-1]] 初始化为 words[0] 的长度
其余全部初始化为 INF
对于第 i 个字符串, 遍历 26 X 26 个字符的情况
令 words[i] 的长度为 l, 第一个字符为 c1, 结尾的字符为 c2
要么 words[i] 接在 str_i 的后面, 则 dp[i][j][c2] = min(dp[i][j][c2], dp[i - 1][j][k] + l - (k == c1))
要么接在前面 dp[i][c1][k] = min(dp[i][c1][k], dp[i - 1][j][k] + l - (c2 == j))
最后返回 dp[-1] 中的最小值
时间复杂度为 O(N), 空间复杂度为 O(N), 实际上都还要乘上一个常数 C ^ 2, 表示字符集的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimizeConcatenatedLength(vector<string>& words) 
    {
        int n = words.size(), INF = 0x3f3f3f3f, result = INF;
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(26, vector<int>(26, INF)));
        dp.front()[words.front().front() - 'a'][words.front().back() - 'a'] = words.front().size();
        for (int i = 1; i < n; i++) 
        {
            for (int j = 0, l = words[i].size(), c1 = words[i].front() - 'a', c2 = words[i].back() - 'a'; j < 26; j++) 
            {
                for (int k = 0; k < 26; k++) 
                {
                    dp[i][j][c2] = min(dp[i][j][c2], dp[i - 1][j][k] + l - (k == c1));
                    dp[i][c1][k] = min(dp[i][c1][k], dp[i - 1][j][k] + l - (c2 == j));
                }
            }
        }
        for (int i = 0; i < 26; i++) for (int j = 0; j < 26; j++) result = min(result, dp.back()[i][j]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimizeConcatenatedLength(String[] words) {
        int n = words.length, dp[][][] = new int[n][26][26], INF = 0x3f3f3f3f, result = INF;
        for (int i = 0; i < n; i++) for (int j = 0; j < 26; j++) Arrays.fill(dp[i][j], INF);
        dp[0][words[0].charAt(0) - 'a'][words[0].charAt(words[0].length() - 1) - 'a'] = words[0].length();
        for (int i = 1; i < n; i++) {
            for (int j = 0, l = words[i].length(), c1 = words[i].charAt(0) - 'a', c2 = words[i].charAt(words[i].length() - 1) - 'a'; j < 26; j++) {
                for (int k = 0; k < 26; k++) {
                    dp[i][j][c2] = Math.min(dp[i][j][c2], dp[i - 1][j][k] + l - (k == c1 ? 1 : 0));
                    dp[i][c1][k] = Math.min(dp[i][c1][k], dp[i - 1][j][k] + l - (c2 == j ? 1 : 0));
                }
            }
        }
        for (int i = 0; i < 26; i++) for (int j = 0; j < 26; j++) result = Math.min(result, dp[n - 1][i][j]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimizeConcatenatedLength(self, words: List[str]) -> int:
        @lru_cache(None)
        def dfs(i: int, s: str, e: str) -> int:
            return 0 if i == len(words) else min(dfs(i + 1, s, words[i][-1]) + len(words[i]) - int(words[i][0] == e), dfs(i + 1, words[i][0], e) + len(words[i]) - int(words[i][-1] == s))
        return dfs(1, words[0][0], words[0][-1]) + len(words[0])
```
