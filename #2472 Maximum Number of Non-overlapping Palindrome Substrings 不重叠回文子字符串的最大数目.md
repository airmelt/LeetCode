# 2472 Maximum Number of Non-overlapping Palindrome Substrings 不重叠回文子字符串的最大数目

__Description:__

You are given a string `s` and a __positive__ integer `k`.

Select a set of __non-overlapping__ substrings from the string `s` that satisfy the following conditions:

- The __length__ of each substring is __at least__ `k`.
- Each substring is a __palindrome__.

Return the maximum number of substrings in an optimal selection.

A substring is a contiguous sequence of characters within a string.

__Example:__

Example 1:

```text
Input: s = "abaccdbbd", k = 3
Output: 2
Explanation: We can select the substrings underlined in s = "abaccdbbd". Both "aba" and "dbbd" are palindromes and have a length of at least k = 3.
It can be shown that we cannot find a selection with more than two valid substrings.
```

Example 2:

```text
Input: s = "adbcda", k = 2
Output: 0
Explanation: There is no palindrome substring of length at least 2 in the string.
```

__Constraints:__

- `1 <= k <= s.length <= 2000`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `s` 和一个 __正__ 整数 `k` 。

从字符串 `s` 中选出一组满足下述条件且 __不重叠__ 的子字符串:

- 每个子字符串的长度 __至少__ 为 `k` 。
- 每个子字符串是一个 __回文串__ 。

返回最优方案中能选择的子字符串的 最大 数目。

子字符串 是字符串中一个连续的字符序列。

__示例:__

示例 1 ：

```text
输入：s = "abaccdbbd", k = 3
输出：2
解释：可以选择 s = "abaccdbbd" 中斜体加粗的子字符串。"aba" 和 "dbbd" 都是回文，且长度至少为 k = 3 。
可以证明，无法选出两个以上的有效子字符串。
```

示例 2 ：

```text
输入：s = "adbcda", k = 2
输出：0
解释：字符串中不存在长度至少为 2 的回文子字符串。
```

__提示：__

- `1 <= k <= s.length <= 2000`
- `s` 仅由小写英文字母组成

__思路:__

```text
动态规划
设 dp[i + 1] 表示以 i 结尾的回文串的最大个数
初始化 dp[0] = 0, 表示空串
如果 s[l] 不在回文串里, dp[l + 1] = max(dp[l], dp[l + 1])
否则采用中心扩展法 dp[r + 1] = max(dp[r + 1], dp[l] + 1), 由于长度为 r - l + 1 的回文串一定包括长度为 r - l - 1 的回文串
所以 r - l + 1 >= k, 更新 dp[r + 1] = max(dp[r + 1], dp[l] + 1)
时间复杂度为 O(NK), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int maxPalindromes(string s, int k) 
    {
        int n = s.size(), m = (n << 1) - 1;
        vector<int> dp(n + 1);
        for (int i = 0, l = 0, r = 0; i < m; l = (++i) >> 1) 
        {
            dp[l + 1] = max(dp[l], dp[l + 1]);
            for (r = (i + 1) >> 1; l > -1 and r < n and s[l] == s[r]; l--, r++) 
            {
                if (r - l + 1 >= k) 
                {
                    dp[r + 1] = max(dp[r + 1], dp[l] + 1);
                    break;
                }
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int maxPalindromes(String s, int k) {
        int n = s.length(), dp[] = new int[n + 1], m = (n << 1) - 1;
        for (int i = 0, l = 0, r = 0; i < m; i++) {
            dp[(l = (i >> 1)) + 1] = Math.max(dp[l + 1], dp[l]);
            for (r = (i + 1) >> 1; l > -1 && r < n && s.charAt(l) == s.charAt(r); l--, r++) {
                if (r - l + 1 >= k) {
                    dp[r + 1] = Math.max(dp[r + 1], dp[l] + 1);
                    break;
                }
            }
        }
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def maxPalindromes(self, s: str, k: int) -> int:
        result, n, right, candidates = 0, len(s), 0, deque()

        def count(start: int, end: int):
            nonlocal candidates
            while start > -1 and end < n and s[start] == s[end]:
                if end - start + 1 >= k:
                    candidates.append([start, end])
                start -= 1
                end += 1
                
        for i in range(n):
            count(i, i)
            count(i, i + 1)
        for start, end in candidates:
            if start >= right and end - start + 1 >= k:
                right = end + 1
                result += 1
        return result
```
