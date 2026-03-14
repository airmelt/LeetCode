# 2937 Make Three Strings Equal 使三个字符串相等

__Description:__

You are given three strings: `s1`, `s2`, and `s3`. In one operation you can choose one of these strings and delete its __rightmost__ character. Note that you __cannot__ completely empty a string.

Return the _minimum number of operations_ required to make the strings equal_._ If it is impossible to make them equal, return `-1`.

__Example:__

Example 1:

```text
Input: s1 = "abc", s2 = "abb", s3 = "ab"

Output: 2
```

__Explanation:__ Deleting the rightmost character from both `s1` and `s2` will result in three equal strings.

Example 2:

```text
Input: s1 = "dac", s2 = "bac", s3 = "cac"

Output: -1
```

__Explanation:__ Since the first letters of `s1` and `s2` differ, they cannot be made equal.

__Constraints:__

- `1 <= s1.length, s2.length, s3.length <= 100`
- `s1`, `s2` and `s3` consist only of lowercase English letters.

__题目描述:__

给你三个字符串 `s1`、 `s2` 和 `s3`。 你可以根据需要对这三个字符串执行以下操作 __任意次数__ 。

在每次操作中，你可以选择其中一个长度至少为 `2` 的字符串 并删除其 __最右位置上__ 的字符。

如果存在某种方法能够使这三个字符串相等，请返回使它们相等所需的 __最小__ 操作次数；否则，返回 `-1`。

__示例:__

示例 1：

```text
输入：s1 = "abc"，s2 = "abb"，s3 = "ab"
输出：2
解释：对 s1 和 s2 进行一次操作后，可以得到三个相等的字符串。
可以证明，不可能用少于两次操作使它们相等。
```

示例 2：

```text
输入：s1 = "dac"，s2 = "bac"，s3 = "cac"
输出：-1
解释：因为 s1 和 s2 的最左位置上的字母不相等，所以无论进行多少次操作，它们都不可能相等。因此答案是 -1 。
```

__提示：__

- `1 <= s1.length, s2.length, s3.length <= 100`
- `s1`、 `s2` 和 `s3` 仅由小写英文字母组成。

__思路:__

```text
模拟
求出三个字符串的公共前缀
如果没有公共前缀就返回 -1
否则返回三个字符串的长度减去公共前缀长度的 3 倍
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findMinimumOperations(string s1, string s2, string s3) 
    {
        int a = s1.size(), b = s2.size(), c = s3.size(), lcp = 0;
        for (int n = min({ a, b, c }), i = 0; i < n; i++, lcp++) if (s1[i] != s2[i] or s2[i] != s3[i]) break;
        return lcp == 0 ? -1 : a + b + c - lcp * 3;
    }
};
```

__Java__:

```Java
class Solution {
    public int findMinimumOperations(String s1, String s2, String s3) {
        int a = s1.length(), b = s2.length(), c = s3.length(), lcp = 0;
        for (int n = Math.min(a, Math.min(b, c)), i = 0; i < n; i++, lcp++) if (s1.charAt(i) != s2.charAt(i) || s2.charAt(i) != s3.charAt(i)) break;
        return lcp == 0 ? -1 : a + b + c - lcp * 3;
    }
}
```

__Python__:

```Python
class Solution:
    def findMinimumOperations(self, s1: str, s2: str, s3: str) -> int:
        lcp = 0
        for x, y, z in zip(s1, s2, s3):
            if x != y or x != z:
                break
            lcp += 1
        return -1 if not lcp else len(s1) + len(s2) + len(s3) - lcp * 3
```
