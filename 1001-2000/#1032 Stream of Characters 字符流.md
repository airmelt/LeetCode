# 1032 Stream of Characters 字符流

__Description__:
Design an algorithm that accepts a stream of characters and checks if a suffix of these characters is a string of a given array of strings words.

For example, if words = ["abc", "xyz"] and the stream added the four characters (one by one) 'a', 'x', 'y', and 'z', your algorithm should detect that the suffix "xyz" of the characters "axyz" matches "xyz" from words.

Implement the StreamChecker class:

StreamChecker(String[] words) Initializes the object with the strings array words.
boolean query(char letter) Accepts a new character from the stream and returns true if any non-empty suffix from the stream forms a word that is in words.

__Example:__

Example 1:

Input
["StreamChecker", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query"]
[[["cd", "f", "kl"]], ["a"], ["b"], ["c"], ["d"], ["e"], ["f"], ["g"], ["h"], ["i"], ["j"], ["k"], ["l"]]
Output
[null, false, false, false, true, false, true, false, false, false, false, false, true]

Explanation

```Java
StreamChecker streamChecker = new StreamChecker(["cd", "f", "kl"]);
streamChecker.query("a"); // return False
streamChecker.query("b"); // return False
streamChecker.query("c"); // return False
streamChecker.query("d"); // return True, because 'cd' is in the wordlist
streamChecker.query("e"); // return False
streamChecker.query("f"); // return True, because 'f' is in the wordlist
streamChecker.query("g"); // return False
streamChecker.query("h"); // return False
streamChecker.query("i"); // return False
streamChecker.query("j"); // return False
streamChecker.query("k"); // return False
streamChecker.query("l"); // return True, because 'kl' is in the wordlist
```

__Constraints:__

1 <= words.length <= 2000
1 <= words[i].length <= 2000
words[i] consists of lowercase English letters.
letter is a lowercase English letter.
At most 4 * 10^4 calls will be made to query.

__题目描述__:
设计一个算法：接收一个字符流，并检查这些字符的后缀是否是字符串数组 words 中的一个字符串。

例如，words = ["abc", "xyz"] 且字符流中逐个依次加入 4 个字符 'a'、'x'、'y' 和 'z' ，你所设计的算法应当可以检测到 "axyz" 的后缀 "xyz" 与 words 中的字符串 "xyz" 匹配。

按下述要求实现 StreamChecker 类：

StreamChecker(String[] words) ：构造函数，用字符串数组 words 初始化数据结构。
boolean query(char letter)：从字符流中接收一个新字符，如果字符流中的任一非空后缀能匹配 words 中的某一字符串，返回 true ；否则，返回 false。

__示例 :__

输入：
["StreamChecker", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query", "query"]
[[["cd", "f", "kl"]], ["a"], ["b"], ["c"], ["d"], ["e"], ["f"], ["g"], ["h"], ["i"], ["j"], ["k"], ["l"]]
输出：
[null, false, false, false, true, false, true, false, false, false, false, false, true]

解释：

```Java
StreamChecker streamChecker = new StreamChecker(["cd", "f", "kl"]);
streamChecker.query("a"); // 返回 False
streamChecker.query("b"); // 返回 False
streamChecker.query("c"); // 返回n False
streamChecker.query("d"); // 返回 True ，因为 'cd' 在 words 中
streamChecker.query("e"); // 返回 False
streamChecker.query("f"); // 返回 True ，因为 'f' 在 words 中
streamChecker.query("g"); // 返回 False
streamChecker.query("h"); // 返回 False
streamChecker.query("i"); // 返回 False
streamChecker.query("j"); // 返回 False
streamChecker.query("k"); // 返回 False
streamChecker.query("l"); // 返回 True ，因为 'kl' 在 words 中
```

__提示:__

1 <= words.length <= 2000
1 <= words[i].length <= 2000
words[i] 由小写英文字母组成
letter 是一个小写英文字母
最多调用查询 4 * 10^4 次

__思路__:

前缀树
反向加入 words 中的所有单词到前缀树中
查询时反向查询, 只要前缀匹配立即返回 true, 否则返回 false
建立前缀树时间复杂度为 O(ns), 空间复杂度为 O(n), n 为 words 数组的长度, s 为 words 中字符串平均长度, 查询时间复杂度为 O(m), m 为当前查询所有的字符流总长度

__代码__:
__C++__:

```C++
struct Node
{
    Node* children[26];
    bool is_word;
    Node()
    {
        is_word = false;
        memset(children, 0, sizeof(children));
    }
};

class StreamChecker 
{
private:
    Node* root;
    string history = "";
public:
    StreamChecker(vector<string>& words) 
    {
        root = new Node();
        for (const auto& word : words)
        {
            Node* cur = root;
            for (int i = word.size() - 1; i >= 0; --i)
            {
                if (!cur -> children[word[i] - 'a']) cur->children[word[i]-'a'] = new Node();
                cur = cur -> children[word[i] - 'a'];
            }
            cur -> is_word = true;
        }
    }
    
    bool query(char letter) 
    {
        history += letter;
        Node* cur = root;
        for (int i = history.size() - 1; i >= 0; --i)
        {
            int c = history[i] - 'a';
            if (!cur -> children[c]) return false;
            else
            {
                cur = cur -> children[c];
                if (cur -> is_word) return true;
            }
        }
        return false;
    }
};

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker* obj = new StreamChecker(words);
 * bool param_1 = obj->query(letter);
 */
```

__Java__:

```Java
public class StreamChecker {
    private class Node {
        private boolean isWorld;
        private Node[] next;

        public Node(boolean isWorld) {
            this.isWorld = isWorld;
            next = new Node[26];
        }

        public Node() {
            this(false);
        }
    }
    private Node root;
    private StringBuilder sb;

    public void insert(String word) {
        Node cur = root;
        for (int i = word.length() - 1; i >= 0; i--) {
            char c = word.charAt(i);
            if (cur.next[c - 'a'] == null) cur.next[c - 'a'] = new Node();
            cur = cur.next[c - 'a'];
        }
        cur.isWorld = true;
    }

    public StreamChecker(String[] words) {
        root = new Node();
        for (String s : words) insert(s);
        sb = new StringBuilder();
    }

    public boolean query(char letter) {
        sb.append(letter);
        Node cur = root;
        for (int i = sb.length() - 1; i >= 0; i--) {
            Node node = cur.next[sb.charAt(i) - 'a'];
            if (node == null) return false;
            cur = node;
            if (cur.isWorld) return true;
        }
        return false;
    }
}

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker obj = new StreamChecker(words);
 * boolean param_1 = obj.query(letter);
 */
```

__Python__:

```Python
class Node:
    def __init__(self, val: str):
        self.val = val
        self.children = {}

class Trie:
    def __init__(self):
        self.root = Node(False)
    
    def add(self, word: str) -> None:
        cur = self.root
        for c in word:
            cur = cur.children.setdefault(c, Node(False))
        cur.val = True
    
    def check(self, word: List[str]) -> bool:
        cur = self.root
        for c in word:
            if c not in cur.children:
                return False
            cur = cur.children[c]
            if cur.val:
                return True
        return False

class StreamChecker:

    def __init__(self, words: List[str]):
        self.trie = Trie()
        for word in words:
            self.trie.add(word[::-1])
        self.history = []
        

    def query(self, letter: str) -> bool:
        self.history.append(letter)
        return self.trie.check(reversed(self.history))



# Your StreamChecker object will be instantiated and called as such:
# obj = StreamChecker(words)
# param_1 = obj.query(letter)
```
