# 792 Number of Matching Subsequences 匹配子序列的单词数

__Description__:
Given a string s and an array of strings words, return the number of words[i] that is a subsequence of s.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".

__Example:__

Example 1:

Input: s = "abcde", words = ["a","bb","acd","ace"]
Output: 3
Explanation: There are three strings in words that are a subsequence of s: "a", "acd", "ace".

Example 2:

Input: s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
Output: 2

__Constraints:__

1 <= s.length <= 5 * 10^4
1 <= words.length <= 5000
1 <= words[i].length <= 50
s and words[i] consist of only lowercase English letters.

__题目描述__:
给定字符串 S 和单词字典 words, 求 words[i] 中是 S 的子序列的单词个数。

__示例 :__

输入:
S = "abcde"
words = ["a", "bb", "acd", "ace"]
输出: 3
解释: 有三个是 S 的子序列的单词: "a", "acd", "ace"。

__注意:__

所有在words和 S 里的单词都只由小写字母组成。
S 的长度在 [1, 50000]。
words 的长度在 [1, 5000]。
words[i]的长度在[1, 50]。

__思路__:

1. 暴力法
按序查找 words 中的每一个 word 的每一个字符, 查找失败立即返回
时间复杂度为 O(nml), 空间复杂度为 O(1), n 表示 s 的长度, m 表示 words 的长度, l 表示 words 中每个 word 的平均长度
2. 前缀树匹配
设置一个长度为 26 的桶存放当前遍历的所有 word 的对应 words 的下标位置, 及当前字符对应 word 的下标位置
每次移动 s 对应的下标, 并将该字符对应的桶中的元素进行移动, 每次遍历到 word 的结尾就 ++result
时间复杂度为 O(n + l), 空间复杂度为 O(m), n 表示 s 的长度, m 表示 words 的长度, l 表示 words 中每个 word 的平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numMatchingSubseq(string s, vector<string>& words) 
    {
        vector<queue<pair<int, int>>> buckets(26);
        int result = 0;
        for (int i = 0; i < words.size(); i++) buckets[words[i].front() - 'a'].push({ i, 0 });
        for (const auto& c: s)
        {
            auto& q = buckets[c - 'a'];
            for (int i = q.size(); i > 0; i--)
            {
                auto [word, pos] = q.front();
                q.pop();
                ++pos;
                if (pos == words[word].size()) ++result;
                else buckets[words[word][pos] - 'a'].push({ word, pos });
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numMatchingSubseq(String s, String[] words) {
        int result = 0, j;
        for (int i = 0, n = words.length; i < n; i ++) {
            int pos = -1;
            for (j = 0; j < words[i].length(); j++) {
                pos = s.indexOf(words[i].charAt(j), pos + 1);
                if (pos == -1) break;
            }
            if (j == words[i].length()) ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        bucket, m, result = [[] for _ in range(26)], len(words), 0
        for i in range(m):
            bucket[ord(words[i][0]) - ord('a')].append((i, 0))
        for c in s:
            bucket[ord(c) - ord('a')], q = [], bucket[ord(c) - ord('a')]
            for word, pos in q:
                pos += 1
                if pos == len(words[word]):
                    result += 1
                else:
                    bucket[ord(words[word][pos]) - ord('a')].append((word, pos))
        return result
```
