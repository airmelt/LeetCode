# 676 Implement Magic Dictionary 实现一个魔法字典

__Description__:
Design a data structure that is initialized with a list of different words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.

Implement the MagicDictionary class:

MagicDictionary() Initializes the object.
void buildDict(String[] dictionary) Sets the data structure with an array of distinct strings dictionary.
bool search(String searchWord) Returns true if you can change exactly one character in searchWord to match any string in the data structure, otherwise returns false.

__Example:__

Example 1:

Input
["MagicDictionary", "buildDict", "search", "search", "search", "search"]
[[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
Output
[null, null, false, true, false, false]

Explanation

```Java
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // return False
magicDictionary.search("hhllo"); // We can change the second 'h' to 'e' to match "hello" so we return True
magicDictionary.search("hell"); // return False
magicDictionary.search("leetcoded"); // return False
```

__Constraints:__

1 <= dictionary.length <= 100
1 <= dictionary[i].length <= 100
dictionary[i] consists of only lower-case English letters.
All the strings in dictionary are distinct.
1 <= searchWord.length <= 100
searchWord consists of only lower-case English letters.
buildDict will be called only once before search.
At most 100 calls will be made to search.

__题目描述__:
设计一个使用单词列表进行初始化的数据结构，单词列表中的单词 互不相同 。 如果给出一个单词，请判定能否只将这个单词中一个字母换成另一个字母，使得所形成的新单词存在于你构建的字典中。

实现 MagicDictionary 类：

MagicDictionary() 初始化对象
void buildDict(String[] dictionary) 使用字符串数组 dictionary 设定该数据结构，dictionary 中的字符串互不相同
bool search(String searchWord) 给定一个字符串 searchWord ，判定能否只将字符串中 一个 字母换成另一个字母，使得所形成的新字符串能够与字典中的任一字符串匹配。如果可以，返回 true ；否则，返回 false 。

__示例 :__

输入
["MagicDictionary", "buildDict", "search", "search", "search", "search"]
[[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
输出
[null, null, false, true, false, false]

解释

```Java
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // 返回 False
magicDictionary.search("hhllo"); // 将第二个 'h' 替换为 'e' 可以匹配 "hello" ，所以返回 True
magicDictionary.search("hell"); // 返回 False
magicDictionary.search("leetcoded"); // 返回 False
```

__提示:__

1 <= dictionary.length <= 100
1 <= dictionary[i].length <= 100
dictionary[i] 仅由小写英文字母组成
dictionary 中的所有字符串 互不相同
1 <= searchWord.length <= 100
searchWord 仅由小写英文字母组成
buildDict 仅在 search 之前调用一次
最多调用 100 次 search

__思路__:

1. 暴力法
按照单词的不同长度将单词放到哈希表中
搜索时遍历哈希表, 找到只有 1 个不同的字母的单词
build() 时间复杂度 O(s), search() 时间复杂度 O(mn), 空间复杂度 O(s), 其中 s 为所有单词字母总数, n 为魔法字典单词数, m 为搜索单词的长度
2. 前缀树
build() 时间复杂度 O(s), search() 时间复杂度 O(mn), 空间复杂度 O(s), 其中 s 为所有单词字母总数, n 为魔法字典单词数, m 为搜索单词的长度
3. 替代法
将单词的每一个字母用 '*' 代替之后存入哈希表
搜索时搜索长度相同的哈希表中的单词, 同样将待搜索单词的每一个字母替换成 '*'
搜索匹配且搜索单词不在哈希表中
build() 时间复杂度 O(s ^ 2), search() 时间复杂度 O(mn), 空间复杂度 O(s ^ 2), 其中 s 为单词字母数, n 为魔法字典单词数, m 为搜索单词的长度

__代码__:
__C++__:

```C++
class Trie
{
public:
    bool is_word;
    vector<Trie*> children;
    Trie() : is_word(false), children(26, nullptr) {};
    ~Trie() 
    {
        for (Trie* child : children) if (child) delete child;
    }
};
class MagicDictionary 
{
private:
    Trie *root;
    
    void add(string &word)
    {
        Trie *cur = root;
        for (auto c : word)
        {
            if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new Trie();
            cur = cur -> children[c - 'a'];
        }
        cur -> is_word = true;
    }
    
    bool helper(Trie *cur, string &word, int start, bool changed)
    {
        if (!cur) return false;
        if (start == word.size()) return changed and cur -> is_word;
        else
        {
            for (int i = 0; i < 26; i++)
            {
                if (cur -> children[i])
                {
                    if ('a' + i == word[start]) 
                    {
                        if (helper(cur -> children[i], word, start + 1, changed)) return true;
                    }
                    else if (!changed and (helper(cur -> children[i], word, start + 1, true))) return true;
                }
            }
        }
        return false;
    }
public:
    /** Initialize your data structure here. */
    MagicDictionary() 
    {
        root = new Trie();
    }
    
    void buildDict(vector<string> dictionary) 
    {
        for (auto &word : dictionary) add(word);
    }
    
    bool search(string searchWord) 
    {
        return helper(root, searchWord, 0, false);
    }
};

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */
```

__Java__:

```Java
class MagicDictionary {
    private Map<Integer, List<String>> map;

    /** Initialize your data structure here. */
    public MagicDictionary() {
        map = new HashMap<Integer, List<String>>();
    }
    
    public void buildDict(String[] dictionary) {
        for (String word : dictionary) {
            if (map.containsKey(word.length())) map.get(word.length()).add(word);
            else {
                List<String> list = new ArrayList<>();
                list.add(word);
                map.put(word.length(), list);
            }
        }
    }
    
    public boolean search(String searchWord) {
        for (String word : map.getOrDefault(searchWord.length(), new ArrayList<String>())) {
            int cur = 0;
            for (int i = 0; i < searchWord.length(); i++) if (word.charAt(i) != searchWord.charAt(i)) ++cur;
            if (cur == 1) return true;
        }
        return false;
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```

__Python__:

```Python
class MagicDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.d = defaultdict(set)
        

    def buildDict(self, dictionary: List[str]) -> None:
        for word in dictionary:
            self.d[len(word)].add(word)


    def search(self, searchWord: str) -> bool:
        return any(sum(word[i] != searchWord[i] for i in range(len(searchWord))) == 1 for word in self.d[len(searchWord)])


# Your MagicDictionary object will be instantiated and called as such:
# obj = MagicDictionary()
# obj.buildDict(dictionary)
# param_2 = obj.search(searchWord)
```
