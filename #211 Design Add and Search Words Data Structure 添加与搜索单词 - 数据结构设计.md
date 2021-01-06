# 211 Design Add and Search Words Data Structure 添加与搜索单词 - 数据结构设计

__Description__:
You should design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the WordDictionary class:

```text
WordDictionary() Initializes the object.
void addWord(word) adds word to the data structure, it can be matched later.
bool search(word) returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.
```

__Example:__

Input
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
Output
[null,null,null,null,false,true,true,true]

Explanation

```Java
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

__Constraints:__

1 <= word.length <= 500
word in addWord consists lower-case English letters.
word in search consist of  '.' or lower-case English letters.
At most 50000 calls will be made to addWord and search .

__题目描述__:
如果数据结构中有任何与word匹配的字符串，则bool search（word）返回true，否则返回false。 单词可能包含点“。” 点可以与任何字母匹配的地方。

请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。

实现词典类 WordDictionary ：

```text
WordDictionary() 初始化词典对象
void addWord(word) 将 word 添加到数据结构中，之后可以对它进行匹配
bool search(word) 如果数据结构中存在字符串与 word 匹配，则返回 true ；否则，返回  false 。word 中可能包含一些 '.' ，每个 . 都可以表示任何一个字母。
```

__示例 :__

输入：
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
输出：
[null,null,null,null,false,true,true,true]

解释：

```Java
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
```

__提示：__

1 <= word.length <= 500
addWord 中的 word 由小写英文字母组成
search 中的 word 由 '.' 或小写英文字母组成
最调用多 50000 次 addWord 和 search

__思路__:

参考[LeetCode #208 Implement Trie (Prefix Tree) 实现 Trie (前缀树)](https://www.jianshu.com/p/8d7b8b324079)
add操作时间复杂度O(n), search操作时间复杂度O(nm), 空间复杂度O(nm), 其中 n为单词长度, m为单词个数

__代码__:
__C++__:

```C++
class WordDictionary 
{
private:
    WordDictionary* children[26];
    bool is_word;
public:
    /** Initialize your data structure here. */
    WordDictionary() 
    {
        for (int i = 0; i < 26; i++) children[i] = nullptr;
        is_word = false;
    }
    
    /** Adds a word into the data structure. */
    void addWord(string word)
    {
        WordDictionary *cur = this;
        for (auto& c : word)
        {
            if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new WordDictionary();
            cur = cur -> children[c - 'a'];
        }
        cur -> is_word = true;
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    bool search(string word)
    {
        if (word.empty()) return is_word;
        WordDictionary *cur = this;
        for (int i = 0; i < word.size(); i++)
        {
            auto &c = word[i];
            if (c == '.')
            {
                for (int j = 0; j < 26; j++) if(cur -> children[j]) if (cur -> children[j] -> search(word.substr(i + 1))) return true;
                return false;
            }
            else if (cur -> children[c - 'a']) cur = cur -> children[c - 'a'];
            else return false;
        }
        return cur -> is_word;
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```

__Java__:

```Java
class WordDictionary {

    private Trie trie;
    
    private class Trie {
        TrieNode root;
        
        Trie() {
            root = new TrieNode();
        }
        
        void insert(String word) {
            TrieNode node = root;
            for (int i = 0; i < word.length(); i++) {
                if (!node.containsKey(word.charAt(i))) node.put(word.charAt(i), new TrieNode());
                node = node.get(word.charAt(i));
            }
            node.isWord = true;
        }
        
        boolean helper(TrieNode root, String word, int index) {
            if (index >= word.length()) return root.isWord;
            if (word.charAt(index) != '.') {
                if (root.containsKey(word.charAt(index))) return helper(root.get(word.charAt(index)), word, index + 1);
                return false;
            }
            for (TrieNode child : root.children) if (child != null && helper(child, word, index + 1)) return true;
            return false;
        } 
        
        boolean search(String word) {
            return helper(root, word, 0);
        }
    }
    
    private class TrieNode {
        TrieNode[] children;
        boolean isWord;
        TrieNode() {
            children = new TrieNode[26];
        }
        
        boolean containsKey(char c) {
            return children[c - 'a'] != null;
        }
        
        TrieNode get(char c) {
            return children[c - 'a'];
        }
        
        void put(char c, TrieNode node) {
            children[c - 'a'] = node;
        }
    }
    /** Initialize your data structure here. */
    public WordDictionary() {
        trie = new Trie();
    }
    
    /** Adds a word into the data structure. */
    public void addWord(String word) {
        trie.insert(word);
    }
    
    /** Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. */
    public boolean search(String word) {
        return trie.search(word);
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```

__Python__:

```Python
class WordDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = defaultdict(list)


    def addWord(self, word: str) -> None:
        """
        Adds a word into the data structure.
        """
        self.root[len(word)].append(word)


    def search(self, word: str) -> bool:
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        """
        def helper(word: str, in_list: List[str]) -> bool:
            return False if not in_list else True if not word or word in in_list else helper(word[1:], [i[1:] for i in in_list if word[0] == "." or i[0] == word[0]])
        return helper(word, self.root[len(word)])


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```
