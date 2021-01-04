# 433 Minimum Genetic Mutation 最小基因变化

__Description__:
A gene string can be represented by an 8-character long string, with choices from "A", "C", "G", "T".

Suppose we need to investigate about a mutation (mutation from "start" to "end"), where ONE mutation is defined as ONE single character changed in the gene string.

For example, "AACCGGTT" -> "AACCGGTA" is 1 mutation.

Also, there is a given gene "bank", which records all the valid gene mutations. A gene must be in the bank to make it a valid gene string.

Now, given 3 things - start, end, bank, your task is to determine what is the minimum number of mutations needed to mutate from "start" to "end". If there is no such a mutation, return -1.

__Note:__

Starting point is assumed to be valid, so it might not be included in the bank.
If multiple mutations are needed, all mutations during in the sequence must be valid.
You may assume start and end string is not the same.

__Example:__

Example 1:

start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

return: 1

Example 2:

start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

return: 2

Example 3:

start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

return: 3

__题目描述__:
一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 "A", "C", "G", "T"中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由"AACCGGTT" 变化至 "AACCGGTA" 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

__注意：__

起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
如果一个起始基因序列需要多次变化，那么它每一次变化之后的基因序列都必须是合法的。
假定起始基因序列与目标基因序列是不一样的。

__示例 :__
示例 1：

start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1

示例 2：

start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

返回值: 2

示例 3：

start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3

__思路__:

参考[LeetCode #127 Word Ladder 单词接龙](https://www.jianshu.com/p/0cda556c2b2d)
时间复杂度O(mn), 空间复杂度O(mn), m, n分别为 bank的长度和 bank中的字符串的长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minMutation(string start, string end, vector<string>& bank) 
    {
        unordered_set<string> s(bank.begin(), bank.end());
        if (s.find(end) == s.end()) return -1;
        unordered_set<string> begin_set{start}, end_set{end};
        int result = 0, chars[]{'A', 'C', 'T', 'G'};
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
                    for (auto c : chars)
                    {
                        need[i] = (char)c;
                        if (s.find(need) == s.end()) continue;
                        if (end_set.find(need) != end_set.end()) return result;
                        temp_set.insert(need);
                    }
                }
            }
            if (temp_set.size() < end_set.size()) begin_set = temp_set;
            else 
            {
                begin_set = end_set;
                end_set = temp_set;
            }
        }
        return -1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minMutation(String start, String end, String[] bank) {
        Set<String> set = new HashSet<>();
        for (String word : bank) set.add(word);
        Set<String> beginSet = new HashSet<>(), endSet = new HashSet<>();
        beginSet.add(start);
        endSet.add(end);
        if (!set.contains(end)) return -1;
        int result = 0, chars[] = {'A', 'C', 'T', 'G'};
        while (!beginSet.isEmpty()) {
            Set<String> tempSet = new HashSet<>();
            ++result;
            for (String word : beginSet) set.remove(word);
            for (String word : beginSet) {
                for (int i = 0; i < word.length(); i++) {
                    StringBuilder sb = new StringBuilder(word);
                    for (int c : chars) {
                        sb.setCharAt(i, (char)c);
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
        return -1;
    }
}
```

__Python__:

```Python
class Solution:
    def minMutation(self, start: str, end: str, bank: List[str]) -> int:
        result, s, begin_set, end_set, chars = 0, set(bank), set([start]), set([end]), ['A', 'C', 'T', 'G']
        if end not in s:
            return -1
        while begin_set:
            temp_set = set()
            result += 1
            for word in begin_set:
                s.discard(word)
            for word in begin_set:
                for i in range(len(word)):
                    need = word
                    for c in chars:
                        need = need[:i] + c + need[i + 1:]
                        if need not in s:
                            continue
                        if need in end_set:
                            return result
                        temp_set.add(need)
            begin_set, end_set = (temp_set, end_set) if len(temp_set) < len(end_set) else (end_set, temp_set)
        return -1
```
