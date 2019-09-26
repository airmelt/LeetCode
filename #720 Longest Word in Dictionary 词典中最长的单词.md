__Description__:
Given a list of strings words representing an English Dictionary, find the longest word in words that can be built one character at a time by other words in words. If there is more than one possible answer, return the longest word with the smallest lexicographical order.

If there is no answer, return the empty string.

__Example:__
Example 1:

Input: 
words = ["w","wo","wor","worl", "world"]
Output: "world"
Explanation: 
The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".

Example 2:

Input: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
Output: "apple"
Explanation: 
Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".

__Note:__

All the strings in the input will only contain lowercase letters.
The length of words will be in the range [1, 1000].
The length of words[i] will be in the range [1, 30].

__题目描述__:
给出一个字符串数组words组成的一本英语词典。从中找出最长的一个单词，该单词是由words词典中其他单词逐步添加一个字母组成。若其中有多个可行的答案，则返回答案中字典序最小的单词。

若无答案，则返回空字符串。

__示例 :__
示例 1:

输入: 
words = ["w","wo","wor","worl", "world"]
输出: "world"
解释: 
单词"world"可由"w", "wo", "wor", 和 "worl"添加一个字母组成。

示例 2:

输入: 
words = ["a", "banana", "app", "appl", "ap", "apply", "apple"]
输出: "apple"
解释: 
"apply"和"apple"都能由词典中的单词组成。但是"apple"得字典序小于"apply"。

__注意：__

所有输入的字符串都只包含小写字母。
words数组长度范围为[1,1000]。
words[i]的长度范围为[1,30]。

__思路__:
1. 前缀树, 参考[LeetCode #14 Longest Common Prefix 最长公共前缀](https://www.jianshu.com/p/64dfea263509), 加上一个记录是否为单词的标志
时间复杂度O(n), 空间复杂度O(n)
2. 先按字典序排序, 遍历取第一个出现的最长单词即可
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:
```
class PreTree {
public:
    bool is_word;
    vector<PreTree*> children;
    PreTree(): is_word(false), children(26, nullptr) {}
};
class Solution {
public:
    string longestWord(vector<string>& words) {
        preTree = new PreTree();
        for (auto &word : words) insert(word);
        string temp = "";
        search_word(preTree, temp);
        return result;
    }
private:
    string result = "";
    PreTree *preTree;
    void insert(string &word) {
        PreTree *pt = preTree;
        for (auto c : word) {
            if (!pt -> children[c - 'a']) pt -> children[c - 'a'] = new PreTree();
            pt = pt -> children[c - 'a'];
        }
        pt -> is_word = true;
    }
    void search_word(PreTree *pt, string &word) {
        if (pt) {
            for (int i = 0; i < 26; i++) {
                if (pt -> children[i] && pt ->children[i] -> is_word) {
                    string temp = word + char('a' + i);
                    if (temp.size() > result.size()) result = temp;
                    search_word(pt -> children[i], temp);
                }
            }
        }
    }
};
```

__Java__:
```
class Solution {
    public String longestWord(String[] words) {
        Arrays.sort(words);
        String result = "";
        Set<String> set = new HashSet<>(words.length);
        for (String word : words) {
            if (word.length() == 1 || set.contains(word.substring(0, word.length() - 1))) {
                result = word.length() > result.length() ? word : result;
                set.add(word);
            }
        }
        return result;
    }
}
```

__Python__:
```
class Solution:
    def longestWord(self, words: List[str]) -> str:
        words, result, s = sorted(words), "", set()
        for word in words:
            if len(word) == 1 or word[:-1] in s:
                if len(word) > len(result):
                    result = word
                s.add(word)
        return result
```