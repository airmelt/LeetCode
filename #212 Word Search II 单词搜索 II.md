__Description__:
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

__Example:__

Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
 

__Note:__

All inputs are consist of lowercase letters a-z.
The values of words are distinct.

__题目描述__:
给定一个二维网格 board 和一个字典中的单词列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

__示例 :__

输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

输出: ["eat","oath"]
说明:
你可以假设所有输入都由小写字母 a-z 组成。

__提示：__

你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： 实现Trie（前缀树）。

__思路__:
参考[LeetCode #208 Implement Trie (Prefix Tree) 实现 Trie (前缀树)](https://www.jianshu.com/p/8d7b8b324079)和[LeetCode #79 Word Search 单词搜索](https://www.jianshu.com/p/26fc42b8784d)
将 words数组中的所有 word插入到前缀树中
再在 board数组中进行 dfs搜索
时间复杂度O(mn * 3 ^ l), 空间复杂度O(lk), 其中 m和 n分别是 board数组的长宽, l是 words数组中的单词长度, k为 words中的单词数

__代码__:
__C++__:
```C++
struct Trie
{
    Trie* children[26];
    string word = "";
    Trie ()
    {
        for (int i = 0; i < 26; i++) children[i] = nullptr;
    }
};
class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> result;
        Trie *t = new Trie();
        for (auto& word : words)
        {
            Trie *cur = t;
            for (auto& c : word)
            {
                if (!cur -> children[c - 'a']) cur -> children[c - 'a'] = new Trie();
                cur = cur -> children[c - 'a'];
            }
            cur -> word = word;
        }
        for (int i = 0; i < board.size(); i++) for (int j = 0; j < board[i].size(); j++) helper(result, board, i, j, t);
        return result;
    }
private:
    void helper(vector<string>& result, vector<vector<char>>& board, int i, int j, Trie* t)
    {
        if (i < 0 || i > board.size() - 1 || j < 0 || j > board[0].size() - 1) return;
        char c = board[i][j];
        if (c == '*' || !t -> children[c - 'a']) return;
        t = t -> children[c - 'a'];
        if (t -> word != "")
        {
            result.push_back(t -> word);
            t -> word = "";
        }
        board[i][j] = '*';
        helper(result, board, i + 1, j, t);
        helper(result, board, i - 1, j, t);
        helper(result, board, i, j + 1, t);
        helper(result, board, i, j - 1, t);
        board[i][j] = c;
        return;
    }
};
```

__Java__:
```Java
class Solution {
    public List<String> findWords(char[][] board, String[] words) {
        Trie t = new Trie();
        TrieNode root = t.root;
        for (String word : words) t.insert(word);
        List<String> result = new ArrayList<>();
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) helper(board, visited, i, j, m, n, result, root);
        return result;
    }

    private void helper(char[][] board, boolean[][] visited, int i, int j, int m, int n, List<String> result, TrieNode cur) {
        if (i < 0 || i > m - 1 || j < 0 || j > n - 1 || visited[i][j] || cur.children[board[i][j] - 'a'] == null) return;
        cur = cur.children[board[i][j] - 'a'];
        visited[i][j] = true;
        if (cur.isWord) {
            result.add(cur.word);
            cur.isWord = false;
        }
        helper(board, visited, i + 1, j, m, n, result, cur);
        helper(board, visited, i - 1, j, m, n, result, cur);
        helper(board, visited, i, j + 1, m, n, result, cur);
        helper(board, visited, i, j - 1, m, n, result, cur);
        visited[i][j] = false;
    }

    class Trie {
        TrieNode root = new TrieNode();

        void insert(String word) {
            TrieNode cur = root;
            for (char c : word.toCharArray()) {
                if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new TrieNode();
                cur = cur.children[c - 'a'];   
            }
            cur.isWord = true;
            cur.word = word;
        }
    }

    class TrieNode {
        String word;
        TrieNode[] children = new TrieNode[26];
        boolean isWord = false;
    }
}
```

__Python__:
```Python
class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        trie = {}
        for word in words:
            t = trie
            for w in word:
                t = t.setdefault(w, {})
            t["end"] = 1
        result = []
        def dfs(i: int, j: int, trie: dict, s: str):
            c = board[i][j]
            if c not in trie: 
                return
            trie = trie[c]
            if "end" in trie and trie["end"]:
                result.append(s + c)
                trie["end"] = 0
            board[i][j] = "#"
            for x, y in zip((-1, 1, 0, 0), (0, 0, 1, -1)):
                if -1 < x + i < len(board) and -1 < y + j < len(board[0]) and board[x + i][y + j] != "#":
                    dfs(x + i, y + j, trie, s + c)
            board[i][j] = c
        for i in range(len(board)):
            for j in range(len(board[0])):
                dfs(i, j, trie, "")
        return result
```