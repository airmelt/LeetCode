# 677 Map Sum Pairs 键值映射

__Description__:
Implement the MapSum class:

MapSum() Initializes the MapSum object.
void insert(String key, int val) Inserts the key-val pair into the map. If the key already existed, the original key-value pair will be overridden to the new one.
int sum(string prefix) Returns the sum of all the pairs' value whose key starts with the prefix.

__Example:__

Example 1:

Input
["MapSum", "insert", "sum", "insert", "sum"]
[[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
Output
[null, null, 3, null, 5]

Explanation

```Java
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // return 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // return 5 (apple + app = 3 + 2 = 5)
```

__Constraints:__

1 <= key.length, prefix.length <= 50
key and prefix consist of only lowercase English letters.
1 <= val <= 1000
At most 50 calls will be made to insert and sum.

__题目描述__:
实现一个 MapSum 类，支持两个方法，insert 和 sum：

MapSum() 初始化 MapSum 对象
void insert(String key, int val) 插入 key-val 键值对，字符串表示键 key ，整数表示值 val 。如果键 key 已经存在，那么原来的键值对将被替代成新的键值对。
int sum(string prefix) 返回所有以该前缀 prefix 开头的键 key 的值的总和。

__示例 :__

输入：
["MapSum", "insert", "sum", "insert", "sum"]
[[], ["apple", 3], ["ap"], ["app", 2], ["ap"]]
输出：
[null, null, 3, null, 5]

解释：

```Java
MapSum mapSum = new MapSum();
mapSum.insert("apple", 3);  
mapSum.sum("ap");           // return 3 (apple = 3)
mapSum.insert("app", 2);    
mapSum.sum("ap");           // return 5 (apple + app = 3 + 2 = 5)
```

__提示:__

1 <= key.length, prefix.length <= 50
key 和 prefix 仅由小写英文字母组成
1 <= val <= 1000
最多调用 50 次 insert 和 sum

__思路__:

前缀树
构建一个前缀树
插入的时候每次都将 val 值更新
查询的时候从 prefix 的结尾开始查找所有存在与前缀树的单词的值
插入和查询时间复杂度 O(n), 空间复杂度 O(n)

__代码__:
__C++__:

```C++
class Trie
{
public:
    bool is_word;
    int val;
    vector<Trie*> children;
    Trie() : is_word(false), children(26, nullptr), val(0) {}
    ~Trie()
    {
        for (auto child : children) if (child) delete child;
    }
};

class MapSum 
{
private:
    Trie *root;
    
    int helper(Trie *cur) 
    {
        int result = 0;
        if (!cur) return result;
        if (cur -> is_word) result += cur -> val;
        for (int i = 0; i < 26; i++) if (cur -> children[i]) result += helper(cur -> children[i]);
        return result;
    }
public:
    /** Initialize your data structure here. */
    MapSum() 
    {
        root = new Trie();
    }
    
    void insert(string key, int val) 
    {
        Trie *cur = root;
        for (const auto &c : key) 
        {
            if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new Trie();
            cur = cur -> children[c - 'a'];
        }
        cur -> is_word = true;
        cur -> val = val;
    }
    
    int sum(string prefix) 
    {
        Trie *cur = root;
        for (const auto &c : prefix) 
        {
            if (cur -> children[c - 'a']) cur = cur -> children[c - 'a'];
            else return 0;
        }
        return helper(cur);
    }
};

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum* obj = new MapSum();
 * obj->insert(key,val);
 * int param_2 = obj->sum(prefix);
 */
```

__Java__:

```Java
class Trie {
    public boolean isWord;
    public Trie[] children;
    public int val;
    
    Trie() {
        isWord = false;
        children = new Trie[26];
        val = 0;
    }
}
class MapSum {

    private Trie root;
    /** Initialize your data structure here. */
    public MapSum() {
        root = new Trie();
    }
    
    public void insert(String key, int val) {
        Trie cur = root;
        for (char c : key.toCharArray()) {
            if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new Trie();
            cur = cur.children[c - 'a'];
        }
        cur.isWord = true;
        cur.val = val;
    }
    
    public int sum(String prefix) {
        Trie cur = root;
        for (char c : prefix.toCharArray()) {
            if (cur.children[c - 'a'] != null) cur = cur.children[c - 'a'];
            else return 0;
        }
        return helper(cur);
    }
    
    private int helper(Trie cur) {
        int result = 0;
        if (cur == null) return result;
        if (cur.isWord) result += cur.val;
        for (int i = 0; i < 26; i++) if (cur.children[i] != null) result += helper(cur.children[i]);
        return result;
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```

__Python__:

```Python
class Trie:
    def __init__(self):
        self.children = [None] * 26
        self.is_word = False
        self.val = 0
        
class MapSum:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = Trie()


    def insert(self, key: str, val: int) -> None:
        cur = self.root
        for c in key:
            if not cur.children[ord(c) - ord('a')]:
                cur.children[ord(c) - ord('a')] = Trie()
            cur = cur.children[ord(c) - ord('a')]
        cur.is_word, cur.val = True, val


    def sum(self, prefix: str) -> int:
        cur = self.root
        for c in prefix:
            if cur.children[ord(c) - ord('a')]:
                cur = cur.children[ord(c) - ord('a')]
            else:
                return 0
            
        def helper(cur: Trie) -> int:
            result = 0
            if not cur:
                return result
            if cur.is_word:
                result += cur.val
            for i in range(26):
                if cur.children[i]:
                    result += helper(cur.children[i])
            return result
        
        return helper(cur)

# Your MapSum object will be instantiated and called as such:
# obj = MapSum()
# obj.insert(key,val)
# param_2 = obj.sum(prefix)
```
