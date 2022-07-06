# 1178 Number of Valid Words for Each Puzzle 猜字谜

__Description__:
With respect to a given puzzle string, a word is valid if both the following conditions are satisfied:
word contains the first letter of puzzle.
For each letter in word, that letter is in puzzle.
For example, if the puzzle is "abcdefg", then valid words are "faced", "cabbage", and "baggage", while
invalid words are "beefed" (does not include 'a') and "based" (includes 's' which is not in the puzzle).
Return an array answer, where answer[i] is the number of words in the given word list words that is valid with respect to the puzzle puzzles[i].

__Example:__

Example 1:

Input: words = ["aaaa","asas","able","ability","actt","actor","access"], puzzles = ["aboveyz","abrodyz","abslute","absoryz","actresz","gaswxyz"]
Output: [1,1,3,2,4,0]
Explanation:
1 valid word for "aboveyz" : "aaaa"
1 valid word for "abrodyz" : "aaaa"
3 valid words for "abslute" : "aaaa", "asas", "able"
2 valid words for "absoryz" : "aaaa", "asas"
4 valid words for "actresz" : "aaaa", "asas", "actt", "access"
There are no valid words for "gaswxyz" cause none of the words in the list contains letter 'g'.

Example 2:

Input: words = ["apple","pleas","please"], puzzles = ["aelwxyz","aelpxyz","aelpsxy","saelpxy","xaelpsy"]
Output: [0,1,3,2,0]

__Constraints:__

1 <= words.length <= 10^5
4 <= words[i].length <= 50
1 <= puzzles.length <= 10^4
puzzles[i].length == 7
words[i] and puzzles[i] consist of lowercase English letters.
Each puzzles[i] does not contain repeated characters.

__题目描述__:
外国友人仿照中国字谜设计了一个英文版猜字谜小游戏，请你来猜猜看吧。

字谜的迷面 puzzle 按字符串形式给出，如果一个单词 word 符合下面两个条件，那么它就可以算作谜底：

单词 word 中包含谜面 puzzle 的第一个字母。
单词 word 中的每一个字母都可以在谜面 puzzle 中找到。
例如，如果字谜的谜面是 "abcdefg"，那么可以作为谜底的单词有 "faced", "cabbage", 和 "baggage"；而 "beefed"（不含字母 "a"）以及 "based"（其中的 "s" 没有出现在谜面中）都不能作为谜底。
返回一个答案数组 answer，数组中的每个元素 answer[i] 是在给出的单词列表 words 中可以作为字谜迷面 puzzles[i] 所对应的谜底的单词数目。

__示例 :__

输入：
words = ["aaaa","asas","able","ability","actt","actor","access"],
puzzles = ["aboveyz","abrodyz","abslute","absoryz","actresz","gaswxyz"]
输出：[1,1,3,2,4,0]
解释：
1 个单词可以作为 "aboveyz" 的谜底 : "aaaa"
1 个单词可以作为 "abrodyz" 的谜底 : "aaaa"
3 个单词可以作为 "abslute" 的谜底 : "aaaa", "asas", "able"
2 个单词可以作为 "absoryz" 的谜底 : "aaaa", "asas"
4 个单词可以作为 "actresz" 的谜底 : "aaaa", "asas", "actt", "access"
没有单词可以作为 "gaswxyz" 的谜底，因为列表中的单词都不含字母 'g'。

__提示:__

1 <= words.length <= 10^5
4 <= words[i].length <= 50
1 <= puzzles.length <= 10^4
puzzles[i].length == 7
words[i][j], puzzles[i][j] 都是小写英文字母。
每个 puzzles[i] 所包含的字符都不重复。

__思路__:

状态压缩 ➕ 子集DFS
由于只要出现过所有字符, 所以可以将 words 中的每个 word 按照字符用一个整数表示
并将相同的合并, 存入到哈希表中
对于 puzzles 中的每一个 puzzle, 第一个字符固定为 puzzle 首字符, 然后将剩下的查找所有子集并在哈希表中查找该模式出现的次数即为答案
求子集可以用 DFS 的方式也可以用位运算的形式
时间复杂度为 O(ms + n * 2 ^ k), 空间复杂度为 O(m), 其中 w 为 words 单词平均长度, m 为 words 的长度, n 为 puzzles 的长度, k 为 puzzles 中每个字符串的长度, 题中为 7

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findNumOfValidWords(vector<string>& words, vector<string>& puzzles) 
    {
        unordered_map<int, int> m;
        for (const auto& word : words) ++m[cal(word)];
        vector<int> result;
        for (const auto& p : puzzles) 
        {
            int mask = cal(p);
            int subset = mask, first = 1 << (p.front() - 'a');
            result.emplace_back(0);
            while (subset)
            {
                if (subset & first) result.back() += m[subset];
                subset = (subset - 1) & mask;
            }
        }
        return result;
    }
private:
    int cal(const string& s)
    {
        int result = 0;
        for (const auto& c : s) result |= 1 << (c - 'a');
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> findNumOfValidWords(String[] words, String[] puzzles) {
        Map<Integer, Integer> map = new HashMap<>();
        for (String word : words) {
            int key = 0;
            for (char ch : word.toCharArray()) key |= 1 << ch - 'a';
            map.put(key, map.getOrDefault(key, 0) + 1);
        }
        List<Integer> result = new ArrayList<>(puzzles.length);
        for (String p : puzzles) {
            char[] puzzle = p.toCharArray();
            result.add(dfs(puzzle, 1, map, 1 << puzzle[0] - 'a'));
        }
        return result;
    }
    
    private int dfs(char[] puzzle, int idx, Map<Integer, Integer> map, int key) {
        return idx == puzzle.length ? map.getOrDefault(key, 0) : dfs(puzzle, idx + 1, map, key) + dfs(puzzle, idx + 1, map, key | 1 << puzzle[idx] - 'a');
    }
}
```

__Python__:

```Python
class Solution:
    def findNumOfValidWords(self, words: List[str], puzzles: List[str]) -> List[int]:
        counter, result = Counter(filter(lambda x: len(x) <= 7, map(frozenset, words))), [0] * len(puzzles)
        for index, puzzle in enumerate(puzzles):
            for comb in list(map(lambda i: combinations(puzzle[1:], i), range(7))):
                result[index] += sum(counter[frozenset((puzzle[0],) + ele)] for ele in comb)
        return result
```
