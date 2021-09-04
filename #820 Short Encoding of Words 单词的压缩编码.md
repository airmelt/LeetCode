# 820 Short Encoding of Words 单词的压缩编码

__Description__:
A valid encoding of an array of words is any reference string s and array of indices indices such that:

words.length == indices.length
The reference string s ends with the '#' character.
For each index indices[i], the substring of s starting from indices[i] and up to (but not including) the next '#' character is equal to words[i].
Given an array of words, return the length of the shortest reference string s possible of any valid encoding of words.

__Example:__

Example 1:

Input: words = ["time", "me", "bell"]
Output: 10
Explanation: A valid encoding would be s = "time#bell#" and indices = [0, 2, 5].
words[0] = "time", the substring of s starting from indices[0] = 0 to the next '#' is underlined in "time#bell#"
words[1] = "me", the substring of s starting from indices[1] = 2 to the next '#' is underlined in "time#bell#"
words[2] = "bell", the substring of s starting from indices[2] = 5 to the next '#' is underlined in "time#bell#"

Example 2:

Input: words = ["t"]
Output: 2
Explanation: A valid encoding would be s = "t#" and indices = [0].

__Constraints:__

1 <= words.length <= 2000
1 <= words[i].length <= 7
words[i] consists of only lowercase letters.

__题目描述__:
单词数组 words 的 有效编码 由任意助记字符串 s 和下标数组 indices 组成，且满足：

words.length == indices.length
助记字符串 s 以 '#' 字符结尾
对于每个下标 indices[i] ，s 的一个从 indices[i] 开始、到下一个 '#' 字符结束（但不包括 '#'）的 子字符串 恰好与 words[i] 相等
给你一个单词数组 words ，返回成功对 words 进行编码的最小助记字符串 s 的长度 。

__示例 :__

示例 1：

输入：words = ["time", "me", "bell"]
输出：10
解释：一组有效编码为 s = "time#bell#" 和 indices = [0, 2, 5] 。
words[0] = "time" ，s 开始于 indices[0] = 0 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"
words[1] = "me" ，s 开始于 indices[1] = 2 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"
words[2] = "bell" ，s 开始于 indices[2] = 5 到下一个 '#' 结束的子字符串，如加粗部分所示 "time#bell#"

示例 2：

输入：words = ["t"]
输出：2
解释：一组有效编码为 s = "t#" 和 indices = [0] 。

__提示:__

1 <= words.length <= 2000
1 <= words[i].length <= 7
words[i] 仅由小写字母组成

__思路__:

1. 贪心 ➕ 排序 C++
先将 words 中每个词反序
然后将 words 排序, 这样连续的单词中如果有包含关系, 前面的单词一定是后面的子串
每次碰到子串就继续循环, 直到不是子串的情况, 这时给结果加上当前的串的长度
时间复杂度为 O(nlgn + nm), 空间复杂度为 O(mn), n 表示 words 的大小, m 表示 words 中每个单词的平均长度
2. 哈希表 Java
用哈希表保存每一个独特的单词, 即去掉所有它的后缀单词
时间复杂度为 O(nm ^ 2), 空间复杂度为 O(mn), n 表示 words 的大小, m 表示 words 中每个单词的平均长度
3. 字典树 (Trie) Python
保留所有不是其他单词后缀的词
将 words 中的每个词反向插入字典树中
找到所有叶子结点的长度 ➕ 1 即可
时间复杂度为 O(nm), 空间复杂度为 O(mn), n 表示 words 的大小, m 表示 words 中每个单词的平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minimumLengthEncoding(vector<string>& words) 
    {
        for (auto& word : words) reverse(word.begin(), word.end());
        sort(words.begin(), words.end());
        int result = 0;
        for (int i = 0; i < words.size() - 1; i++)
        {
            int size = words[i].size();
            if (words[i] == words[i + 1].substr(0, size)) continue;
            result += size + 1;
        }
        return result + words.back().size() + 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumLengthEncoding(String[] words) {
        Set<String> set = new HashSet<String>(Arrays.asList(words));
        for (String word: words) for (int k = 1; k < word.length(); k++) set.remove(word.substring(k));
        int result = 0;
        for (String word: set) result += word.length() + 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumLengthEncoding(self, words: List[str]) -> int:
        words, Trie = list(set(words)), lambda: collections.defaultdict(Trie)
        trie = Trie()
        nodes = [reduce(dict.__getitem__, word[::-1], trie) for word in words]
        return sum(len(word) + 1 for i, word in enumerate(words) if not len(nodes[i]))
```
