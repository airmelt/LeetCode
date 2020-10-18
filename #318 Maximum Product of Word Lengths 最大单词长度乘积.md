__Description__:
Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

__Example:__
Example 1:

Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16 
Explanation: The two words can be "abcw", "xtfn".

Example 2:

Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4 
Explanation: The two words can be "ab", "cd".

Example 3:

Input: ["a","aa","aaa","aaaa"]
Output: 0 
Explanation: No such pair of words.
 
__Constraints:__

0 <= words.length <= 10^3
0 <= words[i].length <= 10^3
words[i] consists only of lowercase English letters.

__题目描述__:
给定一个字符串数组 words，找到 length(word[i]) * length(word[j]) 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

__示例 :__
示例 1:

输入: ["abcw","baz","foo","bar","xtfn","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "xtfn"。

示例 2:

输入: ["a","ab","abc","d","cd","bcd","abcd"]
输出: 4 
解释: 这两个单词为 "ab", "cd"。

示例 3:

输入: ["a","aa","aaa","aaaa"]
输出: 0 
解释: 不存在这样的两个单词。

__思路__:
由于只有小写字母, 可以用 32位的整数存放字母是否出现
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int maxProduct(vector<string>& words) 
    {
        int result = 0;
        if (words.empty()) return result;
        vector<int> hash(words.size(), 0);
        for (int i = 0; i < words.size(); i++) for (auto &c : words[i]) hash[i] |= 1 << (c - 'a');
        for (int i = 0; i < words.size() - 1; i++) for (int j = i + 1; j < words.size(); j++) if ((hash[i] & hash[j]) == 0) result = max((int)(words[i].size() * words[j].size()), result);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int maxProduct(String[] words) {
        int hash[] = new int[words.length], result = 0;
        for (int i = 0; i < words.length; i++) for (char c : words[i].toCharArray()) hash[i] |= 1 << (c - 'a');
        for (int i = 0; i < words.length - 1; i++) for (int j = i + 1; j < words.length; j++) if ((hash[i] & hash[j]) == 0) result = Math.max(words[i].length() * words[j].length(), result);
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def maxProduct(self, words: List[str]) -> int:
        h, result = [0] * len(words), 0
        for i, word in enumerate(words):
            for c in word:
                h[i] |= 1 << (ord(c) - ord('a'))
        for i in range(len(words) - 1):
            for j in range(i + 1, len(words)):
                if not (h[i] & h[j]):
                    result = max(result, len(words[i]) * len(words[j]))
        return result
```