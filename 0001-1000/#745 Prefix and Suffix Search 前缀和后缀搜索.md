# 745 Prefix and Suffix Search 前缀和后缀搜索

__Description__:
Design a special dictionary with some words that searchs the words in it by a prefix and a suffix.

Implement the WordFilter class:

WordFilter(string[] words) Initializes the object with the words in the dictionary.
f(string prefix, string suffix) Returns the index of the word in the dictionary, which has the prefix prefix and the suffix suffix. If there is more than one valid index, return the largest of them. If there is no such word in the dictionary, return -1.

__Example:__

Example 1:

Input
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
Output
[null, 0]

Explanation

```Java
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".
```

__Constraints:__

1 <= words.length <= 15000
1 <= words[i].length <= 10
1 <= prefix.length, suffix.length <= 10
words[i], prefix and suffix consist of lower-case English letters only.
At most 15000 calls will be made to the function f.

__题目描述__:
设计一个包含一些单词的特殊词典，并能够通过前缀和后缀来检索单词。

实现 WordFilter 类：

WordFilter(string[] words) 使用词典中的单词 words 初始化对象。
f(string prefix, string suffix) 返回词典中具有前缀 prefix 和后缀suffix 的单词的下标。如果存在不止一个满足要求的下标，返回其中 最大的下标 。如果不存在这样的单词，返回 -1 。

__示例 :__

输入：
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
输出：
[null, 0]

解释：

```Java
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // 返回 0 ，因为下标为 0 的单词的 prefix = "a" 且 suffix = 'e" 。
```

__提示:__

1 <= words.length <= 15000
1 <= words[i].length <= 10
1 <= prefix.length, suffix.length <= 10
words[i]、prefix 和 suffix 仅由小写英文字母组成
最多对函数 f 进行 15000 次调用

__思路__:

前缀树
将前缀和后缀用分隔符, 如 "#" 连在一起
即可用普通的前缀树求解
时间复杂度为 O(mn ^ 2), 空间复杂度为 O(mn ^ 2), m 为 words 的长度, n 为 word 的长度

__代码__:
__C++__:

```C++
class Trie
{
private:
    Trie *children[27]{ nullptr };
    int val = 0;
public:
    
    void insert(string &word, int start, int cur_val)
    {
        Trie *p = this;
        for (int i = start, s = word.size(); i < s; i++)
        {
            int cur = word[i] == '#' ? 26 : word[i] - 'a';
            if (!p -> children[cur]) p -> children[cur] = new Trie;
            p = p -> children[cur];
            p -> val = max(p -> val, cur_val);
        }
    }
    
    int search(string &word)
    {
        Trie *p = this;
        for (int i = 0, s = word.size(); i < s; i++)
        {
            int cur = word[i] == '#' ? 26 : word[i] - 'a';
            if (!p -> children[cur]) return -1;
            else p = p -> children[cur];
        }
        return p -> val;
    }
};
class WordFilter 
{
private:
    Trie root;
public:
    WordFilter(vector<string>& words) 
    {
        string cur;
        for (int i = 0, s = words.size(); i < s; i++)
        {
            cur = words[i] + "#" + words[i];
            for (int j = 0, t = words[i].size(); j <= t; j++) root.insert(cur, j, i);
        }
    }
    
    int f(string prefix, string suffix) 
    {
        string target = suffix + "#" + prefix;
        return root.search(target);
    }
    
};

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(prefix,suffix);
 */
```

__Java__:

```Java
class WordFilter {
    private Map<String, Integer> map = new HashMap<>();
    public WordFilter(String[] words) {
        for (int i = 0; i < words.length; i++) for (int j = 1; j <= words[i].length(); j++) for (int k = 1; k <= words[i].length(); k++) map.put(words[i].substring(0, j) + "#" + words[i].substring(words[i].length() - k, words[i].length()), i);
    }
        
    public int f(String prefix, String suffix) {
        return map.getOrDefault(prefix + "#" + suffix, -1);
    }
}

/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter obj = new WordFilter(words);
 * int param_1 = obj.f(prefix,suffix);
 */
```

__Python__:

```Python
class WordFilter:

    def __init__(self, words: List[str]):
        self.d = {}
        for i, w in enumerate(words):
            for j in range(0, len(w) + 1):
                p = w[:j]
                for k in range(0, len(w) + 1):
                    q = w[k:]
                    self.d[(p, q)] = i


    def f(self, prefix: str, suffix: str) -> int:
        return self.d.get((prefix, suffix), -1)



# Your WordFilter object will be instantiated and called as such:
# obj = WordFilter(words)
# param_1 = obj.f(prefix,suffix)
```
