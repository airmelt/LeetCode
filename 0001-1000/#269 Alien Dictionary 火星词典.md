# 269 Alien Dictionary 火星词典

__Description:__

There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.

You are given a list of strings words from the alien language's dictionary, where the strings in words are sorted lexicographically by the rules of this new language.

Return a string of the unique letters in the new alien language sorted in lexicographically increasing order by the new language's rules. If there is no solution, return "". If there are multiple solutions, return any of them.

A string s is lexicographically smaller than a string t if at the first letter where they differ, the letter in s comes before the letter in t in the alien language. If the first min(s.length, t.length) letters are the same, then s is smaller if and only if s.length < t.length.

__Example:__

Example 1:

Input: words = ["wrt","wrf","er","ett","rftt"]
Output: "wertf"

Example 2:

Input: words = ["z","x"]
Output: "zx"

Example 3:

Input: words = ["z","x","z"]
Output: ""
Explanation: The order is invalid, so return "".

__Constraints:__

1 <= words.length <= 100
1 <= words[i].length <= 100
words[i] consists of only lowercase English letters.

__题目描述：__
现有一种使用英语字母的火星语言，这门语言的字母顺序与英语顺序不同。

给你一个字符串列表 words ，作为这门语言的词典，words 中的字符串已经 按这门新语言的字母顺序进行了排序 。

请你根据该词典还原出此语言中已知的字母顺序，并 按字母递增顺序 排列。若不存在合法字母顺序，返回 "" 。若存在多种可能的合法字母顺序，返回其中 任意一种 顺序即可。

字符串 s 字典顺序小于 字符串 t 有两种情况：

在第一个不同字母处，如果 s 中的字母在这门外星语言的字母顺序中位于 t 中字母之前，那么 s 的字典顺序小于 t 。
如果前面 min(s.length, t.length) 字母都相同，那么 s.length < t.length 时，s 的字典顺序也小于 t 。

__示例：__

示例 1：

输入：words = ["wrt","wrf","er","ett","rftt"]
输出："wertf"

示例 2：

输入：words = ["z","x"]
输出："zx"

示例 3：

输入：words = ["z","x","z"]
输出：""
解释：不存在合法字母顺序，因此返回 "" 。

__提示：__

1 <= words.length <= 100
1 <= words[i].length <= 100
words[i] 仅由小写英文字母组成

__思路：__

拓扑排序
题意实际上需要保证出现在前面的字符在拓扑排序中的位置更靠前
先用 words 建立图, 记录下各个结点的后继结点
注意这里如果出现 "abc", "ab" 前后出现直接返回 ""
然后统计结点的入度
最后使用 BFS 拓扑排序得到字母的顺序
时间复杂度为 O(mn), 空间复杂度为 O(n), n 为 words 数组长度, m 为 words 中字符串的平均长度

__代码__:

__C++__:

```C++
class Solution 
{
public:
    string alienOrder(vector<string>& words) 
    {
        map<char, unordered_set<char>> m;
        int n = words.size(), count = 0;
        vector<int> degree(26, -1);
        string result = "";
        for (int i = 0; i < n - 1; i++) 
        {
            for (int j = 0; j < min(words[i].size(), words[i + 1].size()); j++) 
            {
                if (words[i][j] != words[i + 1][j]) {
                    m[words[i][j]].insert(words[i + 1][j]);
                    break;
                } else if (j == min(words[i].size(), words[i + 1].size()) - 1 and words[i].size() > words[i + 1].size()) return result;
            }
        }
        for (const auto& word : words) for (const auto& c : word) degree[c - 'a'] = 0;
        for (const auto& [key, value] : m) for (const auto& v : m[key]) ++degree[v - 'a'];
        queue<char> q;
        for (int i = 0; i < 26; i++) 
        {
            if (!degree[i]) q.push((char)(i + 'a'));
            if (degree[i] != -1) ++count;
        }
        while (!q.empty()) 
        {
            auto u = q.front();
            q.pop();
            result += u;
            if (m.count(u)) 
            {
                for (const auto& v : m[u]) 
                {
                    --degree[v - 'a'];
                    if (!degree[v - 'a']) q.push(v);
                }
            }
        }
        return result.size() == count ? result : "";
    }
};
```

__Java__:

```Java
class Solution {
    public String alienOrder(String[] words) {
        Map<Character, Set<Character>> map = new HashMap<>();
        int n = words.length, degree[] = new int[26], count = 0;
        Arrays.fill(degree, -1);
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < Math.min(words[i].length(), words[i + 1].length()); j++) {
                if (words[i].charAt(j) != words[i + 1].charAt(j)) {
                    Set<Character> set = map.getOrDefault(words[i].charAt(j), new HashSet<>());
                    set.add(words[i + 1].charAt(j));
                    map.putIfAbsent(words[i].charAt(j), set);
                    break;
                } else if (j == Math.min(words[i].length(), words[i + 1].length()) - 1 && words[i].length() > words[i + 1].length()) return "";
            }
        }
        for (String word : words) for (char c : word.toCharArray()) degree[c - 'a'] = 0;
        for (Character key : map.keySet()) for (Character v : map.get(key)) ++degree[v - 'a'];
        StringBuilder sb = new StringBuilder();
        Queue<Character> queue = new LinkedList<>();
        for (int i = 0; i < 26; i++) {
            if (degree[i] == 0) queue.add((char)(i + 'a'));
            if (degree[i] != -1) ++count;
        }
        while (!queue.isEmpty()) {
            Character u = queue.poll();
            sb.append(u);
            if (map.containsKey(u)) {
                for (Character v : map.get(u)) {
                    --degree[v - 'a'];
                    if (degree[v - 'a'] == 0) queue.add(v);
                }
            }
        }
        return sb.length() == count ? sb.toString() : "";
    }
}
```

__Python__:

```Python
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        graph, n, result, degree = defaultdict(set), len(words), '', [-1] * 26
        for i in range(n - 1):
            for j in range(min(len(words[i]), len(words[i + 1]))):
                if words[i][j] != words[i + 1][j]:
                    graph[words[i][j]].add(words[i + 1][j])
                    break
                elif j == min(len(words[i]), len(words[i + 1])) - 1 and len(words[i]) > len(words[i + 1]):
                    return result
        for word in words:
            for c in word:
                degree[ord(c) - ord('a')] = 0
        for key, value in graph.items():
            for v in graph[key]:
                degree[ord(v) - ord('a')] += 1
        q, count = deque([chr(i + ord('a')) for i in range(26) if not degree[i]]), sum(degree[i] != -1 for i in range(26))
        while q:
            u = q.popleft()
            result += u
            for v in graph[u]:
                degree[ord(v) - ord('a')] -= 1
                if not degree[ord(v) - ord('a')]:
                    q.append(v)
        return result if len(result) == count else ""
```
