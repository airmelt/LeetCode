# 472 Concatenated Words 连接词

__Description__:
Given an array of strings words (without duplicates), return all the concatenated words in the given list of words.

A concatenated word is defined as a string that is comprised entirely of at least two shorter words in the given array.

__Example:__

Example 1:

Input: words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
Output: ["catsdogcats","dogcatsdog","ratcatdogcat"]
Explanation: "catsdogcats" can be concatenated by "cats", "dog" and "cats";
"dogcatsdog" can be concatenated by "dog", "cats" and "dog";
"ratcatdogcat" can be concatenated by "rat", "cat", "dog" and "cat".

Example 2:

Input: words = ["cat","dog","catdog"]
Output: ["catdog"]

__Constraints:__

1 <= words.length <= 10^4
0 <= words[i].length <= 1000
words[i] consists of only lowercase English letters.
0 <= sum(words[i].length) <= 6 * 10^5

__题目描述__:
给定一个 不含重复 单词的字符串数组 words ，编写一个程序，返回 words 中的所有 连接词 。

连接词 的定义为：一个字符串完全是由至少两个给定数组中的单词组成的。

__示例 :__

示例 1：

输入：words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
输出：["catsdogcats","dogcatsdog","ratcatdogcat"]
解释："catsdogcats"由"cats", "dog" 和 "cats"组成;
     "dogcatsdog"由"dog", "cats"和"dog"组成;
     "ratcatdogcat"由"rat", "cat", "dog"和"cat"组成。

示例 2：

输入：words = ["cat","dog","catdog"]
输出：["catdog"]

__提示:__

1 <= words.length <= 10^4
0 <= words[i].length <= 1000
words[i] 仅由小写字母组成
0 <= sum(words[i].length) <= 6 * 10^5

__思路__:

前缀树 Trie
先建立每个单词的前缀树
按照字符串的长度进行排序
先搜索短的, 如果搜索到单词结尾, 搜索剩下的字符串是否也在前缀树中
时间复杂度O(nlgn * m ^ 3), 空间复杂度O(n), 其中 n为单词数, m为单词的最长的长度

__代码__:
__C++__:

```C++
struct Trie
{
    bool word;
    struct Trie* next[26]{nullptr};
};
class Solution 
{
public:
    vector<string> findAllConcatenatedWordsInADict(vector<string>& words) 
    {
        sort(words.begin(), words.end(), [](auto &a, auto &b) { return a.size() < b.size(); });
        vector<string> result;
        for (auto &word: words) 
        {
            if (word.empty()) continue;
            if (search(word)) result.emplace_back(word);
            else insert(word);
        }
        return result;
    }
private:
    Trie* root = new Trie();
    
    bool search(string &word) 
    {
        Trie *cur = root;
        if (word.empty()) return true;
        for (int i = 0, n = word.size(), c; i < n; i++) 
        {
            c = word[i] - 'a';
            string child = word.substr(i + 1);
            if (!cur -> next[c]) return false;
            if (cur -> next[c] -> word) if (search(child)) return true;
            cur = cur -> next[c];
        }
        return false;
    }

    void insert(string &word) 
    {
        Trie *cur = root;
        for (int i = 0, n = word.size(), c; i < n; i++) 
        {
            c = word[i] - 'a';
            if (!cur -> next[c]) cur -> next[c] = new Trie();
            cur = cur -> next[c];
        }
        cur -> word = true;
    }
};
```

__Java__:

```Java
class Trie {
    public boolean word = false;
    public Trie[] next = new Trie[26];
}

class Solution {
    Trie root = new Trie();
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        Arrays.sort(words, new Comparator<String>() {
            @Override
            public int compare(String a, String b) {
                return a.length() - b.length();
            } 
        });
        List<String> result = new ArrayList<>();
        for (String word: words) {
            if (word.length() == 0) continue;
            if (search(word)) result.add(word);
            else insert(word);
        }
        return result;
    }

    public boolean search(String word) {
        Trie cur = root;
        int n = word.length();
        if (n == 0) return true;
        for (int i = 0, c; i < n; i++) {
            c = word.charAt(i) - 'a';
            if (cur.next[c] == null) return false;
            if (cur.next[c].word) if (search(word.substring(i + 1))) return true;
            cur = cur.next[c];
        }
        return false;
    }

    public void insert(String word) {
        Trie cur = root;
        for (int i = 0, n = word.length(), c; i < n; i++) {
            c = word.charAt(i) - 'a';
            if (cur.next[c] == null) cur.next[c] = new Trie();
            cur = cur.next[c];
        }
        cur.word = true;
    }
}
```

__Python__:

```Python
class Solution:
    
    class Trie:
        def __init__(self):
            self.next = [None] * 26
            self.word = False
            
            
    def findAllConcatenatedWordsInADict(self, words: List[str]) -> List[str]:
        words.sort(key=len)
        root = self.Trie()
        result = []
        
        def search(word: str) -> bool:
            if not word:
                return True
            cur, n = root, len(word)
            for i in range(n):
                c = ord(word[i]) - ord('a')
                if not cur.next[c]:
                    return False
                if cur.next[c].word:
                    if search(word[i + 1:]):
                        return True
                cur = cur.next[c]
            return False
        
        def insert(word: str) -> None:
            cur, n = root, len(word)
            for i in range(n):
                c = ord(word[i]) - ord('a')
                if not cur.next[c]:
                    cur.next[c] = self.Trie()
                cur = cur.next[c]
            cur.word = True
            
        for word in words:
            if not word:
                continue
            if search(word):
                result.append(word)
            else:
                insert(word)
        return result
```
