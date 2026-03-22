# 1415 The k-th Lexicographical String of All Happy Strings of Length n 长度为n的开心字符串中字典序第k小的字符串

__Description:__

A happy string is a string that:

- consists only of letters of the set `['a', 'b', 'c']`.
- `s[i] != s[i + 1]` for all values of `i` from `1` to `s.length - 1` (string is 1-indexed).

For example, strings "abc", "ac", "b" and "abcbabcbcb" are all happy strings and strings "aa", "baa" and "ababbc" are not happy strings.

Given two integers `n` and `k`, consider a list of all happy strings of length `n` sorted in lexicographical order.

Return _the kth string_ of this list or return an __empty string__ if there are less than `k` happy strings of length `n`.

__Example:__

Example 1:

```text
Input: n = 1, k = 3
Output: "c"
Explanation: The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".
```

Example 2:

```text
Input: n = 1, k = 4
Output: ""
Explanation: There are only 3 happy strings of length 1.
```

Example 3:

```text
Input: n = 3, k = 9
Output: "cab"
Explanation: There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9th string = "cab"
```

__Constraints:__

- `1 <= n <= 10`
- `1 <= k <= 100`

__题目描述:__

一个 「开心字符串」定义为：

- 仅包含小写字母 `['a', 'b', 'c']`.
- 对所有在 `1` 到 `s.length - 1` 之间的 `i` ，满足 `s[i] != s[i + 1]` （字符串的下标从 1 开始）。

比方说，字符串 "abc"，"ac"，"b" 和 "abcbabcbcb" 都是开心字符串，但是 "aa"，"baa" 和 "ababbc" 都不是开心字符串。

给你两个整数 `n` 和 `k` ，你需要将长度为 `n` 的所有开心字符串按字典序排序。

请你返回排序后的第 k 个开心字符串，如果长度为 `n` 的开心字符串少于 `k` 个，那么请你返回 __空字符串__ 。

__示例:__

示例 1：

```text
输入：n = 1, k = 3
输出："c"
解释：列表 ["a", "b", "c"] 包含了所有长度为 1 的开心字符串。按照字典序排序后第三个字符串为 "c" 。
```

示例 2：

```text
输入：n = 1, k = 4
输出：""
解释：长度为 1 的开心字符串只有 3 个。
```

示例 3：

```text
输入：n = 3, k = 9
输出："cab"
解释：长度为 3 的开心字符串总共有 12 个 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"] 。第 9 个字符串为 "cab"
```

示例 4：

```text
输入：n = 2, k = 7
输出：""
```

示例 5：

```text
输入：n = 10, k = 100
输出："abacbabacb"
```

__提示：__

- `1 <= n <= 10`
- `1 <= k <= 100`

__思路:__

```text
贪心
先检查 k 是否有效
比较 k 和 3 * 2 ^ n 的大小关系即可
结果的第 i 位由 k / (2 ^ i) 决定
第一位为 k / (2 ^ i) + 'a'
对接下来的每一位都不能与前一位相同
所以分成 'a', 'b', 'c' 讨论
如果 k / (2 ^ i) == 0 说明接下来应该选择较小的字符
否则应该选择较大的字符
大小由字典序决定
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string getHappyString(int n, int k) 
    {
        string result = "";
        if (k-- > 3 * (1 << (n - 1))) return result;
        for (int i = n; i > 0; i--)
        {
            int count = 1 << (i - 1), cur = k / count;
            k %= count;
            result += (i == n ? ('a' + cur) : result.back() == 'a' ? (cur ? 'c' : 'b') : result.back() == 'b' ? (cur ? 'c' : 'a') : (cur ? 'b' : 'a'));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String getHappyString(int n, int k) {
        StringBuilder sb = new StringBuilder();
        if (k-- > 3 * (1 << (n - 1))) return sb.toString();
        for (int i = n; i > 0; i--) {
            int count = 1 << (i - 1), cur = k / count;
            k %= count;
            sb.append(i == n ? (char)('a' + cur) : (sb.charAt(sb.length() - 1) == 'a' ? (cur == 0 ? 'b' : 'c') : (sb.charAt(sb.length() - 1) == 'b' ? (cur == 0 ? 'a' : 'c') : (cur == 0 ? 'a' : 'b'))));
        }
        return sb.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        result = deque()
        if k > 3 * (1 << (n - 1)):
            return ''.join(result)
        k -= 1
        for i in range(n, 0, -1):
            cur, k = (k // (count := 1 << (i - 1))), k % count
            result.append(chr(ord('a') + cur) if i == n else 'a' if ((result[-1] == 'b' and not cur) or (result[-1] == 'c' and not cur)) else 'b' if ((result[-1] == 'a' and not cur) or (result[-1] == 'c' and cur)) else 'c')
        return ''.join(result)
```
