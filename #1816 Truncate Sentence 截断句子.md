# 1816 Truncate Sentence 截断句子

__Description:__

A sentence is a list of words that are separated by a single space with no leading or trailing spaces. Each of the words consists of only uppercase and lowercase English letters (no punctuation).

- For example, `"Hello World"`, `"HELLO"`, and `"hello world hello world"` are all sentences.

You are given a sentence `s`​​​​​​ and an integer `k`​​​​​​. You want to __truncate__ `s`​​​​​​ such that it contains only the __first__ `k`​​​​​​ words. Return `s`​​​​​ after __truncating__ it.

__Example:__

Example 1:

```text
Input: s = "Hello how are you Contestant", k = 4
Output: "Hello how are you"
Explanation:
The words in s are ["Hello", "how" "are", "you", "Contestant"].
The first 4 words are ["Hello", "how", "are", "you"].
Hence, you should return "Hello how are you".
```

Example 2:

```text
Input: s = "What is the solution to this problem", k = 4
Output: "What is the solution"
Explanation:
The words in s are ["What", "is" "the", "solution", "to", "this", "problem"].
The first 4 words are ["What", "is", "the", "solution"].
Hence, you should return "What is the solution".
```

Example 3:

```text
Input: s = "chopper is not a tanuki", k = 5
Output: "chopper is not a tanuki"
```

__Constraints:__

- `1 <= s.length <= 500`
- `k` is in the range `[1, the number of words in s]`.
- `s` consist of only lowercase and uppercase English letters and spaces.
- The words in `s` are separated by a single space.
- There are no leading or trailing spaces.

__题目描述:__

句子 是一个单词列表，列表中的单词之间用单个空格隔开，且不存在前导或尾随空格。每个单词仅由大小写英文字母组成（不含标点符号）。

- 例如， `"Hello World"`、 `"HELLO"` 和 `"hello world hello world"` 都是句子。

给你一个句子 `s`​​​​​​ 和一个整数 `k`​​​​​​ ，请你将 `s`​​ __截断__ ​，​​​使截断后的句子仅含 __前__ `k`​​​​​​ 个单词。返回 __截断__ `s`​​​​ 后得到的句子。

__示例:__

示例 1：

```text
输入：s = "Hello how are you Contestant", k = 4
输出："Hello how are you"
解释：
s 中的单词为 ["Hello", "how" "are", "you", "Contestant"]
前 4 个单词为 ["Hello", "how", "are", "you"]
因此，应当返回 "Hello how are you"
```

示例 2：

```text
输入：s = "What is the solution to this problem", k = 4
输出："What is the solution"
解释：
s 中的单词为 ["What", "is" "the", "solution", "to", "this", "problem"]
前 4 个单词为 ["What", "is", "the", "solution"]
因此，应当返回 "What is the solution"
```

示例 3：

```text
输入：s = "chopper is not a tanuki", k = 5
输出："chopper is not a tanuki"
```

__提示：__

- `1 <= s.length <= 500`
- `k` 的取值范围是 `[1, s 中单词的数目]`
- `s` 仅由大小写英文字母和空格组成
- `s` 中的单词之间由单个空格隔开
- 不存在前导或尾随空格

__思路:__

```text
模拟
不需要将字符串按照空格分割
只需要遍历字符串
遇到空格时将 k 减一
当 k 减为 0 时
返回当前位置之前的字符串即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution
{
public:
    string truncateSentence(string s, int k) 
    {
        for (int i = 0, n = s.size(); i < n; i++) if (s[i] == ' ' and !(--k)) return s.substr(0, i);
        return s;
    }
};
```

__Java__:

```Java
class Solution {
    public String truncateSentence(String s, int k) {
        for (int i = 0, n = s.length(); i < n; i++) if (s.charAt(i) == ' ' && --k == 0) return s.substring(0, i);
        return s;
    }
}
```

__Python__:

```Python
class Solution:
    def truncateSentence(self, s: str, k: int) -> str:
        return ' '.join(s.split(' ')[:k])
```
