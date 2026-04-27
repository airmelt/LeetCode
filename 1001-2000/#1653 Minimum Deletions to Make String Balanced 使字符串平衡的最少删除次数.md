# 1653 Minimum Deletions to Make String Balanced 使字符串平衡的最少删除次数

__Description:__

You are given a string `s` consisting only of characters `'a'` and `'b'`​​​​.

You can delete any number of characters in `s` to make `s` __balanced__. `s` is __balanced__ if there is no pair of indices `(i,j)` such that `i < j` and `s[i] = 'b'` and `s[j]= 'a'`.

Return _the __minimum__ number of deletions needed to make_ `s` ___balanced___.

__Example:__

Example 1:

```text
Input: s = "aababbab"
Output: 2
Explanation: You can either:
Delete the characters at 0-indexed positions 2 and 6 ("aababbab" -> "aaabbb"), or
Delete the characters at 0-indexed positions 3 and 6 ("aababbab" -> "aabbbb").
```

Example 2:

```text
Input: s = "bbaaaaabb"
Output: 2
Explanation: The only solution is to delete the first two characters.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` is `'a'` or `'b'`​​.

__题目描述:__

给你一个字符串 `s` ，它仅包含字符 `'a'` 和 `'b'`​​​​ 。

你可以删除 `s` 中任意数目的字符，使得 `s` __平衡__ 。当不存在下标对 `(i,j)` 满足 `i < j` ，且 `s[i] = 'b'` 的同时 `s[j]= 'a'` ，此时认为 `s` 是 __平衡__ 的。

请你返回使 `s` __平衡__ 的 __最少__ 删除次数。

__示例:__

示例 1：

```text
输入：s = "aababbab"
输出：2
解释：你可以选择以下任意一种方案：
下标从 0 开始，删除第 2 和第 6 个字符（"aababbab" -> "aaabbb"），
下标从 0 开始，删除第 3 和第 6 个字符（"aababbab" -> "aabbbb"）。
```

示例 2：

```text
输入：s = "bbaaaaabb"
输出：2
解释：唯一的最优解是删除最前面两个字符。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s[i]` 要么是 `'a'` 要么是 `'b'`​ 。​

__思路:__

```text
动态规划
dp[i] 表示删除 s[i] 之前的字符，使得 s[0:i] 平衡的最少删除次数
当前字符为 'a' 时, dp[i] = min(dp[i - 1] + 1, b), 即要么把当前字符删除, 要么把之前所有的 'b' 删除
当前字符为 'b' 时, 由于 s 已经平衡, 所以 dp[i] = dp[i - 1]
由于 dp[i] 只与 dp[i - 1] 有关, 所以可以用一个变量 dp 代替 dp[i]
在遇到 'b' 时, 统计个数,
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumDeletions(string s) 
    {
        int b = 0, dp = 0;
        for (const auto& c : s) 
        {
            if (c == 'a') dp = min(dp + 1, b);
            else ++b;
        }
        return dp;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumDeletions(String s) {
        int b = 0, dp = 0;
        for (char c : s.toCharArray()) {
            if (c == 'a') dp = Math.min(b, dp + 1);
            else ++b;
        }
        return dp;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumDeletions(self, s: str) -> int:
        return min(a + b for a, b in zip(a, b)) if (a := list(accumulate(s[i] == 'a' for i in range(len(s) - 1, -1, -1)))[::-1] + [0]) and (b := [0] + list(accumulate(s[i] == 'b' for i in range(len(s))))) else 0
```
