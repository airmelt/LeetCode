# 30 Substring with Concatenation of All Words 串联所有单词的子串

__Description__:
You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

__Example:__

Example 1:

Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.

Example 2:

Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []

__题目描述__:
给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。

注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。

__示例 :__

示例 1：

输入：
  s = "barfoothefoobarman",
  words = ["foo","bar"]
输出：[0,9]
解释：
从索引 0 和 9 开始的子串分别是 "barfoo" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。

示例 2：

输入：
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
输出：[]

__思路__:

1. 对 words进行排序, 遍历字符串 s, 找出 words中的长度的字符串并排序再与排序后的 words的拼接结果比较, 相同就加入结果中
时间复杂度O(nmlgm), 空间复杂度O(m), 其中 n表示 s的长度, m表示 words拼接之后的长度
2. 对于 方法1, 可以用哈希表存放 words并比较, 可以减少排序带来的时间
时间复杂度O(nm), 空间复杂度O(m)
3. 滑动窗口
时间复杂度O(n), 空间复杂度O(m)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> findSubstring(string s, vector<string>& words) 
    {
        vector<int> result;
        if (s.size() == 0 or words.size() == 0) return result;
        unordered_map<string, int> m;
        int word_size = words[0].size(), words_size = words.size(), s_size = s.size();
        for (auto word : words) ++m[word];
        for (int i = 0; i < word_size; i++) 
        {
            int left = i, right = i, count = 0;
            unordered_map<string, int> temp;
            while (right + word_size <= s_size) 
            {
                string word = s.substr(right, word_size);
                right += word_size;
                if (m.find(word) == m.end()) 
                {
                    count = 0;
                    left = right;
                    temp.clear();
                } 
                else 
                {
                    ++temp[word];
                    ++count;
                    while (temp[word] > m[word]) 
                    {
                        string test = s.substr(left, word_size);
                        --count;
                        --temp[test];
                        left += word_size;
                    }
                    if (count == words_size) result.push_back(left);
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
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> result = new ArrayList<>();
        if (s.length() == 0 || words.length == 0) return result;
        HashMap<String, Integer> map = new HashMap<>();
        int word_size = words[0].length(), words_size = words.length, s_size = s.length();
        for (String word : words) map.put(word, map.getOrDefault(word, 0) + 1);
        for (int i = 0; i < word_size; i++) {
            int left = i, right = i, count = 0;
            HashMap<String, Integer> temp = new HashMap<>();
            while (right + word_size <= s_size) {
                String word = s.substring(right, right + word_size);
                right += word_size;
                if (!map.containsKey(word)) {
                    count = 0;
                    left = right;
                    temp.clear();
                } else {
                    temp.put(word, temp.getOrDefault(word, 0) + 1);
                    count++;
                    while (temp.getOrDefault(word, 0) > map.getOrDefault(word, 0)) {
                        String test = s.substring(left, left + word_size);
                        count--;
                        temp.put(test, temp.getOrDefault(test, 0) - 1);
                        left += word_size;
                    }
                    if (count == words_size) result.add(left);
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        if not words or len(s) < len(words[0]) * len(words):
            return []
        result, wide, num, n, total = list(), len(words[0]), len(words), len(s), len(words[0]) * len(words)
        words.sort()
        for i in range(n + 1 - total):
            l = [s[j : j + wide] for j in range(i, i + total, wide)]
            l.sort()
            if l == words:
                result.append(i)
        return result
```
