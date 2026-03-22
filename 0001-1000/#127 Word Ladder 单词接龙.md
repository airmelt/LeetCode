# 127 Word Ladder 单词接龙

__Description__:
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time.
Each transformed word must exist in the word list.

__Note:__

Return 0 if there is no such transformation sequence.
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

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.

Example 2:

Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.

__题目描述__:
给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。

__说明：__

如果不存在这样的转换序列，返回 0。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

__示例 :__

示例 1:

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。

示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。

__思路__:

1. 一开始用的传统 BFS, 会超时
2. 考虑到地点和终点已知, 可以用双端 BFS
3. 从两端分别搜索, 每次用较小的 set进行搜索
时间复杂度O(mn), 空间复杂度O(mn), m, n分别为 wordList的长度和 wordList中的字符串的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) 
    {
        unordered_set<string> s(wordList.begin(), wordList.end());
        if (s.find(endWord) == s.end()) return 0;
        unordered_set<string> begin_set{beginWord};
        unordered_set<string> end_set{endWord};
        int result = 1;
        while (!begin_set.empty())
        {
            unordered_set<string> temp_set;
            ++result;
            for (auto &word : begin_set) s.erase(word);
            for (auto &word : begin_set) 
            {
                for (int i = 0; i < word.size(); ++i)
                {
                    string need = word;
                    for (char c = 'a'; c <= 'z'; ++c)
                    {
                        need[i] = c;
                        if (s.find(need) == s.end()) continue;
                        if (end_set.find(need) != end_set.end()) return result;
                        temp_set.insert(need);
                    }
                }
            }
            begin_set = temp_set.size() < end_set.size() ? temp_set : end_set;
            end_set = temp_set.size() < end_set.size() ? end_set : temp_set;
        }
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> set = new HashSet<>();
        for (String word : wordList) set.add(word);
        Set<String> beginSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        beginSet.add(beginWord);
        endSet.add(endWord);
        if (!set.contains(endWord)) return 0;
        int result = 1;
        while (!beginSet.isEmpty()) {
            Set<String> tempSet = new HashSet<>();
            ++result;
            for (String word : beginSet) set.remove(word);
            for (String word : beginSet) {
                for (int i = 0; i < word.length(); i++) {
                    StringBuilder sb = new StringBuilder(word);
                    for (char c = 'a'; c <= 'z'; c++) {
                        sb.setCharAt(i, c);
                        String need = sb.toString();
                        if (!set.contains(need)) continue;
                        if (endSet.contains(need)) return result;
                        tempSet.add(need);
                    }
                }
            }
            beginSet = tempSet.size() < endSet.size() ? tempSet : endSet;
            endSet = tempSet.size() < endSet.size() ? endSet : tempSet;
        }
        return 0;
    }
}
```

__Python__:

```Python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        result, s, begin_set, end_set = 1, set(wordList), set([beginWord]), set([endWord])
        if endWord not in s:
            return 0
        while begin_set:
            temp_set = set()
            result += 1
            for word in begin_set:
                s.discard(word)
            for word in begin_set:
                for i in range(len(word)):
                    need = word
                    for c in range(26):
                        need = need[:i] + chr(ord('a') + c) + need[i + 1:]
                        if need not in s:
                            continue
                        if need in end_set:
                            return result
                        temp_set.add(need)
            begin_set, end_set = (temp_set, end_set) if len(temp_set) < len(end_set) else (end_set, temp_set)
        return 0
```
