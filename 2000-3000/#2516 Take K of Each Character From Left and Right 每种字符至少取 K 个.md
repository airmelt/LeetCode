# 2516 Take K of Each Character From Left and Right 每种字符至少取 K 个

__Description:__

You are given a string `s` consisting of the characters `'a'`, `'b'`, and `'c'` and a non-negative integer `k`. Each minute, you may take either the __leftmost__ character of `s`, or the __rightmost__ character of `s`.

Return _the __minimum__ number of minutes needed for you to take __at least___ `k` _of each character, or return_ `-1` _if it is not possible to take_ `k` _of each character._

__Example:__

Example 1:

```text
Input: s = "aabaaaacaabc", k = 2
Output: 8
Explanation: 
Take three characters from the left of s. You now have two 'a' characters, and one 'b' character.
Take five characters from the right of s. You now have four 'a' characters, two 'b' characters, and two 'c' characters.
A total of 3 + 5 = 8 minutes is needed.
It can be proven that 8 is the minimum number of minutes needed.
```

Example 2:

```text
Input: s = "a", k = 1
Output: -1
Explanation: It is not possible to take one 'b' or 'c' so return -1.
```

__Constraints:__

- `1 <= s.length <= 10 ^ 5`
- `s` consists of only the letters `'a'`, `'b'`, and `'c'`.
- `0 <= k <= s.length`

__题目描述:__

给你一个由字符 `'a'`、 `'b'`、 `'c'` 组成的字符串 `s` 和一个非负整数 `k` 。每分钟，你可以选择取走 `s` __最左侧__ 还是 __最右侧__ 的那个字符。

你必须取走每种字符 __至少__ `k` 个，返回需要的 __最少__ 分钟数；如果无法取到，则返回 `-1` 。

__示例:__

示例 1：

```text
输入：s = "aabaaaacaabc", k = 2
输出：8
解释：
从 s 的左侧取三个字符，现在共取到两个字符 'a' 、一个字符 'b' 。
从 s 的右侧取五个字符，现在共取到四个字符 'a' 、两个字符 'b' 和两个字符 'c' 。
共需要 3 + 5 = 8 分钟。
可以证明需要的最少分钟数是 8 。
```

示例 2：

```text
输入：s = "a", k = 1
输出：-1
解释：无法取到一个字符 'b' 或者 'c'，所以返回 -1 。
```

__提示：__

- `1 <= s.length <= 10 ^ 5`
- `s` 仅由字母 `'a'`、 `'b'`、 `'c'` 组成
- `0 <= k <= s.length`

__思路:__

```text
滑动窗口
设 s 中有 a 个 'a', b 个 'b', c 个 'c'
转化为求最长的窗口
使得窗口中有 k - a 个 'a', k - b 个 'b' 和 k - c 个 'c'
先计算 s 中字符的个数
如果有任何字符的个数小于 k 直接返回 -1
直接维护窗口外的字符数
移动右端点的同时减去窗口对应的字符
当窗口外的字符数小于 k 时移动左端点并加入窗口
记录最长的窗口即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int takeCharacters(string s, int k) 
    {
        int d[3]{0}, result = 0, n = s.size(), left = 0;
        for (const auto& c : s) ++d[c - 'a'];
        for (const auto& i : d) if (i < k) return -1;
        for (int right = 0; right < n; right++) 
        {
            --d[s[right] - 'a'];
            while (d[s[right] - 'a'] < k) ++d[s[left++] - 'a'];
            result = max(result, right - left + 1);
        }
        return n - result;
    }
};
```

__Java__:

```Java
class Solution {
    public int takeCharacters(String s, int k) {
        int d[] = new int[3], result = 0, n = s.length(), left = 0;
        for (char c : s.toCharArray()) ++d[c - 'a'];
        for (int i : d) if (i < k) return -1;
        for (int right = 0; right < n; right++) {
            --d[s.charAt(right) - 'a'];
            while (d[s.charAt(right) - 'a'] < k) ++d[s.charAt(left++) - 'a'];
            result = Math.max(result, right - left + 1);
        }
        return n - result;
    }
}
```

__Python__:

```Python
class Solution:
    def takeCharacters(self, s: str, k: int) -> int:
        d, result, left, n = Counter(s), 0, 0, len(s)
        if any(d[c] < k for c in 'abc'):
            return -1
        for right, c in enumerate(s):
            d[c] -= 1
            while d[c] < k:
                d[s[left]] += 1
                left += 1
            result = max(result, right - left + 1)
        return n - result
```
