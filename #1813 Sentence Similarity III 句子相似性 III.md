# 1813 Sentence Similarity III 句子相似性 III

__Description:__

A sentence is a list of words that are separated by a single space with no leading or trailing spaces. For example, `"Hello World"`, `"HELLO"`, `"hello world hello world"` are all sentences. Words consist of __only__ uppercase and lowercase English letters.

Two sentences `sentence1` and `sentence2` are __similar__ if it is possible to insert an arbitrary sentence __(possibly empty)__ inside one of these sentences such that the two sentences become equal. For example, `sentence1 = "Hello my name is Jane"` and `sentence2 = "Hello Jane"` can be made equal by inserting `"my name is"` between `"Hello"` and `"Jane"` in `sentence2`.

Given two sentences `sentence1` and `sentence2`, return `true` _if_ `sentence1` _and_ `sentence2` _are similar._ Otherwise, return `false`.

__Example:__

Example 1:

```text
Input: sentence1 = "My name is Haley", sentence2 = "My Haley"
Output: true
Explanation: sentence2 can be turned to sentence1 by inserting "name is" between "My" and "Haley".
```

Example 2:

```text
Input: sentence1 = "of", sentence2 = "A lot of words"
Output: false
Explanation: No single sentence can be inserted inside one of the sentences to make it equal to the other.
```

Example 3:

```text
Input: sentence1 = "Eating right now", sentence2 = "Eating"
Output: true
Explanation: sentence2 can be turned to sentence1 by inserting "right now" at the end of the sentence.
```

__Constraints:__

- `1 <= sentence1.length, sentence2.length <= 100`
- `sentence1` and `sentence2` consist of lowercase and uppercase English letters and spaces.
- The words in `sentence1` and `sentence2` are separated by a single space.

__题目描述:__

一个句子是由一些单词与它们之间的单个空格组成，且句子的开头和结尾没有多余空格。比方说， `"Hello World"` ， `"HELLO"` ， `"hello world hello world"` 都是句子。每个单词都 __只__ 包含大写和小写英文字母。

如果两个句子 `sentence1` 和 `sentence2` ，可以通过往其中一个句子插入一个任意的句子（__可以是空句子__）而得到另一个句子，那么我们称这两个句子是 __相似的__ 。比方说， `sentence1 = "Hello my name is Jane"` 且 `sentence2 = "Hello Jane"` ，我们可以往 `sentence2` 中 `"Hello"` 和 `"Jane"` 之间插入 `"my name is"` 得到 `sentence1` 。

给你两个句子 `sentence1` 和 `sentence2` ，如果 `sentence1` 和 `sentence2` 是相似的，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：sentence1 = "My name is Haley", sentence2 = "My Haley"
输出：true
解释：可以往 sentence2 中 "My" 和 "Haley" 之间插入 "name is" ，得到 sentence1 。
```

示例 2：

```text
输入：sentence1 = "of", sentence2 = "A lot of words"
输出：false
解释：没法往这两个句子中的一个句子只插入一个句子就得到另一个句子。
```

示例 3：

```text
输入：sentence1 = "Eating right now", sentence2 = "Eating"
输出：true
解释：可以往 sentence2 的结尾插入 "right now" 得到 sentence1 。
```

示例 4：

```text
输入：sentence1 = "Luky", sentence2 = "Lucccky"
输出：false
```

__提示：__

- `1 <= sentence1.length, sentence2.length <= 100`
- `sentence1` 和 `sentence2` 都只包含大小写英文字母和空格。
- `sentence1` 和 `sentence2` 中的单词都只由单个空格隔开。

__思路:__

```text
双指针
将字符串按照空格分隔符分割成数组
从头尾两端分别遍历直到出现不相等的字符串
比较指针的位置是否等于字符串数组的长度
时间复杂度为 O(M + N), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool areSentencesSimilar(string sentence1, string sentence2) 
    {
        vector<string> s1, s2; 
        split(sentence1, s1, " ");
        split(sentence2, s2, " ");
        int m = s1.size(), n = s2.size(), i = 0, j = 0;
        while (i < m and i < n and s1[i] == s2[i]) ++i;
        while (j < m - i and j < n - i and s1[m - j - 1] == s2[n - j - 1]) ++j;
        return i + j == min(m, n);
    }
private:
    // Splits this string around matches of the given separator
    int split(const string& str, vector<string>& result, string separator = ",")
    {
        if (str.empty())
        {
            return 0;
        }

        string temp;
        string::size_type pos_begin = str.find_first_not_of(separator);
        string::size_type comma_pos = 0;

        while (pos_begin != string::npos)
        {
            comma_pos = str.find(separator, pos_begin);
            if (comma_pos != string::npos)
            {
                temp = str.substr(pos_begin, comma_pos - pos_begin);
                pos_begin = comma_pos + separator.length();
            }
            else
            {
                temp = str.substr(pos_begin);
                pos_begin = comma_pos;
            }

            if (!temp.empty())
            {
                result.push_back(temp);
                /* string.clear()
                作用: 将字符串的内容清空，让源字符串成为一个空字符串（长度为0个字符）*/
                temp.clear();
            }
        }
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean areSentencesSimilar(String sentence1, String sentence2) {
        String[] s1 = sentence1.split(" "), s2 = sentence2.split(" ");
        int m = s1.length, n = s2.length, i = 0, j = 0;
        while (i < m && i < n && s1[i].equals(s2[i])) ++i;
        while (j < m - i && j < n - i && s1[m - j - 1].equals(s2[n - j - 1])) ++j;
        return i + j == Math.min(m, n);
    }
}
```

__Python__:

```Python
class Solution:
    def areSentencesSimilar(self, sentence1: str, sentence2: str) -> bool:
        return sentence1 == sentence2 or sentence2.startswith(sentence1 + ' ') or sentence2.endswith(' ' + sentence1) or next((True for i, c in enumerate(sentence1) if c == ' ' and sentence2.startswith(sentence1[:i] + ' ') and sentence2.endswith(' ' + sentence1[i + 1:])), False) if len(sentence1) <= len(sentence2) else self.areSentencesSimilar(sentence2, sentence1)
```
