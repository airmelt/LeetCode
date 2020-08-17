__Description__:
Implement a trie with insert, search, and startsWith methods.

__Example:__

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true

__Note:__

You may assume that all inputs are consist of lowercase letters a-z.
All inputs are guaranteed to be non-empty strings.

__题目描述__:
实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

__示例 :__

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true

__说明：__

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。

__思路__:
用一个变量 is_word记录, 方便查找
用一个字符数组记录出现过的字符
查询单词或者前缀时没有这个字符就返回 false
查询返回 is_word
前缀返回 true
Trie就是维护公共前缀的树
时间复杂度O(n), 空间复杂度O(26 ^ n), 其中 n为所查找的字符串的长度

__代码__:
__C++__:
```C++
class Trie 
{
private:
    Trie *child[26];
    bool is_word;
public:
    /** Initialize your data structure here. */
    Trie() 
    {
        is_word = false;
        for (int i = 0; i < 26; i++) child[i] = nullptr;
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) 
    {
        Trie *cur = this;
        for (auto c : word)
        {
            if (!cur -> child[c - 'a']) cur -> child[c - 'a'] = new Trie();
            cur = cur -> child[c - 'a'];
        }
        cur -> is_word = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) 
    {
        Trie *cur = this;
        for (auto c : word)
        {
            if (!cur -> child[c - 'a']) return false;
            cur = cur -> child[c - 'a'];
        }
        return cur -> is_word;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) 
    {
        Trie *cur = this;
        for (auto c : prefix)
        {
            if (!cur -> child[c - 'a']) return false;
            cur = cur -> child[c - 'a'];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

__Java__:
```Java
class Trie {

    private class TrieNode {
        private boolean isWord;
        private TrieNode[] child;
        public TrieNode() {
            isWord = false;
            child = new TrieNode[26];
        }
    }
    
    private TrieNode root;
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode cur = root;
        for (char c : word.toCharArray()) {
            if (cur.child[c - 'a'] == null) cur.child[c - 'a'] = new TrieNode();
            cur = cur.child[c - 'a'];
        }
        cur.isWord = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode cur = root;
        for (char c : word.toCharArray()) {
            if (cur.child[c - 'a'] == null) return false;
            cur = cur.child[c - 'a'];
        }
        return cur.isWord;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode cur = root;
        for (char c : prefix.toCharArray()) {
            if (cur.child[c - 'a'] == null) return false;
            cur = cur.child[c - 'a'];
        }
        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

__Python__:
```Python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self.root
        for c in word:
            if c not in node.keys():
                node[c] = {}
            node = node[c]
        node['is_word'] = True

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.root
        for c in word:
            if c not in node.keys():
                return False
            node = node[c]
        return 'is_word' in node.keys()


    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.root
        for c in prefix:
            if c not in node.keys():
                return False
            node = node[c]
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```