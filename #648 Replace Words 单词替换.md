# 648 Replace Words 单词替换

__Description__:
In English, we have a concept called root, which can be followed by some other word to form another longer word - let's call this word successor. For example, when the root "an" is followed by the successor word "other", we can form a new word "another".

Given a dictionary consisting of many roots and a sentence consisting of words separated by spaces, replace all the successors in the sentence with the root forming it. If a successor can be replaced by more than one root, replace it with the root that has the shortest length.

Return the sentence after the replacement.

__Example:__

Example 1:

Input: dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"

Example 2:

Input: dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
Output: "a a b c"

Example 3:

Input: dictionary = ["a", "aa", "aaa", "aaaa"], sentence = "a aa a aaaa aaa aaa aaa aaaaaa bbb baba ababa"
Output: "a a a a a a a a bbb baba a"

Example 4:

Input: dictionary = ["catt","cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"

Example 5:

Input: dictionary = ["ac","ab"], sentence = "it is abnormal that this solution is accepted"
Output: "it is ab that this solution is ac"

__Constraints:__

1 <= dictionary.length <= 1000
1 <= dictionary[i].length <= 100
dictionary[i] consists of only lower-case letters.
1 <= sentence.length <= 10^6
sentence consists of only lower-case letters and spaces.
The number of words in sentence is in the range [1, 1000]
The length of each word in sentence is in the range [1, 1000]
Each two consecutive words in sentence will be separated by exactly one space.
sentence does not have leading or trailing spaces.

__题目描述__:
在英语中，我们有一个叫做 词根(root)的概念，它可以跟着其他一些词组成另一个较长的单词——我们称这个词为 继承词(successor)。例如，词根an，跟随着单词 other(其他)，可以形成新的单词 another(另一个)。

现在，给定一个由许多词根组成的词典和一个句子。你需要将句子中的所有继承词用词根替换掉。如果继承词有许多可以形成它的词根，则用最短的词根替换它。

你需要输出替换之后的句子。

__示例 :__

示例 1：

输入：dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
输出："the cat was rat by the bat"

示例 2：

输入：dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
输出："a a b c"

示例 3：

输入：dictionary = ["a", "aa", "aaa", "aaaa"], sentence = "a aa a aaaa aaa aaa aaa aaaaaa bbb baba ababa"
输出："a a a a a a a a bbb baba a"

示例 4：

输入：dictionary = ["catt","cat","bat","rat"], sentence = "the cattle was rattled by the battery"
输出："the cat was rat by the bat"

示例 5：

输入：dictionary = ["ac","ab"], sentence = "it is abnormal that this solution is accepted"
输出："it is ab that this solution is ac"

__提示:__

1 <= dictionary.length <= 1000
1 <= dictionary[i].length <= 100
dictionary[i] 仅由小写字母组成。
1 <= sentence.length <= 10^6
sentence 仅由小写字母和空格组成。
sentence 中单词的总量在范围 [1, 1000] 内。
sentence 中每个单词的长度在范围 [1, 1000] 内。
sentence 中单词之间由一个空格隔开。
sentence 没有前导或尾随空格。

__思路__:

1. 直接查找
判断 dictionary 中的每个词是否是 sentence 中的词的前缀并替代
时间复杂度 O(n ^ 2), 空间复杂度 O(n)
2. 前缀树
先生成 dictionary 的前缀树
遍历 sentence 查找前缀树
时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Trie
{
private:
    bool is_word;
    Trie *child[26] = {0};
public:
    Trie(): is_word(false) {}
    
    void insert(const string &word)
    {
        Trie *cur = this;
        for (auto &c : word)
        {
            int i = c - 'a';
            if (!cur -> child[i]) cur -> child[i] = new Trie();
            cur = cur -> child[i];
        }
        cur -> is_word = true;
    }
    
    int search(const string &word)
    {
        Trie *cur = this;
        for (int i = 0; i < word.size(); i++)
        {
            int pos = word[i] - 'a';
            if (!cur -> child[pos]) return -1;
            cur = cur -> child[pos];
            if (cur -> is_word) return i;
        }
        return -1;
    }
};
class Solution 
{
public:
    string replaceWords(vector<string>& dictionary, string sentence) 
    {
        Trie *root = new Trie();
        for (const auto &word : dictionary) root -> insert(word);
        string result, str;
        stringstream ss(sentence);
        while (ss >> str)
        {
            int pos = root -> search(str);
            if (pos == -1) result += str + " ";
            else result += str.substr(0, pos + 1) + " ";
        }
        return result.empty() ? result : result.substr(0, result.size() - 1);
    }
};
```

__Java__:

```Java
class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        String[] words = sentence.split("\\s+");
        StringBuilder result = new StringBuilder();
        for (String word : words) {
            for (String prefix : dictionary) if (word.startsWith(prefix)) word = prefix;
            result.append(word).append(" ");
        }
        return result.toString().trim();
    }
}
```

__Python__:

```Python
class Solution:
    def replaceWords(self, dictionary: List[str], sentence: str) -> str:
        words, result = sentence.split(" "), ""
        for word in words:
            for prefix in dictionary:
                if word.startswith(prefix):
                    word = prefix
            result += word + " "
        return result.strip()
```
