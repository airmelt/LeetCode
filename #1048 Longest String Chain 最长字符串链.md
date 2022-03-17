# 1048 Longest String Chain 最长字符串链

__Description__:
You are given an array of words where each word consists of lowercase English letters.

wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".
A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1.

Return the length of the longest possible word chain with words chosen from the given list of words.

__Example:__

Example 1:

Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chains is ["a","ba","bda","bdca"].

Example 2:

Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5
Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

Example 3:

Input: words = ["abcd","dbqca"]
Output: 1
Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.

__Constraints:__

1 <= words.length <= 1000
1 <= words[i].length <= 16
words[i] only consists of lowercase English letters.

__题目描述__:
给出一个单词数组 words ，其中每个单词都由小写英文字母组成。

如果我们可以 不改变其他字符的顺序 ，在 wordA 的任何地方添加 恰好一个 字母使其变成 wordB ，那么我们认为 wordA 是 wordB 的 前身 。

例如，"abc" 是 "abac" 的 前身 ，而 "cba" 不是 "bcad" 的 前身
词链是单词 [word_1, word_2, ..., word_k] 组成的序列，k >= 1，其中 word1 是 word2 的前身，word2 是 word3 的前身，依此类推。一个单词通常是 k == 1 的 单词链 。

从给定单词列表 words 中选择单词组成词链，返回 词链的 最长可能长度 。

__示例 :__

示例 1：

输入：words = ["a","b","ba","bca","bda","bdca"]
输出：4
解释：最长单词链之一为 ["a","ba","bda","bdca"]

示例 2:

输入：words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
输出：5
解释：所有的单词都可以放入单词链 ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

示例 3:

输入：words = ["abcd","dbqca"]
输出：1
解释：字链["abcd"]是最长的字链之一。
["abcd"，"dbqca"]不是一个有效的单词链，因为字母的顺序被改变了。

__提示:__

1 <= words.length <= 1000
1 <= words[i].length <= 16
words[i] 仅由小写英文字母组成。

__思路__:

动态规划
从长的单词中删除一个字母来得到更短的单词
设 dp[word] 表示以 word 开头能得到的依次递减的最长字符串链
dp[word] = max(dp[word[:i] + word[i + 1:]] + 1)
初始化 dp[word] = 1, 因为 word 本身也是一个长度为 1 的字符串链
最后取 max(dp.values())
时间复杂度O(mn), 空间复杂度O(mn), n 为 words 数组的长度, m 为每个单词的平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int longestStrChain(vector<string>& words) 
    {
        unordered_map<string, int> dp;
        sort(words.begin(), words.end(), [](string& a, string& b) { return a.size() < b.size(); });
        int result = 0;
        for (const auto& word : words) 
        {
            int best = 0, n = word.length();
            for (int i = 0; i < n; ++i) best = max(best, dp[word.substr(0, i) + word.substr(i + 1)] + 1);
            dp[word] = best;
            result = max(result, best);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestStrChain(String[] words) {
        Map<String, Integer> dp = new HashMap<>();
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        int result = 0;
        for (String word : words) {
            int best = 0, n = word.length();
            for (int i = 0; i < n; ++i) best = Math.max(best, dp.getOrDefault(word.substring(0, i) + word.substring(i + 1), 0) + 1);
            dp.put(word, best);
            result = Math.max(result, best);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        dp = defaultdict(int)
        for w in sorted(words, key=len):
            dp[w] = max(dp[w[:i] + w[i + 1:]] + 1 for i in range(len(w)))
        return max(dp.values())
```
