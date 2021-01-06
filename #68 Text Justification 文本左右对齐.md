# 68 Text Justification 文本左右对齐

__Description__:
Given an array of words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

__Note:__

A word is defined as a character sequence consisting of non-space characters only.
Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
The input array words contains at least one word.

__Example:__

Example 1:

Input:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
Output:

```text
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

Example 2:

Input:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
Output:

```text
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
```

Explanation: Note that the last line is "shall be    " instead of "shall     be",
             because the last line must be left-justified instead of fully-justified.
             Note that the second line is also left-justified becase it contains only one word.

Example 3:

Input:
words = ["Science","is","what","we","understand","well","enough","to","explain",
         "to","a","computer.","Art","is","everything","else","we","do"]
maxWidth = 20
Output:

```text
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

__题目描述__:
给定一个单词数组和一个长度 maxWidth，重新排版单词，使其成为每行恰好有 maxWidth 个字符，且左右两端对齐的文本。

你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 ' ' 填充，使得每行恰好有 maxWidth 个字符。

要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。

文本的最后一行应为左对齐，且单词之间不插入额外的空格。

__说明:__

单词是指由非空格字符组成的字符序列。
每个单词的长度大于 0，小于等于 maxWidth。
输入单词数组 words 至少包含一个单词。

__示例 :__

示例 1:

输入:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
输出:

```text
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

示例 2:

输入:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
输出:

```text
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
```

解释: 注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
     因为最后一行应为左对齐，而不是左右两端对齐。
     第二行同样为左对齐，这是因为这行只包含一个单词。

示例 3:

输入:
words = ["Science","is","what","we","understand","well","enough","to","explain",
         "to","a","computer.","Art","is","everything","else","we","do"]
maxWidth = 20
输出:

```text
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

__思路__:

按照题目要求放置, 注意边界条件的处理
时间复杂度O(mn), 空间复杂度O(m), m为字符串长度, n为字符串数组长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) 
    {
        vector<string> result;
        string line = "";
        int cur = 0, line_word = 0, word_num = 0;
        while (word_num < words.size())
        {
            if (cur + words[word_num].size() <= maxWidth) 
            {
                cur += words[word_num].size();
                line += words[word_num];
                line_word++;
                if (cur < maxWidth)
                {
                    cur++;
                    line += " ";
                }
                else if (cur == maxWidth)
                {
                    result.push_back(line);
                    line = "";
                    cur = 0;
                    line_word = 0;
                }
                word_num++;
            }
            else
            {
                cur--;
                line.erase(line.end() - 1);
                int space_num = maxWidth - cur;
                if (line_word == 1) 
                {
                    string space(space_num, ' ');
                    line += space;
                    result.push_back(line);
                    line_word = 0;
                    cur = 0;
                    line = "";
                }
                else
                {
                    vector<int> spaces(line_word - 1, 0);
                    int i = line_word - 2;
                    for (; i >= 0; i--)
                    {
                        spaces[i] = space_num / (line_word - 1);
                        space_num -= spaces[i];
                        line_word--;
                    }
                    int temp = 0, wordpos = word_num - spaces.size() - 1;
                    for(i = 0; i < spaces.size(); i++)
                    {
                        temp += words[wordpos].size() + 1;
                        string space_temp = string(spaces[i], ' ');
                        line.insert(temp, space_temp);
                        temp += spaces[i];
                        wordpos++;
                    }
                    result.push_back(line);
                    line_word = 0;
                    cur = 0;
                    line = "";
                }
                
            }
        }
        if (cur != 0)
        {
            string block(maxWidth - cur, ' ');
            line += block;
            result.push_back(line);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> result = new ArrayList<>();
        StringBuilder line = new StringBuilder();
        int cur = 0, line_word = 0, word_num = 0;
        while (word_num < words.length) {
            if (cur + words[word_num].length() <= maxWidth) {
                cur += words[word_num].length();
                line.append(words[word_num]);
                line_word++;
                if (cur < maxWidth) {
                    cur++;
                    line.append(' ');
                }
                else if (cur == maxWidth) {
                    result.add(line.toString());
                    line.delete(0, line.length());
                    cur = 0;
                    line_word = 0;
                }
                word_num++;
            } else {
                cur--;
                line.setLength(line.length() - 1);
                int space_num = maxWidth - cur;
                if (line_word == 1) {
                    for (int i = 0; i < space_num; i++) line.append(' ');
                    result.add(line.toString());
                    line_word = 0;
                    cur = 0;
                    line.delete(0, line.length());;
                } else {
                    int i = line_word - 2, spaces[] = new int[line_word - 1];
                    for (; i >= 0; i--) {
                        spaces[i] = space_num / (line_word - 1);
                        space_num -= spaces[i];
                        line_word--;
                    }
                    int temp = 0, wordpos = word_num - spaces.length - 1;
                    for(i = 0; i < spaces.length; i++) {
                        temp += words[wordpos].length() + 1;
                        for (int j = 0; j < spaces[i]; j++) line.insert(temp, ' ');
                        temp += spaces[i];
                        wordpos++;
                    }
                    result.add(line.toString());
                    line_word = 0;
                    cur = 0;
                    line.delete(0, line.length());
                }
                
            }
        }
        if (cur != 0) {
            for (int i = 0; i < maxWidth - cur; i++) line.append(' ');
            result.add(line.toString());
        }
        return result;
    }
}
```

__Python__:

```Python
from functools import reduce
class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        rows = []
        def func(length, word):
            if length + len(word) >= maxWidth or not rows:
                rows.append([word])
                return len(word)
            rows[-1].append(word)
            return length + len(word) + 1
        reduce(func, words, 0)
        def align_normal(row):
            if len(row) == 1:
                yield row[0] + ' ' * (maxWidth - len(row[0]))
                return
            space_size, extra = divmod(maxWidth - sum(map(len, row)), len(row) - 1)
            for i in range(extra):
                yield row[i] + ' ' * (space_size + 1)
            for i in range(extra, len(row) - 1):
                yield row[i] + ' ' * space_size
            yield row[-1]
        def align_last(row):
            for i in range(len(row) - 1):
                yield row[i] + ' '
            yield row[-1] + ' ' * (maxWidth - len(row) + 1 - sum(map(len, row)))
        return [''.join(align_normal(row)) for row in rows[:-1]] + [''.join(align_last(rows[-1]))]
```
