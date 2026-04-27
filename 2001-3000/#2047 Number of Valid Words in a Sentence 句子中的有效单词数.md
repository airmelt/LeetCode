# 2047 Number of Valid Words in a Sentence 句子中的有效单词数

__Description:__

A sentence consists of lowercase letters ( `'a'` to `'z'`), digits ( `'0'` to `'9'`), hyphens ( `'-'`), punctuation marks ( `'!'`, `'.'`, and `','`), and spaces ( `' '`) only. Each sentence can be broken down into __one or more tokens__ separated by one or more spaces `' '`.

A token is a valid word if all three of the following are true:

- It only contains lowercase letters, hyphens, and/or punctuation (__no__ digits).
- There is __at most one__ hyphen `'-'`. If present, it __must__ be surrounded by lowercase characters ( `"a-b"` is valid, but `"-ab"` and `"ab-"` are not valid).
- There is __at most one__ punctuation mark. If present, it __must__ be at the __end__ of the token ( `"ab,"`, `"cd!"`, and `"."` are valid, but `"a!b"` and `"c.,"` are not valid).

Examples of valid words include `"a-b."`, `"afad"`, `"ba-c"`, `"a!"`, and `"!"`.

Given a string `sentence`, return _the number of valid words in_ `sentence`.

__Example:__

Example 1:

```text
Input: sentence = "cat and  dog"
Output: 3
Explanation: The valid words in the sentence are "cat", "and", and "dog".
```

Example 2:

```text
Input: sentence = "!this  1-s b8d!"
Output: 0
Explanation: There are no valid words in the sentence.
"!this" is invalid because it starts with a punctuation mark.
"1-s" and "b8d" are invalid because they contain digits.
```

Example 3:

```text
Input: sentence = "alice and  bob are playing stone-game10"
Output: 5
Explanation: The valid words in the sentence are "alice", "and", "bob", "are", and "playing".
"stone-game10" is invalid because it contains digits.
```

__Constraints:__

- `1 <= sentence.length <= 1000`
- `sentence` only contains lowercase English letters, digits, `' '`, `'-'`, `'!'`, `'.'`, and `','`.
- There will be at least `1` token.

__题目描述:__

句子仅由小写字母（ `'a'` 到 `'z'`）、数字（ `'0'` 到 `'9'`）、连字符（ `'-'`）、标点符号（ `'!'`、 `'.'` 和 `','`）以及空格（ `' '`）组成。每个句子可以根据空格分解成 __一个或者多个 token__ ，这些 token 之间由一个或者多个空格 `' '` 分隔。

如果一个 token 同时满足下述条件，则认为这个 token 是一个有效单词：

- 仅由小写字母、连字符和/或标点（不含数字）组成。
- __至多一个__ 连字符 `'-'` 。如果存在，连字符两侧应当都存在小写字母（ `"a-b"` 是一个有效单词，但 `"-ab"` 和 `"ab-"` 不是有效单词）。
- __至多一个__ 标点符号。如果存在，标点符号应当位于 token 的 __末尾__ 。

这里给出几个有效单词的例子: `"a-b."`、 `"afad"`、 `"ba-c"`、 `"a!"` 和 `"!"` 。

给你一个字符串 `sentence` ，请你找出并返回 `sentence` 中 __有效单词的数目__ 。

__示例:__

示例 1：

```text
输入：sentence = "cat and  dog"
输出：3
解释：句子中的有效单词是 "cat"、"and" 和 "dog"
```

示例 2：

```text
输入：sentence = "!this  1-s b8d!"
输出：0
解释：句子中没有有效单词
"!this" 不是有效单词，因为它以一个标点开头
"1-s" 和 "b8d" 也不是有效单词，因为它们都包含数字
```

示例 3：

```text
输入：sentence = "alice and  bob are playing stone-game10"
输出：5
解释：句子中的有效单词是 "alice"、"and"、"bob"、"are" 和 "playing"
"stone-game10" 不是有效单词，因为它含有数字
```

__提示：__

- `1 <= sentence.length <= 1000`
- `sentence` 由小写英文字母、数字（ `0-9`）、以及字符（ `' '`、 `'-'`、 `'!'`、 `'.'` 和 `','`）组成
- 句子中至少有 `1` 个 token

__思路:__

```text
模拟
可以用正则表达式匹配有效单词
也可以先将句子分割成单词, 然后检查每个单词是否有效
每个词不能为空, 不能包含数字, 不能包含空格
最多只能有一个连字符, 如果有连字符, 连字符两侧必须是字母
最多只能有一个标点符号, 如果有标点符号, 标点符号必须在单词末尾
时间复杂度为 O(MN), 空间复杂度为 O(M), 其中 M 为句子长度, N 为单词平均长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countValidWords(string sentence) 
    {
        regex ex("[a-z]*([a-z]-[a-z])?[a-z]*[!,.]?");
        istringstream ss(sentence);
        string token;
        int result = 0;
        while (ss >> token) result += regex_match(token, ex);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countValidWords(String sentence) {
        String[] ss = sentence.split(" ");
        int result = 0;
        for (String s : ss) if (check(s)) ++result;
        return result;
    }

    private boolean check(String s) {
        int n = s.length();
        if (n == 0) return false;
        for (int i = 0, c1 = 0, c2 = 0; i < n; i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) return false;
            if (c == ' ') return false;
            if (c == '-' && ++c1 >= 0) {
                if (c1 > 1 || (i == 0 || i == n - 1)) return false;
                if (!Character.isLetter(s.charAt(i - 1)) || !Character.isLetter(s.charAt(i + 1))) return false;
            }
            if ((c == '!' || c == '.' || c == ',') && ++c2 >= 0) if (c2 > 1 || (i != n - 1)) return false;
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def countValidWords(self, sentence: str) -> int:
        return sum(bool(re.match('([,.!]|[a-z]+(-[a-z]+)?[,.!]?)$', w)) for w in sentence.split(' '))
```
