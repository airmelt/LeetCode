# 336 Palindrome Pairs 回文对

__Description__:
Given a list of unique words, return all the pairs of the distinct indices (i, j) in the given list, so that the concatenation of the two words words[i] + words[j] is a palindrome.

__Example:__

Example 1:

Input: words = ["abcd","dcba","lls","s","sssll"]
Output: [[0,1],[1,0],[3,2],[2,4]]
Explanation: The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]

Example 2:

Input: words = ["bat","tab","cat"]
Output: [[0,1],[1,0]]
Explanation: The palindromes are ["battab","tabbat"]

Example 3:

Input: words = ["a",""]
Output: [[0,1],[1,0]]

__Constraints:__

1 <= words.length <= 5000
0 <= words[i] <= 300
words[i] consists of lower-case English letters.

__题目描述__:
给定一组 互不相同 的单词， 找出所有不同 的索引对(i, j)，使得列表中的两个单词， words[i] + words[j] ，可拼接成回文串。

__示例 :__

示例 1：

输入：["abcd","dcba","lls","s","sssll"]
输出：[[0,1],[1,0],[3,2],[2,4]]
解释：可拼接成的回文串为 ["dcbaabcd","abcddcba","slls","llssssll"]

示例 2：

输入：["bat","tab","cat"]
输出：[[0,1],[1,0]]
解释：可拼接成的回文串为 ["battab","tabbat"]

__思路__:

1. 暴力解
两层遍历, 查询每次组成的字符串是否是回文串
时间复杂度O(mn ^ 2), 空间复杂度O(m), n为字符串数组的长度, m为字符串的平均长度
2. 字典树(Trie)
对于字符串数组中的两个字符串 s, t, 若 s + t为回文串
若 s.size() == t.size(), 则 reverse(s) == t
若 s.size() > t.size(), 令 s = s1 + s2, 则 reverse(s1) == t, reverse(s2) == s2
若 s.size() < t.size(), 令 t = t1 + t2, 则 s == reverse(t2), reverse(t1) == t1
使用字典树存储所有的翻转的回文前缀和所有翻转单词的下标
时间复杂度O(mn ^ 2), 空间复杂度O(m), n为字符串数组的长度, m为字符串的平均长度
3. 字典树(Trie) + 马拉车算法
在 2的基础上使用马拉车算法减少时间复杂度
时间复杂度O(mn), 空间复杂度O(mn), n为字符串数组的长度, m为字符串的平均长度

__代码__:
__C++__:

```C++
struct Trie 
{
    struct node 
    {
        int ch[26];
        int flag;
        node() 
        {
            flag = -1;
            memset(ch, 0, sizeof(ch));
        }
    };

    vector<node> tree;

    Trie() { tree.emplace_back(); }

    void insert(string& s, int id) 
    {
        int len = s.length(), add = 0;
        for (int i = 0; i < len; i++) 
        {
            int x = s[i] - 'a';
            if (!tree[add].ch[x]) 
            {
                tree.emplace_back();
                tree[add].ch[x] = tree.size() - 1;
            }
            add = tree[add].ch[x];
        }
        tree[add].flag = id;
    }

    vector<int> query(string& s) 
    {
        int len = s.length(), add = 0;
        vector<int> result(len + 1, -1);
        for (int i = 0; i < len; i++) 
        {
            result[i] = tree[add].flag;
            int x = s[i] - 'a';
            if (!tree[add].ch[x]) return result;
            add = tree[add].ch[x];
        }
        result[len] = tree[add].flag;
        return result;
    }
};

class Solution 
{
public:
    vector<pair<int, int>> manacher(string& s) 
    {
        int n = s.size();
        string tmp = "#";
        tmp += s[0];
        for (int i = 1; i < n; i++) 
        {
            tmp += '*';
            tmp += s[i];
        }
        tmp += '!';
        int m = n * 2;
        vector<int> len(m);
        vector<pair<int, int>> result(n);
        int p = 0, maxn = -1;
        for (int i = 1; i < m; i++) 
        {
            len[i] = maxn >= i ? min(len[2 * p - i], maxn - i) : 0;
            while (tmp[i - len[i] - 1] == tmp[i + len[i] + 1]) len[i]++;
            if (i + len[i] > maxn)  p = i, maxn = i + len[i];
            if (i - len[i] == 1) result[(i + len[i]) / 2].first = 1;
            if (i + len[i] == m - 1) result[(i - len[i]) / 2].second = 1;
        }
        return result;
    }

    vector<vector<int>> palindromePairs(vector<string>& words) 
    {
        Trie trie1, trie2;
        int n = words.size();
        for (int i = 0; i < n; i++) 
        {
            trie1.insert(words[i], i);
            string tmp = words[i];
            reverse(tmp.begin(), tmp.end());
            trie2.insert(tmp, i);
        }
        vector<vector<int>> result;
        for (int i = 0; i < n; i++) 
        {
            const vector<pair<int, int>>& rec = manacher(words[i]);
            const vector<int>& id1 = trie2.query(words[i]);
            reverse(words[i].begin(), words[i].end());
            const vector<int>& id2 = trie1.query(words[i]);
            int m = words[i].size(), all_id = id1[m];
            if (all_id != -1 and all_id != i) result.push_back({i, all_id});
            for (int j = 0; j < m; j++) 
            {
                if (rec[j].first) 
                {
                    int left_id = id2[m - j - 1];
                    if (left_id != -1 and left_id != i) result.push_back({left_id, i});
                }
                if (rec[j].second) 
                {
                    int right_id = id1[j];
                    if (right_id != -1 and right_id != i) result.push_back({i, right_id});
                }
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    private Node root;
    
    public List<List<Integer>> palindromePairs(String[] words) {
        this.root = new Node();
        int n = words.length;
        for (int i = 0; i < n; i++) {
            StringBuilder rev = new StringBuilder(words[i]).reverse();
            Node cur = root;
            if (isPalindrome(rev.substring(0))) cur.suffixs.add(i);
            for (int j = 0; j < rev.length(); j++) {
                char c = rev.charAt(j);
                if (cur.children[c - 'a'] == null) cur.children[c - 'a'] = new Node();
                cur = cur.children[c - 'a'];
                if (isPalindrome(rev.substring(j + 1))) cur.suffixs.add(i);
            }
            cur.words.add(i);
        }
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            String word = words[i];
            Node cur = root;
            int j = 0;
            for (; j < word.length(); j++) {
                if (isPalindrome(word.substring(j))) for (int k : cur.words) if (k != i) result.add(Arrays.asList(i, k));
                char c = word.charAt(j);
                if (cur.children[c - 'a'] == null) break;
                cur = cur.children[c - 'a'];
            }
            if (j == word.length()) for (int k : cur.suffixs) if (k != i) result.add(Arrays.asList(i,k));
        }
        return result;
    }
    
    private boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;
        while (i < j) if (s.charAt(i++) != s.charAt(j--)) return false;
        return true;
    }
}
class Node {
    public Node[] children;
    public List<Integer> words;
    public List<Integer> suffixs;
    public Node() {
        this.children = new Node[26];
        this.words = new ArrayList<>();
        this.suffixs = new ArrayList<>();
    }
}
```

__Python__:

```Python
class Solution:
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        d, result = {word[::-1]: i for i, word in enumerate(words)}, []
        for i, word in enumerate(words):
            for k in range(len(word) + 1):
                (sub := word[k:]) == sub[::-1] and (j := d.get(word[:k], -1)) >= 0 and i != j and result.append((i, j))
                k and (sub := word[:k]) == sub[::-1] and (j := d.get(word[k:], -1)) >= 0 and i != j and result.append((j, i))
        return result
```
