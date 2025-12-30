# 2828 Check if a String Is an Acronym of Words 判别首字母缩略词

__Description:__

Given an array of strings `words` and a string `s`, determine if `s` is an __acronym__ of words.

The string `s` is considered an acronym of `words` if it can be formed by concatenating the __first__ character of each string in `words` __in order__. For example, `"ab"` can be formed from `["apple", "banana"]`, but it can't be formed from `["bear", "aardvark"]`.

Return `true` _if_ `s` _is an acronym of_ `words`_, and_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: words = ["alice","bob","charlie"], s = "abc"
Output: true
Explanation: The first character in the words "alice", "bob", and "charlie" are 'a', 'b', and 'c', respectively. Hence, s = "abc" is the acronym.
```

Example 2:

```text
Input: words = ["an","apple"], s = "a"
Output: false
Explanation: The first character in the words "an" and "apple" are 'a' and 'a', respectively. 
The acronym formed by concatenating these characters is "aa". 
Hence, s = "a" is not the acronym.
```

Example 3:

```text
Input: words = ["never","gonna","give","up","on","you"], s = "ngguoy"
Output: true
Explanation: By concatenating the first character of the words in the array, we get the string "ngguoy". 
Hence, s = "ngguoy" is the acronym.
```

__Constraints:__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 10`
- `1 <= s.length <= 100`
- `words[i]` and `s` consist of lowercase English letters.

__题目描述:__

给你一个字符串数组 `words` 和一个字符串 `s` ，请你判断 `s` 是不是 `words` 的 __首字母缩略词__ 。

如果可以按顺序串联 `words` 中每个字符串的第一个字符形成字符串 `s` ，则认为 `s` 是 `words` 的首字母缩略词。例如， `"ab"` 可以由 `["apple", "banana"]` 形成，但是无法从 `["bear", "aardvark"]` 形成。

如果 `s` 是 `words` 的首字母缩略词，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：words = ["alice","bob","charlie"], s = "abc"
输出：true
解释：words 中 "alice"、"bob" 和 "charlie" 的第一个字符分别是 'a'、'b' 和 'c'。因此，s = "abc" 是首字母缩略词。
```

示例 2：

```text
输入：words = ["an","apple"], s = "a"
输出：false
解释：words 中 "an" 和 "apple" 的第一个字符分别是 'a' 和 'a'。
串联这些字符形成的首字母缩略词是 "aa" 。
因此，s = "a" 不是首字母缩略词。
```

示例 3：

```text
输入：words = ["never","gonna","give","up","on","you"], s = "ngguoy"
输出：true
解释：串联数组 words 中每个字符串的第一个字符，得到字符串 "ngguoy" 。
因此，s = "ngguoy" 是首字母缩略词。
```

__提示：__

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 10`
- `1 <= s.length <= 100`
- `words[i]` 和 `s` 由小写英文字母组成

__思路:__

```text
模拟
先比较长度是否相等
再比较 words 中每个字符串的第一个字符和 s 中每一个字符是否对应相等
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isAcronym(vector<string>& words, string s) 
    {
        return words.size() == s.size() and ranges::equal(words, s, {}, [](auto& w) { return w.front(); });
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isAcronym(List<String> words, String s) { 
        return words.size() == s.length() && Objects.equals(s, String.join("", words.stream().map(word -> String.valueOf(word.charAt(0))).toList()));
    }
}
```

__Python__:

```Python
class Solution:
    def isAcronym(self, words: List[str], s: str) -> bool:
        return len(s) == len(words) and s == reduce(lambda x, y: x + y, (word[0] for word in words))
```
