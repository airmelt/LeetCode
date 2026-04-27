# 1960 Maximum Product of the Length of Two Palindromic Substrings 两个回文子字符串长度的最大乘积

__Description:__

You are given a __0-indexed__ string `s` and are tasked with finding two __non-intersecting palindromic__ substrings of __odd__ length such that the product of their lengths is maximized.

More formally, you want to choose four integers `i`, `j`, `k`, `l` such that `0 <= i <= j < k <= l < s.length` and both the substrings `s[i...j]` and `s[k...l]` are palindromes and have odd lengths. `s[i...j]` denotes a substring from index `i` to index `j` __inclusive__.

Return the maximum possible product of the lengths of the two non-intersecting palindromic substrings.

A palindrome is a string that is the same forward and backward. A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: s = "ababbb"
Output: 9
Explanation: Substrings "aba" and "bbb" are palindromes with odd length. product = 3 * 3 = 9.
```

Example 2:

```text
Input: s = "zaaaxbbby"
Output: 9
Explanation: Substrings "aaa" and "bbb" are palindromes with odd length. product = 3 * 3 = 9.
```

__Constraints:__

- `2 <= s.length <= 10 ^ 5`
- `s` consists of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `s` ，你需要找到两个 __不重叠____的回文__ 子字符串，它们的长度都必须为 __奇数__ ，使得它们长度的乘积最大。

更正式地，你想要选择四个整数 `i` ， `j` ， `k` ， `l` ，使得 `0 <= i <= j < k <= l < s.length` ，且子字符串 `s[i...j]` 和 `s[k...l]` 都是回文串且长度为奇数。 `s[i...j]` 表示下标从 `i` 到 `j` 且 __包含__ 两端下标的子字符串。

请你返回两个不重叠回文子字符串长度的 最大 乘积。

回文字符串 指的是一个从前往后读和从后往前读一模一样的字符串。子字符串 指的是一个字符串中一段连续字符。

__示例:__

示例 1：

```text
输入：s = "ababbb"
输出：9
解释：子字符串 "aba" 和 "bbb" 为奇数长度的回文串。乘积为 3 * 3 = 9 。
```

示例 2：

```text
输入：s = "zaaaxbbby"
输出：9
解释：子字符串 "aaa" 和 "bbb" 为奇数长度的回文串。乘积为 3 * 3 = 9 。
```

__提示：__

- `2 <= s.length <= 10 ^ 5`
- `s` 只包含小写英文字母。

__思路:__

```text
马拉车 ➕ 前缀和 ➕ 后缀和
由于只需要计算奇数长度的回文子串, 因此可以使用马拉车算法计算出每个位置的回文半径, 不需要添加特殊字符
span[i] 表示以 i 为中心的回文子串的半径, 即 s[i] 对应的回文串的长度为 span[i] * 2 - 1
用 pre[i] 表示以 s[:i] 的回文子串的最大长度, suf[i] 表示以 s[i:] 的回文子串的最大长度
最后枚举 i, 计算 pre[i] * suf[i + 1] 的最大值即可
由于 pre[i + span[i] - 1] 最大为 span[i] * 2 - 1, 先将 pre[i + span[i] - 1] 更新为 max(pre[i + span[i] - 1], span[i] * 2 - 1)
suf[i - span[i] + 1] 同理
然后枚举 i, 将 pre[i] 更新为 max(pre[i], pre[i - 1]), suf[i] 更新为 max(suf[i], suf[i - 1] - 2)
前者是由于 pre[i] 需要取奇数长度的回文子串, 后者是由于对 suf[i - 1] 来说, 去掉头尾两个字符后, 仍然是 suf[i] 的子串
最后枚举 i, 将 pre[i] 更新为 max(pre[i], pre[i + 1] - 2), suf[i] 更新为 max(suf[i], suf[i + 1]), 与上面同理
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long maxProduct(string s) 
    {
        int n = s.size(), l = 0, r = -1;
        vector<int> span(n), pre(n), suf(n);
        for (int i = 0; i < n; i++) 
        {
            span[i] = (i <= r ? min(span[l + r - i], r - i + 1) : 1);
            while (i - span[i] > -1 and i + span[i] < n and s[i - span[i]] == s[i + span[i]]) ++span[i];
            if (i + span[i] - 1 > r) 
            {
                l = i - span[i] + 1;
                r = i + span[i] - 1;
            }
        }
        for (int i = 0; i < n; i++) 
        {
            pre[i + span[i] - 1] = max(pre[i + span[i] - 1], span[i] * 2 - 1);
            suf[i - span[i] + 1] = max(suf[i - span[i] + 1], span[i] * 2 - 1);
        }
        for (int i = 1; i < n; i++) 
        {
            pre[i] = max(pre[i], pre[i - 1]);
            suf[i] = max(suf[i], suf[i - 1] - 2);
        }
        for (int i = n - 2; i > -1; i--) 
        {
            pre[i] = max(pre[i], pre[i + 1] - 2);
            suf[i] = max(suf[i], suf[i + 1]);
        }
        long long result = 0;
        for (int i = 0; i < n - 1; i++) result = max(result, (long long)pre[i] * suf[i + 1]);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long maxProduct(String s) {
        int n = s.length(), span[] = new int[n], l = 0, r = -1, pre[] = new int[n], suf[] = new int[n];
        for (int i = 0; i < n; i++) {
            span[i] = i <= r ? Math.min(span[l + r - i], r - i + 1) : 1;
            while (i - span[i] > -1 && i + span[i] < n && s.charAt(i - span[i]) == s.charAt(i + span[i])) ++span[i];
            if (i + span[i] - 1 > r) {
                l = i - span[i] + 1;
                r = i + span[i] - 1;
            }
        }
        for (int i = 0; i < n; i++) {
            pre[i + span[i] - 1] = Math.max(pre[i + span[i] - 1], span[i] * 2 - 1);
            suf[i - span[i] + 1] = Math.max(suf[i - span[i] + 1], span[i] * 2 - 1);
        }
        for (int i = 1; i < n; i++) {
            pre[i] = Math.max(pre[i], pre[i - 1]);
            suf[i] = Math.max(suf[i], suf[i - 1] - 2);
        }
        for (int i = n - 2; i > -1; i--) {
            pre[i] = Math.max(pre[i], pre[i + 1] - 2);
            suf[i] = Math.max(suf[i], suf[i + 1]);
        }
        long result = 0;
        for (int i = 0; i < n - 1; i++) result = Math.max(result, (long)pre[i] * suf[i + 1]);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maxProduct(self, s: str) -> int:
        span, l, r, pre, suf = [0] * (n := len(s)), 0, -1, [0] * n, [0] * n
        for i in range(n):
            span[i] = (min(span[l + r - i], r - i + 1) if i <= r else 1)
            while i - span[i] > -1 and i + span[i] < n and s[i - span[i]] == s[i + span[i]]:
                span[i] += 1
            if i + span[i] - 1 > r:
                l, r = i - span[i] + 1, i + span[i] - 1
        for i in range(n):
            pre[i + span[i] - 1], suf[i - span[i] + 1] = max(pre[i + span[i] - 1], span[i] * 2 - 1), max(suf[i - span[i] + 1], span[i] * 2 - 1)
        for i in range(1, n):
            pre[i], suf[i] = max(pre[i], pre[i - 1]), max(suf[i], suf[i - 1] - 2)
        for i in range(n - 2, -1, -1):
            pre[i], suf[i] = max(pre[i], pre[i + 1] - 2), max(suf[i], suf[i + 1])
        return max(pre[i] * suf[i + 1] for i in range(n - 1))
```
