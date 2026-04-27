# 1859 Sorting the Sentence 将句子排序

__Description:__

A sentence is a list of words that are separated by a single space with no leading or trailing spaces. Each word consists of lowercase and uppercase English letters.

A sentence can be shuffled by appending the 1-indexed word position to each word then rearranging the words in the sentence.

- For example, the sentence `"This is a sentence"` can be shuffled as `"sentence4 a3 is2 This1"` or `"is2 sentence4 This1 a3"`.

Given a __shuffled sentence__ `s` containing no more than `9` words, reconstruct and return _the original sentence_.

__Example:__

Example 1:

```text
Input: s = "is2 sentence4 This1 a3"
Output: "This is a sentence"
Explanation: Sort the words in s to their original positions "This1 is2 a3 sentence4", then remove the numbers.
```

Example 2:

```text
Input: s = "Myself2 Me1 I4 and3"
Output: "Me Myself and I"
Explanation: Sort the words in s to their original positions "Me1 Myself2 and3 I4", then remove the numbers.
```

__Constraints:__

- `2 <= s.length <= 200`
- `s` consists of lowercase and uppercase English letters, spaces, and digits from `1` to `9`.
- The number of words in `s` is between `1` and `9`.
- The words in `s` are separated by a single space.
- `s` contains no leading or trailing spaces.

__题目描述:__

一个 句子 指的是一个序列的单词用单个空格连接起来，且开头和结尾没有任何空格。每个单词都只包含小写或大写英文字母。

我们可以给一个句子添加 从 1 开始的单词位置索引 ，并且将句子中所有单词 打乱顺序 。

- 比方说，句子 `"This is a sentence"` 可以被打乱顺序得到 `"sentence4 a3 is2 This1"` 或者 `"is2 sentence4 This1 a3"` 。

给你一个 __打乱顺序__ 的句子 `s` ，它包含的单词不超过 `9` 个，请你重新构造并得到原本顺序的句子。

__示例:__

示例 1：

```text
输入：s = "is2 sentence4 This1 a3"
输出："This is a sentence"
解释：将 s 中的单词按照初始位置排序，得到 "This1 is2 a3 sentence4" ，然后删除数字。
```

示例 2：

```text
输入：s = "Myself2 Me1 I4 and3"
输出："Me Myself and I"
解释：将 s 中的单词按照初始位置排序，得到 "Me1 Myself2 and3 I4" ，然后删除数字。
```

__提示：__

- `2 <= s.length <= 200`
- `s` 只包含小写和大写英文字母、空格以及从 `1` 到 `9` 的数字。
- `s` 中单词数目为 `1` 到 `9` 个。
- `s` 中的单词由单个空格分隔。
- `s` 不包含任何前导或者后缀空格。

__思路:__

```text
模拟
可以将字符串先按照空格分割开, 然后按照最后一个字符的大小排序
或者直接找到字符中的数字并将字符串插入到合适的位置
时间复杂度为 O(N), 空间复杂度为 O(N), N 为字符串的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string sortSentence(string s) 
    {
        vector<string> arr(9);
        string cur = "";
        int n = 0;
        for (const auto& c : s)
        {
            if (c >= '0' && c <= '9')
            {
                arr[c - '0' - 1] = cur;
                cur.clear();
                ++n;
            }
            else if (c != ' ') cur.push_back(c);
        }
        string result = arr.front();
        for (int i = 1; i < n; i++) result += " " + arr[i];
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String sortSentence(String s) {
        return Arrays.stream(s.split(" ")).sorted(Comparator.comparingInt(o -> o.charAt(o.length() - 1))).map(e -> e.substring(0, e.length() - 1)).collect(Collectors.joining(" "));
    }
}
```

__Python__:

```Python
class Solution:
    def sortSentence(self, s: str) -> str:
        return ' '.join(map(lambda x: x[:-1], sorted([i for i in s.split(' ')], key=lambda x: x[-1])))
```
