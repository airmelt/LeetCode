# 2063 Vowels of All Substrings 所有子字符串中的元音

__Description:__

Given a string `word`, return _the __sum of the number of vowels__ (_`'a'`, `'e'`_,_ `'i'`_,_ `'o'`_, and_ `'u'`_)_ _in every substring of_ `word`.

A substring is a contiguous (non-empty) sequence of characters within a string.

Note: Due to the large constraints, the answer may not fit in a signed 32-bit integer. Please be careful during the calculations.

__Example:__

Example 1:

```text
Input: word = "aba"
Output: 6
Explanation: 
All possible substrings are: "a", "ab", "aba", "b", "ba", and "a".
```

- "b" has 0 vowels in it
- "a", "ab", "ba", and "a" have 1 vowel each
- "aba" has 2 vowels in it

Hence, the total sum of vowels = 0 + 1 + 1 + 1 + 1 + 2 = 6.

Example 2:

```text
Input: word = "abc"
Output: 3
Explanation: 
All possible substrings are: "a", "ab", "abc", "b", "bc", and "c".
```

- "a", "ab", and "abc" have 1 vowel each
- "b", "bc", and "c" have 0 vowels each

Hence, the total sum of vowels = 1 + 1 + 1 + 0 + 0 + 0 = 3.

Example 3:

```text
Input: word = "ltcd"
Output: 0
Explanation: There are no vowels in any substring of "ltcd".
```

__Constraints:__

- `1 <= word.length <= 10 ^ 5`
- `word` consists of lowercase English letters.

__题目描述:__

给你一个字符串 `word` ，返回 `word` 的所有子字符串中 __元音的总数__ ，元音是指 `'a'`、 `'e'`_、_`'i'`_、_`'o'` 和 `'u'` _。_

子字符串 是字符串中一个连续（非空）的字符序列。

__注意:__由于对 `word` 长度的限制比较宽松，答案可能超过有符号 32 位整数的范围。计算时需当心。

__示例:__

示例 1：

```text
输入：word = "aba"
输出：6
解释：
所有子字符串是："a"、"ab"、"aba"、"b"、"ba" 和 "a" 。
```

- "b" 中有 0 个元音
- "a"、"ab"、"ba" 和 "a" 每个都有 1 个元音
- "aba" 中有 2 个元音

因此，元音总数 = 0 + 1 + 1 + 1 + 1 + 2 = 6 。

示例 2：

```text
输入：word = "abc"
输出：3
解释：
所有子字符串是："a"、"ab"、"abc"、"b"、"bc" 和 "c" 。
```

- "a"、"ab" 和 "abc" 每个都有 1 个元音
- "b"、"bc" 和 "c" 每个都有 0 个元音

因此，元音总数 = 1 + 1 + 1 + 0 + 0 + 0 = 3 。

示例 3：

```text
输入：word = "ltcd"
输出：0
解释："ltcd" 的子字符串均不含元音。
```

示例 4：

```text
输入：word = "noosabasboosa"
输出：237
解释：所有子字符串中共有 237 个元音。
```

__提示：__

- `1 <= word.length <= 10 ^ 5`
- `word` 由小写英文字母组成

__思路:__

```text
动态规划
考虑每个元音字母
包括该元音
该元音字母左边的子字符串选择有 i + 1 个, 0, 1, 2, ..., i
该元音字母右边的子字符串选择有 n - i 个, i, i + 1, ..., n - 1
共计 (i + 1) * (n - i) 个子字符串
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long countVowels(string word) 
    {
        long long result = 0;
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
        for (int i = 0, n = word.size(); i < n; i++) if (vowels.count(word[i])) result += (i + 1LL) * (n - i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long countVowels(String word) {
        long result = 0;
        for (int i = 0, n = word.length(); i < n; i++) if ("aeiou".indexOf(word.charAt(i)) != -1) result += (i + 1L) * (n - i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countVowels(self, word: str) -> int:
        return sum((c in {"a", "e", "i", "o", "u"}) * (i + 1) * (len(word) - i) for i, c in enumerate(word))
```
