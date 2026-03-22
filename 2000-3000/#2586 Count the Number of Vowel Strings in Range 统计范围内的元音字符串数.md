# 2586 Count the Number of Vowel Strings in Range 统计范围内的元音字符串数

__Description:__

You are given a __0-indexed__ array of string `words` and two integers `left` and `right`.

A string is called a __vowel string__ if it starts with a vowel character and ends with a vowel character where vowel characters are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

Return _the number of vowel strings_ `words[i]` _where_ `i` _belongs to the inclusive range_ `[left, right]`.

__Example:__

Example 1:

```text
Input: words = ["are","amy","u"], left = 0, right = 2
Output: 2
Explanation: 
```

- "are" is a vowel string because it starts with 'a' and ends with 'e'.
- "amy" is not a vowel string because it does not end with a vowel.
- "u" is a vowel string because it starts with 'u' and ends with 'u'.

The number of vowel strings in the mentioned range is 2.

Example 2:

```text
Input: words = ["hey","aeo","mu","ooo","artro"], left = 1, right = 4
Output: 3
Explanation:
```

- "aeo" is a vowel string because it starts with 'a' and ends with 'o'.
- "mu" is not a vowel string because it does not start with a vowel.
- "ooo" is a vowel string because it starts with 'o' and ends with 'o'.
- "artro" is a vowel string because it starts with 'a' and ends with 'o'.

The number of vowel strings in the mentioned range is 3.

__Constraints:__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 10`
- `words[i]` consists of only lowercase English letters.
- `0 <= left <= right < words.length`

__题目描述:__

给你一个下标从 __0__ 开始的字符串数组 `words` 和两个整数: `left` 和 `right` 。

如果字符串以元音字母开头并以元音字母结尾，那么该字符串就是一个 __元音字符串__ ，其中元音字母是 `'a'`、 `'e'`、 `'i'`、 `'o'`、 `'u'` 。

返回 `words[i]` 是元音字符串的数目，其中 `i` 在闭区间 `[left, right]` 内。

__示例:__

示例 1：

```text
输入：words = ["are","amy","u"], left = 0, right = 2
输出：2
解释：
```

- "are" 是一个元音字符串，因为它以 'a' 开头并以 'e' 结尾。
- "amy" 不是元音字符串，因为它没有以元音字母结尾。
- "u" 是一个元音字符串，因为它以 'u' 开头并以 'u' 结尾。

在上述范围中的元音字符串数目为 2 。

示例 2：

```text
输入：words = ["hey","aeo","mu","ooo","artro"], left = 1, right = 4
输出：3
解释：
```

- "aeo" 是一个元音字符串，因为它以 'a' 开头并以 'o' 结尾。
- "mu" 不是元音字符串，因为它没有以元音字母开头。
- "ooo" 是一个元音字符串，因为它以 'o' 开头并以 'o' 结尾。
- "artro" 是一个元音字符串，因为它以 'a' 开头并以 'o' 结尾。

在上述范围中的元音字符串数目为 3 。

__提示：__

- `1 <= words.length <= 1000`
- `1 <= words[i].length <= 10`
- `words[i]` 仅由小写英文字母组成
- `0 <= left <= right < words.length`

__思路:__

```text
模拟
统计 words[left:right + 1] 中以元音字母开头并以元音字母结尾的字符串数目
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int vowelStrings(vector<string>& words, int left, int right) 
    {
        return count_if(words.begin() + left, words.begin() + right + 1, [=](auto& word) { return (word.front() == 'a' or word.front() == 'e' or word.front() == 'i' or word.front() == 'o' or word.front() == 'u') and (word.back() == 'a' or word.back() == 'e' or word.back() == 'i' or word.back() == 'o' or word.back() == 'u');  });
    }
};
```

__Java__:

```Java
class Solution {
    public int vowelStrings(String[] words, int left, int right) {
        return (int)Arrays.stream(words).skip(left).limit(right - left + 1).filter(word -> (word.charAt(0) == 'a' || word.charAt(0) == 'e' || word.charAt(0) == 'i' || word.charAt(0) == 'o' || word.charAt(0) == 'u') && (word.charAt(word.length() - 1) == 'a' || word.charAt(word.length() - 1) == 'e' || word.charAt(word.length() - 1) == 'i' || word.charAt(word.length() - 1) == 'o' || word.charAt(word.length() - 1) == 'u')).count();
    }
}
```

__Python__:

```Python
class Solution:
    def vowelStrings(self, words: List[str], left: int, right: int) -> int:
        return sum(word[0] in 'aeiou' and word[-1] in 'aeiou' for word in words[left:right + 1])
```
