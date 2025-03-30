# 2490 Circular Sentence 回环句

__Description:__

A sentence is a list of words that are separated by a single space with no leading or trailing spaces.

- For example, `"Hello World"`, `"HELLO"`, `"hello world hello world"` are all sentences.

Words consist of only uppercase and lowercase English letters. Uppercase and lowercase English letters are considered different.

A sentence is circular if:

- The last character of each word in the sentence is equal to the first character of its next word.
- The last character of the last word is equal to the first character of the first word.

For example, `"leetcode exercises sound delightful"`, `"eetcode"`, `"leetcode eats soul"` are all circular sentences. However, `"Leetcode is cool"`, `"happy Leetcode"`, `"Leetcode"` and `"I like Leetcode"` are __not__ circular sentences.

Given a string `sentence`, return `true` _if it is circular_. Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: sentence = "leetcode exercises sound delightful"
Output: true
Explanation: The words in sentence are ["leetcode", "exercises", "sound", "delightful"].
```

- leetcode's last character is equal to exercises's first character.
- exercises's last character is equal to sound's first character.
- sound's last character is equal to delightful's first character.
- delightful's last character is equal to leetcode's first character.

The sentence is circular.

Example 2:

```text
Input: sentence = "eetcode"
Output: true
Explanation: The words in sentence are ["eetcode"].
```

- eetcode's last character is equal to eetcode's first character.

The sentence is circular.

Example 3:

```text
Input: sentence = "Leetcode is cool"
Output: false
Explanation: The words in sentence are ["Leetcode", "is", "cool"].
```

- Leetcode's last character is not equal to is's first character.

The sentence is not circular.

__Constraints:__

- `1 <= sentence.length <= 500`
- `sentence` consist of only lowercase and uppercase English letters and spaces.
- The words in `sentence` are separated by a single space.
- There are no leading or trailing spaces.

__题目描述:__

句子 是由单个空格分隔的一组单词，且不含前导或尾随空格。

- 例如， `"Hello World"`、 `"HELLO"`、 `"hello world hello world"` 都是符合要求的句子。

单词 仅 由大写和小写英文字母组成。且大写和小写字母会视作不同字符。

如果句子满足下述全部条件，则认为它是一个 回环句 ：

- 句子中每个单词的最后一个字符等于下一个单词的第一个字符。
- 最后一个单词的最后一个字符和第一个单词的第一个字符相等。

例如， `"leetcode exercises sound delightful"`、 `"eetcode"`、 `"leetcode eats soul"` 都是回环句。然而， `"Leetcode is cool"`、 `"happy Leetcode"`、 `"Leetcode"` 和 `"I like Leetcode"` 都 __不__ 是回环句。

给你一个字符串 `sentence` ，请你判断它是不是一个回环句。如果是，返回 `true` ；否则，返回 `false` 。

__示例:__

示例 1：

```text
输入：sentence = "leetcode exercises sound delightful"
输出：true
解释：句子中的单词是 ["leetcode", "exercises", "sound", "delightful"] 。
```

- leetcode 的最后一个字符和 exercises 的第一个字符相等。
- exercises 的最后一个字符和 sound 的第一个字符相等。
- sound 的最后一个字符和 delightful 的第一个字符相等。
- delightful 的最后一个字符和 leetcode 的第一个字符相等。

这个句子是回环句。

示例 2：

```text
输入：sentence = "eetcode"
输出：true
解释：句子中的单词是 ["eetcode"] 。
```

- eetcode 的最后一个字符和 eetcode 的第一个字符相等。

这个句子是回环句。

示例 3：

```text
输入：sentence = "Leetcode is cool"
输出：false
解释：句子中的单词是 ["Leetcode", "is", "cool"] 。
```

- Leetcode 的最后一个字符和 is 的第一个字符 不 相等。

这个句子 不 是回环句。

__提示：__

- `1 <= sentence.length <= 500`
- `sentence` 仅由大小写英文字母和空格组成
- `sentence` 中的单词由单个空格进行分隔
- 不含任何前导或尾随空格

__思路:__

```text
模拟
如果句子的第一个字符和最后一个字符不相等, 返回 false
遍历句子, 如果当前字符是空格, 且前一个字符和后一个字符不相等, 返回 false
否则返回 true
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool isCircularSentence(string sentence) 
    {
        if (sentence.front() != sentence.back()) return false;
        for (int i = 1, n = sentence.size(); i < n - 1; i++) if (sentence[i] == ' ' and sentence[i - 1] != sentence[i + 1]) return false;
        return true;    
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isCircularSentence(String sentence) {
        if (sentence.charAt(0) != sentence.charAt(sentence.length() - 1)) return false;
        for (int i = 1, n = sentence.length(); i < n - 1; i++) if (sentence.charAt(i) == ' ' && sentence.charAt(i - 1) != sentence.charAt(i + 1)) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def isCircularSentence(self, sentence: str) -> bool:
        return sentence[-1] == sentence[0] and all(x[-1] == y[0] for x, y in pairwise(sentence.split()))
```
