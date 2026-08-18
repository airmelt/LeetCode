# 3137 Minimum Number of Operations to Make Word K-Periodic K 周期字符串需要的最少操作次数

__Description:__

You are given a string `word` of size `n`, and an integer `k` such that `k` divides `n`.

In one operation, you can pick any two indices `i` and `j`, that are divisible by `k`, then replace the __substring__ of length `k` starting at `i` with the substring of length `k` starting at `j`. That is, replace the substring `word[i..i + k - 1]` with the substring `word[j..j + k - 1]`.

Return _the __minimum__ number of operations required to make `word` k-periodic_.

We say that `word` is __k-periodic__ if there is some string `s` of length `k` such that `word` can be obtained by concatenating `s` an arbitrary number of times. For example, if `word == “ababab”`, then `word` is 2-periodic for `s = "ab"`.

__Example:__

Example 1:

```text
Input: word = "leetcodeleet", k = 4

Output: 1

Explanation:

We can obtain a 4-periodic string by picking i = 4 and j = 0. After this operation, word becomes equal to "leetleetleet".
```

Example 2:

```text
Input: word = "leetcoleet", k = 2

Output: 3

Explanation:

We can obtain a 2-periodic string by applying the operations in the table below.
```

| i | j | word |
| --- | --- | --- |
| 0 | 2 | etetcoleet |
| 4 | 0 | etetetleet |
| 6 | 0 | etetetetet |

__Constraints:__

- `1 <= n == nums.length <= 10 ^ 5`
- `1 <= k <= word.length`
- `k` divides `word.length`.
- `word` consists only of lowercase English letters.

__题目描述:__

给你一个长度为 `n` 的字符串 `word` 和一个整数 `k` ，其中 `k` 是 `n` 的因数。

在一次操作中，你可以选择任意两个下标 `i` 和 `j`，其中 `0 <= i, j < n` ，且这两个下标都可以被 `k` 整除，然后用从 `j` 开始的长度为 `k` 的子串替换从 `i` 开始的长度为 `k` 的子串。也就是说，将子串 `word[i..i + k - 1]` 替换为子串 `word[j..j + k - 1]` 。

返回使 `word` 成为 __K__ 周期字符串 所需的 __最少__ 操作次数。

如果存在某个长度为 `k` 的字符串 `s`，使得 `word` 可以表示为任意次数连接 `s` ，则称字符串 `word` 是 __K 周期字符串__ 。例如，如果 `word == "ababab"`，那么 `word` 就是 `s = "ab"` 时的 2 周期字符串 。

__示例:__

示例 1：

```text
输入：word = "leetcodeleet", k = 4

输出：1

解释：可以选择 i = 4 和 j = 0 获得一个 4 周期字符串。这次操作后，word 变为 "leetleetleet" 。
```

示例 2：

```text
输入：word = "leetcoleet", k = 2

输出：3

解释：可以执行以下操作获得一个 2 周期字符串。
```

| i | j | word |
| --- | --- | --- |
| 0 | 2 | etetcoleet |
| 4 | 0 | etetetleet |
| 6 | 0 | etetetetet |

__提示：__

- `1 <= n == nums.length <= 10 ^ 5`
- `1 <= k <= word.length`
- `k` 能整除 `word.length` 。
- `word` 仅由小写英文字母组成。

__思路:__

```text
哈希表
每 k 个统计子字符串的数目
n 个字符可以分成 n // k 段
减去最大的字符串的个数即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumOperationsToMakeKPeriodic(string word, int k) 
    {
        int n = word.length(), mx = 0;
        unordered_map<string, int> m;
        for (int i = 0; i < n; i += k) mx = max(mx, ++m[word.substr(i, k)]);
        return n / k - mx;
    }
};
```

__Java__:

```Java
class Solution 
{
public:
    int minimumOperationsToMakeKPeriodic(string word, int k) 
    {
        int n = word.length(), mx = 0;
        unordered_map<string, int> m;
        for (int i = 0; i < n; i += k) mx = max(mx, ++m[word.substr(i, k)]);
        return n / k - mx;
    }
};
```

__Python__:

```Python
class Solution:
    def minimumOperationsToMakeKPeriodic(self, word: str, k: int) -> int:
        return (n := len(word)) // k - max(Counter(word[i - k: i] for i in range(k, n + 1, k)).values())
```
