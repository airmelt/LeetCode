# 1839 Longest Substring Of All Vowels in Order 所有元音按顺序排布的最长子字符串

__Description:__

A string is considered beautiful if it satisfies the following conditions:

- Each of the 5 English vowels ( `'a'`, `'e'`, `'i'`, `'o'`, `'u'`) must appear __at least once__ in it.
- The letters must be sorted in __alphabetical order__ (i.e. all `'a'`s before `'e'`s, all `'e'`s before `'i'`s, etc.).

For example, strings `"aeiou"` and `"aaaaaaeiiiioou"` are considered __beautiful__, but `"uaeio"`, `"aeoiu"`, and `"aaaeeeooo"` are __not beautiful__.

Given a string `word` consisting of English vowels, return _the __length of the longest beautiful substring__ of_ `word`_. If no such substring exists, return_ `0`.

A substring is a contiguous sequence of characters in a string.

__Example:__

Example 1:

```text
Input: word = "aeiaaioaaaaeiiiiouuuooaauuaeiu"
Output: 13
Explanation: The longest beautiful substring in word is "aaaaeiiiiouuu" of length 13.
```

Example 2:

```text
Input: word = "aeeeiiiioooauuuaeiou"
Output: 5
Explanation: The longest beautiful substring in word is "aeiou" of length 5.
```

Example 3:

```text
Input: word = "a"
Output: 0
Explanation: There is no beautiful substring, so return 0.
```

__Constraints:__

- `1 <= word.length <= 5 * 10 ^ 5`
- `word` consists of characters `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

__题目描述:__

当一个字符串满足如下条件时，我们称它是 美丽的 ：

- 所有 5 个英文元音字母（ `'a'` ， `'e'` ， `'i'` ， `'o'` ， `'u'`）都必须 __至少__ 出现一次。
- 这些元音字母的顺序都必须按照 __字典序__ 升序排布（也就是说所有的 `'a'` 都在 `'e'` 前面，所有的 `'e'` 都在 `'i'` 前面，以此类推）

比方说，字符串 `"aeiou"` 和 `"aaaaaaeiiiioou"` 都是 __美丽的__ ，但是 `"uaeio"` ， `"aeoiu"` 和 `"aaaeeeooo"` __不是美丽的__ 。

给你一个只包含英文元音字母的字符串 `word` ，请你返回 `word` 中 __最长美丽子字符串的长度__ 。如果不存在这样的子字符串，请返回 `0` 。

子字符串 是字符串中一个连续的字符序列。

__示例:__

示例 1：

```text
输入：word = "aeiaaioaaaaeiiiiouuuooaauuaeiu"
输出：13
解释：最长子字符串是 "aaaaeiiiiouuu" ，长度为 13 。
```

示例 2：

```text
输入：word = "aeeeiiiioooauuuaeiou"
输出：5
解释：最长子字符串是 "aeiou" ，长度为 5 。
```

示例 3：

```text
输入：word = "a"
输出：0
解释：没有美丽子字符串，所以返回 0 。
```

__提示：__

- `1 <= word.length <= 5 * 10 ^ 5`
- `word` 只包含字符 `'a'`， `'e'`， `'i'`， `'o'` 和 `'u'` 。

__思路:__

```text
滑动窗口
从第一个为 'a' 的字符开始
找到连续的字符，直到不满足字典序
如果这个区间内的字符的种类满足 "aeiou" 则更新结果
否则窗口向右移动
考虑到字符种类只有 5(26) 种，可以用一个 int 来表示
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestBeautifulSubstring(string word) 
    {
        int left = 0, right = 0, result = 0, n = word.size();
        while (right < n)
        {
            while (left < n and word[left] != 'a') ++left;
            right = left + 1;
            while (right < n - 1 and word[right] <= word[right + 1]) ++right;
            int vowels = 0;
            for (int i = left, m = min(right + 1, n); i < m; i++) vowels |= 1 << (word[i] - 'a');
            if (vowels == 0x104111) result = max(result, right - left + 1);
            left = right + 1;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestBeautifulSubstring(String word) {
        int left = 0, right = 0, result = 0, n = word.length();
        while (right < n) {
            while (left < n && word.charAt(left) != 'a') ++left;
            boolean[] vowels = new boolean[26];
            if (left < n - 4) vowels[0] = true;
            right = left + 1;
            while (right < n - 1 && word.charAt(right) <= word.charAt(right + 1)) vowels[word.charAt(right++) - 'a'] = true;
            if (right < n) vowels[word.charAt(right) - 'a'] = true;
            if (vowels[0] && vowels[4] && vowels[8] && vowels[14] && vowels[20]) result = Math.max(result, right - left + 1);
            left = right + 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestBeautifulSubstring(self, word: str) -> int:
        left, right, result, n, target = 0, 0, 0, len(word), set(list('aeiou'))
        while right < n:
            while left < n and word[left] != "a":
                left += 1
            right = left + 1
            while right < n - 1 and word[right] <= word[right + 1]:
                right += 1
            if set(word[left:right + 1]) == target:
                result = max(result, right - left + 1)
            left = right + 1
        return result
```
