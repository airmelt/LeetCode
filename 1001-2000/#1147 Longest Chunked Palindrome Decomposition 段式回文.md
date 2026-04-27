# 1147 Longest Chunked Palindrome Decomposition 段式回文

__Description__:
You are given a string text. You should split it to k substrings (subtext1, subtext2, ..., subtextk) such that:

subtexti is a non-empty string.
The concatenation of all the substrings is equal to text (i.e., subtext1 + subtext2 + ... + subtextk == text).
subtexti == subtextk - i + 1 for all valid values of i (i.e., 1 <= i <= k).
Return the largest possible value of k.

__Example:__

Example 1:

Input: text = "ghiabcdefhelloadamhelloabcdefghi"
Output: 7
Explanation: We can split the string on "(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)".

Example 2:

Input: text = "merchant"
Output: 1
Explanation: We can split the string on "(merchant)".

Example 3:

Input: text = "antaprezatepzapreanta"
Output: 11
Explanation: We can split the string on "(a)(nt)(a)(pre)(za)(tpe)(za)(pre)(a)(nt)(a)".

__Constraints:__

1 <= text.length <= 1000
text consists only of lowercase English characters.

__题目描述__:
你会得到一个字符串 text 。你应该把它分成 k 个子字符串 (subtext1, subtext2，…， subtextk) ，要求满足:

subtexti 是非空字符串
所有子字符串的连接等于 text ( 即subtext1 + subtext2 + ... + subtextk == text )
subtexti == subtextk - i + 1 表示所有 i 的有效值( 即 1 <= i <= k )
返回k可能最大值。

__示例 :__

示例 1：

输入：text = "ghiabcdefhelloadamhelloabcdefghi"
输出：7
解释：我们可以把字符串拆分成 "(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)"。

示例 2：

输入：text = "merchant"
输出：1
解释：我们可以把字符串拆分成 "(merchant)"。

示例 3：

输入：text = "antaprezatepzapreanta"
输出：11
解释：我们可以把字符串拆分成 "(a)(nt)(a)(pre)(za)(tpe)(za)(pre)(a)(nt)(a)"。

示例 4：

输入：text = "aaa"
输出：3
解释：我们可以把字符串拆分成 "(a)(a)(a)"。

__提示:__

1 <= text.length <= 1000
text 仅由小写英文字符组成

__思路__:

贪心
能将字符串前后分开立即分开
这样就能获得最大的段落数
取中间的字符串递归求解, 结果加上 2, 表示 2 段
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestDecomposition(string text) 
    {
        for (int i = 1, n = text.size(); i <= (n >> 1); i++) if (text.substr(0, i) == text.substr(n - i, i)) return longestDecomposition(text.substr(i, n - (i << 1))) + 2;
        return min(1, int(text.size()));
    }
};
```

__Java__:

```Java
class Solution {
    public int longestDecomposition(String text) {
        for (int i = 1, n = text.length(); i <= (n >> 1); i++) if (text.substring(0, i).equals(text.substring(n - i, n))) return longestDecomposition(text.substring(i, n - i)) + 2;
        return Math.min(1, text.length());
    }
}
```

__Python__:

```Python
class Solution:
    def longestDecomposition(self, text: str) -> int:
        for i in range(1, (len(text) >> 1) + 1):
            if text[:i] == text[-i:]:
                return self.longestDecomposition(text[i:-i]) + 2
        return min(1, len(text))
```
