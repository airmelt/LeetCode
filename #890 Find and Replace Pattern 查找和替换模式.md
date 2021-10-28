# 890 Find and Replace Pattern 查找和替换模式

__Description__:
Given a list of strings words and a string pattern, return a list of words[i] that match pattern. You may return the answer in any order.

A word matches the pattern if there exists a permutation of letters p so that after replacing every letter x in the pattern with p(x), we get the desired word.

Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.

__Example:__

Example 1:

Input: words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
Output: ["mee","aqq"]
Explanation: "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}.
"ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation, since a and b map to the same letter.

Example 2:

Input: words = ["a","b","c"], pattern = "a"
Output: ["a","b","c"]

__Constraints:__

1 <= pattern.length <= 20
1 <= words.length <= 50
words[i].length == pattern.length
pattern and words[i] are lowercase English letters.

__题目描述__:
你有一个单词列表 words 和一个模式  pattern，你想知道 words 中的哪些单词与模式匹配。

如果存在字母的排列 p ，使得将模式中的每个字母 x 替换为 p(x) 之后，我们就得到了所需的单词，那么单词与模式是匹配的。

（回想一下，字母的排列是从字母到字母的双射：每个字母映射到另一个字母，没有两个字母映射到同一个字母。）

返回 words 中与给定模式匹配的单词列表。

你可以按任何顺序返回答案。

__示例 :__

输入：words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
输出：["mee","aqq"]
解释：
"mee" 与模式匹配，因为存在排列 {a -> m, b -> e, ...}。
"ccc" 与模式不匹配，因为 {a -> c, b -> c, ...} 不是排列。
因为 a 和 b 映射到同一个字母。

__提示:__

1 <= words.length <= 50
1 <= pattern.length = words[i].length <= 20

__思路__:

模拟
对 words 数组中的每一个字符串进行检查
可以用查找字符或者哈希表记录字符位置判断
时间复杂度为 O(nk), 空间复杂度为 O(nk), n 为 words 长度, k 为 words 中的单词平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) 
    {
        vector<string> result;
        for (const auto& word : words) if (check(word, pattern)) result.emplace_back(word);
        return result;
    }
 private:
    bool check(const string& word, const string& pattern) 
    {
        if (word.size() != pattern.size()) return false;
        for (int i = 0, n = word.size(); i < n; i++) if (word.find(word[i]) != pattern.find(pattern[i])) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> findAndReplacePattern(String[] words, String pattern) {
        List<String> result = new ArrayList<>();
        for (String word : words) if (check(word, pattern)) result.add(word);
        return result;
    }
    
    private boolean check(String word, String pattern) {
        if (word.length() != pattern.length()) return false;
        for (int i = 0, n = word.length(); i < n; i++) if (word.indexOf(word.charAt(i)) != pattern.indexOf(pattern.charAt(i))) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def findAndReplacePattern(self, words: List[str], pattern: str) -> List[str]:
        return [w for w in words if len(w) == len(pattern) and len(set(w)) == len(set(pattern)) == len(set(zip(w, pattern)))]
```
