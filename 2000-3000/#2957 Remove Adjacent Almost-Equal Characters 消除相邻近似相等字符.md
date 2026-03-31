# 2957 Remove Adjacent Almost-Equal Characters 消除相邻近似相等字符

__Description:__

You are given a __0-indexed__ string `word`.

In one operation, you can pick any index `i` of `word` and change `word[i]` to any lowercase English letter.

Return _the __minimum__ number of operations needed to remove all adjacent __almost-equal__ characters from_ `word`.

Two characters `a` and `b` are __almost-equal__ if `a == b` or `a` and `b` are adjacent in the alphabet.

__Example:__

Example 1:

```text
Input: word = "aaaaa"
Output: 2
Explanation: We can change word into "acaca" which does not have any adjacent almost-equal characters.
It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.
```

Example 2:

```text
Input: word = "abddez"
Output: 2
Explanation: We can change word into "ybdoez" which does not have any adjacent almost-equal characters.
It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 2.
```

Example 3:

```text
Input: word = "zyxyxyz"
Output: 3
Explanation: We can change word into "zaxaxaz" which does not have any adjacent almost-equal characters. 
It can be shown that the minimum number of operations needed to remove all adjacent almost-equal characters from word is 3.
```

__Constraints:__

- `1 <= word.length <= 100`
- `word` consists only of lowercase English letters.

__题目描述:__

给你一个下标从 __0__ 开始的字符串 `word` 。

一次操作中，你可以选择 `word` 中任意一个下标 `i` ，将 `word[i]` 修改成任意一个小写英文字母。

请你返回消除 `word` 中所有相邻 __近似相等__ 字符的 __最少__ 操作次数。

两个字符 `a` 和 `b` 如果满足 `a == b` 或者 `a` 和 `b` 在字母表中是相邻的，那么我们称它们是 __近似相等__ 字符。

__示例:__

示例 1：

```text
输入：word = "aaaaa"
输出：2
解释：我们将 word 变为 "acaca" ，该字符串没有相邻近似相等字符。
消除 word 中所有相邻近似相等字符最少需要 2 次操作。
```

示例 2：

```text
输入：word = "abddez"
输出：2
解释：我们将 word 变为 "ybdoez" ，该字符串没有相邻近似相等字符。
消除 word 中所有相邻近似相等字符最少需要 2 次操作。
```

示例 3：

```text
输入：word = "zyxyxyz"
输出：3
解释：我们将 word 变为 "zaxaxaz" ，该字符串没有相邻近似相等字符。
消除 word 中所有相邻近似相等字符最少需要 3 次操作
```

__提示：__

- `1 <= word.length <= 100`
- `word` 只包含小写英文字母。

__思路:__

```text
贪心
比较 i 和 i - 1 是否绝对值不超过 1
如果是, 结果累加并跳过下一个
因为总可以修改 i 为相邻两个字符都不近似相等的字符
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int removeAlmostEqualCharacters(string word) 
    {
        int result = 0;
        for (int i = 1, n = word.size(); i < n; i++) 
        {
            if (abs(word[i - 1] - word[i]) <= 1) 
            {
                ++result;
                ++i;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int removeAlmostEqualCharacters(String word) {
        int result = 0;
        for (int i = 1, n = word.length(); i < n; i++) {
            if (Math.abs(word.charAt(i - 1) - word.charAt(i)) <= 1) {
                ++result;
                ++i;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def removeAlmostEqualCharacters(self, word: str) -> int:
        result, i, n = 0, 1, len(word)
        while i < n:
            if abs(ord(word[i - 1]) - ord(word[i])) <= 1:
                result += 1
                i += 1
            i += 1
        return result
```
