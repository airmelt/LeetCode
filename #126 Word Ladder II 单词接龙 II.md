__Description__:
Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

Only one letter can be changed at a time
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.

__Note:__

Return an empty list if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume beginWord and endWord are non-empty and are not the same.

__Example:__
Example 1:

Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]

Example 2:

Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.

__题目描述__:
给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：

每次转换只能改变一个字母。
转换后得到的单词必须是字典中的单词。

__说明：__

如果不存在这样的转换序列，返回一个空列表。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

__示例 :__
示例 1:

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]

示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: []

解释: endWord "cog" 不在字典中，所以不存在符合要求的转换序列。

__思路__:
要求输出最小的路径, 一般考虑使用 BFS
考虑到要搜索 word, 可以将 word对应成 id放入哈希表中
为了方便转化可以对应 id再建立一个哈希表, 这里可以简单的用下标替代
将每一个 word看作一个节点, 如果两个 word之间只相差一个字母, 就在这两个节点之间加上一条双向边, 表示可以接龙
考虑代价我们可以用一个数组记录初始单词到各单词之间的距离, 即转化次数, 初始化为正无穷
对于队列遍历的节点, 都对应一个数组, 记录所有通过的路径
如果最后一个节点就是结束单词, 直接输出所有路径对应的单词
否则, 将这个节点对应的下一个节点加入数组, 并更新 cost
时间复杂度O(n), 空间复杂度O(1), 不考虑栈的空间

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) 
    {
        vector<vector<string>> result;
        // 将 word与 id对应
        unordered_map<string, int> word_id;
        // 将 id与 word对应
        vector<string> id_word;
        // 构造图
        vector<vector<int>> edges;
        
        // 构造哈希表存放 word
        int id = 0;
        for (int i = 0; i < wordList.size(); i++)
        {
            if (!word_id.count(wordList[i]))
            {
                word_id[wordList[i]] = id++;
                id_word.push_back(wordList[i]);
            }
        }
        
        // 如果不存在 endWord直接返回空列表
        if (!word_id.count(endWord)) return result;
        
        // 将 beginWord加入哈希表
        if (!word_id.count(beginWord))
        {
            word_id[beginWord] = id++;
            id_word.push_back(beginWord);
        }
        
        // 构造图
        edges.resize(id);
        for (int i = 0; i < id; i++) for (int j = i + 1; j < id; j++)
        {
            // 当两个单词有且只有一个字母不同的时候在图中加入一个双向边
            if (is_one_changed(id_word[i], id_word[j]))
            {
                edges[i].push_back(j);
                edges[j].push_back(i);
            }
        }
        
        // endWord对应的 id
        int end_id = word_id[endWord];
        
        // 搜索队列, 先加入 beginWord
        queue<vector<int>> q;
        q.push(vector<int>{word_id[beginWord]});
            
        // cost数组记录每个单词到 beginWord的代价, 初始化为 INT_MAX表示未可达, beginWord的 cost初始化为 0
        vector<int> cost(id, INT_MAX);
        cost[word_id[beginWord]] = 0;
        
        // bfs
        while (q.size())
        {
            vector<int> now = q.front();
            q.pop();
            
            // 当前路径对应的单词
            int last_word_id = now.back();
            // 如果当前单词就是 endWord
            if (last_word_id == end_id)
            {
                vector<string> temp;
                for (auto index : now) temp.push_back(id_word[index]);
                result.push_back(temp);
            }
            else
            {
                for (int i = 0; i < edges[last_word_id].size(); i++)
                {
                    // 路径最后一个单词的下一个单词
                    int next = edges[last_word_id][i];
                    // 更新 cost
                    if (cost[last_word_id] + 1 <= cost[next])
                    {
                        cost[next] = cost[last_word_id] + 1;
                        vector<int> temp(now.begin(), now.end());
                        temp.push_back(next);
                        q.push(temp);
                    }
                }
            }
        }
        return result;
    } 
private:
    bool is_one_changed(string &word1, string &word2)
    {
        int count = 0;
        for (int i = 0; i < word1.size() && count < 2; i++) if (word1[i] != word2[i]) ++count;
        return count == 1;
    }
};
```

__Java__:
```Java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> result = new ArrayList<>();;
        Map<String, Integer> wordId = new HashMap<>();
        List<String> idWord = new ArrayList<>();
        int id = 0;
        for (int i = 0; i < wordList.size(); i++) {
            if (!wordId.containsKey(wordList.get(i))) {
                wordId.put(wordList.get(i), id++);
                idWord.add(wordList.get(i));
            }
        }
        if (!wordId.containsKey(endWord)) return result;
        if (!wordId.containsKey(beginWord)) {
            wordId.put(beginWord, id++);
            idWord.add(beginWord);
        }
        List<List<Integer>> edges = new ArrayList<>(id);
        for (int i = 0; i < id; i++) edges.add(new ArrayList<Integer>());
        for (int i = 0; i < idWord.size(); i++) for (int j = i + 1; j < idWord.size(); j++) {
            if (isOneChanged(idWord.get(i), idWord.get(j))) {
                edges.get(i).add(j);
                edges.get(j).add(i);
            }
        }
        int cost[] = new int[id], endId = wordId.get(endWord);
        for (int i = 0; i < id; i++) cost[i] = Integer.MAX_VALUE;
        Queue<ArrayList<Integer>> q = new LinkedList<>();
        ArrayList<Integer> tempBegin = new ArrayList<>();
        tempBegin.add(wordId.get(beginWord));
        q.add(tempBegin);
        cost[wordId.get(beginWord)] = 0;
        while (!q.isEmpty()) {
            ArrayList<Integer> now = q.poll();
            int last = now.get(now.size() - 1);
            if (last == endId) {
                ArrayList<String> temp = new ArrayList<>();
                for (int index : now) temp.add(idWord.get(index));
                result.add(temp);
            } else {
                for (int i = 0; i < edges.get(last).size(); i++) {
                    int to = edges.get(last).get(i);
                    if (cost[last] + 1 <= cost[to]) {
                        cost[to] = cost[last] + 1;
                        ArrayList<Integer> temp = new ArrayList<>(now); 
                        temp.add(to);
                        q.add(temp);
                    }
                }
            }
        }
        return result;
    }
    
    private boolean isOneChanged(String word1, String word2) {
        int count = 0;
        for (int i = 0; i < word1.length() && count < 2; i++) if (word1.charAt(i) != word2.charAt(i)) ++count;
        return count == 1;
    }    
}
```

__Python__:
```Python
class Solution:
    def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
        d = collections.defaultdict(list)
        for word in wordList + [beginWord]:
            w = [*word]
            for i, c in enumerate(word):
                w[i] = '.'
                p = ''.join(w)
                d[p].append(word)
                d[word].append(p)
                w[i] = c
        if endWord in d:
            q, v = {beginWord: [[beginWord]]}, {beginWord}
            while q:
                if endWord in q:
                    return [*q[endWord]]
                t = collections.defaultdict(set)
                for i in q:
                    for j in d[i]:
                        for w in d[j]:
                            if w not in v:
                                t[w].update((*p, w) for p in q[i])
                q = t
                v.update(q.keys())
        return []
```Í