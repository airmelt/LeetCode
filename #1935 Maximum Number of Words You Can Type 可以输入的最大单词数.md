# 1935 Maximum Number of Words You Can Type 可以输入的最大单词数

__Description:__

There is a malfunctioning keyboard where some letter keys do not work. All other keys on the keyboard work properly.

Given a string `text` of words separated by a single space (no leading or trailing spaces) and a string `brokenLetters` of all __distinct__ letter keys that are broken, return _the __number of words__ in_ `text` _you can fully type using this keyboard_.

__Example:__

Example 1:

```text
Input: text = "hello world", brokenLetters = "ad"
Output: 1
Explanation: We cannot type "world" because the 'd' key is broken.
```

Example 2:

```text
Input: text = "leet code", brokenLetters = "lt"
Output: 1
Explanation: We cannot type "leet" because the 'l' and 't' keys are broken.
```

Example 3:

```text
Input: text = "leet code", brokenLetters = "e"
Output: 0
Explanation: We cannot type either word because the 'e' key is broken.
```

__Constraints:__

- `1 <= text.length <= 10 ^ 4`
- `0 <= brokenLetters.length <= 26`
- `text` consists of words separated by a single space without any leading or trailing spaces.
- Each word only consists of lowercase English letters.
- `brokenLetters` consists of __distinct__ lowercase English letters.

__题目描述:__

键盘出现了一些故障，有些字母键无法正常工作。而键盘上所有其他键都能够正常工作。

给你一个由若干单词组成的字符串 `text` ，单词间由单个空格组成（不含前导和尾随空格）；另有一个字符串 `brokenLetters` ，由所有已损坏的不同字母键组成，返回你可以使用此键盘完全输入的 `text` 中单词的数目。

__示例:__

示例 1：

```text
输入：text = "hello world", brokenLetters = "ad"
输出：1
解释：无法输入 "world" ，因为字母键 'd' 已损坏。
```

示例 2：

```text
输入：text = "leet code", brokenLetters = "lt"
输出：1
解释：无法输入 "leet" ，因为字母键 'l' 和 't' 已损坏。
```

示例 3：

```text
输入：text = "leet code", brokenLetters = "e"
输出：0
解释：无法输入任何单词，因为字母键 'e' 已损坏。
```

__提示：__

- `1 <= text.length <= 10 ^ 4`
- `0 <= brokenLetters.length <= 26`
- `text` 由若干用单个空格分隔的单词组成，且不含任何前导和尾随空格
- 每个单词仅由小写英文字母组成
- `brokenLetters` 由 __互不相同__ 的小写英文字母组成

__思路:__

```text
模拟
在 text 后面加一个空格作为哨兵
或者通过 split() 分割 text
遍历 text, 如果遇到空格, 则单词数 + 1
否则, 如果遇到 brokenLetters 中的字符, 则跳过该单词
时间复杂度为 O(M + N), 空间复杂度为 O(N), 其中 M 为 text 的长度, N 为 brokenLetters 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int canBeTypedWords(string text, string brokenLetters) 
    {
        text += ' ';
        int n = text.size(), result = 0;
        unordered_set<char> s(brokenLetters.begin(), brokenLetters.end());
        for (int i = 0; i < n; i++) 
        {
            if (text[i] == ' ') ++result;
            else if (s.count(text[i])) while (text[i] != ' ') ++i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int canBeTypedWords(String text, String brokenLetters) {
        String[] words = text.split(" ");
        int result = 0, m = words.length, n = brokenLetters.length(), j = 0;
        if (n == 0) return m;
        for (int i = 0; i < m; i++) {
            for (j = 0; j < n; j++) if (words[i].indexOf(brokenLetters.charAt(j)) != -1) break;
            if (j == n) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def canBeTypedWords(self, text: str, brokenLetters: str) -> int:
        return sum(not any(c in word for c in brokenLetters) for word in text.split())
```
