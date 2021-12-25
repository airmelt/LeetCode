# 966 Vowel Spellchecker 元音拼写检查器

__Description__:
Given a wordlist, we want to implement a spellchecker that converts a query word into a correct word.

For a given query word, the spell checker handles two categories of spelling mistakes:

Capitalization: If the query matches a word in the wordlist (case-insensitive), then the query word is returned with the same case as the case in the wordlist.
Example: wordlist = ["yellow"], query = "YellOw": correct = "yellow"
Example: wordlist = ["Yellow"], query = "yellow": correct = "Yellow"
Example: wordlist = ["yellow"], query = "yellow": correct = "yellow"
Vowel Errors: If after replacing the vowels ('a', 'e', 'i', 'o', 'u') of the query word with any vowel individually, it matches a word in the wordlist (case-insensitive), then the query word is returned with the same case as the match in the wordlist.
Example: wordlist = ["YellOw"], query = "yollow": correct = "YellOw"
Example: wordlist = ["YellOw"], query = "yeellow": correct = "" (no match)
Example: wordlist = ["YellOw"], query = "yllw": correct = "" (no match)
In addition, the spell checker operates under the following precedence rules:

When the query exactly matches a word in the wordlist (case-sensitive), you should return the same word back.
When the query matches a word up to capitlization, you should return the first such match in the wordlist.
When the query matches a word up to vowel errors, you should return the first such match in the wordlist.
If the query has no matches in the wordlist, you should return the empty string.
Given some queries, return a list of words answer, where answer[i] is the correct word for query = queries[i].

__Example:__

Example 1:

Input: wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]
Output: ["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]

Example 2:

Input: wordlist = ["yellow"], queries = ["YellOw"]
Output: ["yellow"]

__Constraints:__

1 <= wordlist.length, queries.length <= 5000
1 <= wordlist[i].length, queries[i].length <= 7
wordlist[i] and queries[i] consist only of only English letters.

__题目描述__:
在给定单词列表 wordlist 的情况下，我们希望实现一个拼写检查器，将查询单词转换为正确的单词。

对于给定的查询单词 query，拼写检查器将会处理两类拼写错误：

大小写：如果查询匹配单词列表中的某个单词（不区分大小写），则返回的正确单词与单词列表中的大小写相同。
例如：wordlist = ["yellow"], query = "YellOw": correct = "yellow"
例如：wordlist = ["Yellow"], query = "yellow": correct = "Yellow"
例如：wordlist = ["yellow"], query = "yellow": correct = "yellow"
元音错误：如果在将查询单词中的元音（‘a’、‘e’、‘i’、‘o’、‘u’）分别替换为任何元音后，能与单词列表中的单词匹配（不区分大小写），则返回的正确单词与单词列表中的匹配项大小写相同。
例如：wordlist = ["YellOw"], query = "yollow": correct = "YellOw"
例如：wordlist = ["YellOw"], query = "yeellow": correct = "" （无匹配项）
例如：wordlist = ["YellOw"], query = "yllw": correct = "" （无匹配项）
此外，拼写检查器还按照以下优先级规则操作：

当查询完全匹配单词列表中的某个单词（区分大小写）时，应返回相同的单词。
当查询匹配到大小写问题的单词时，您应该返回单词列表中的第一个这样的匹配项。
当查询匹配到元音错误的单词时，您应该返回单词列表中的第一个这样的匹配项。
如果该查询在单词列表中没有匹配项，则应返回空字符串。
给出一些查询 queries，返回一个单词列表 answer，其中 answer[i] 是由查询 query = queries[i] 得到的正确单词。

__示例 :__

输入：wordlist = ["KiTe","kite","hare","Hare"], queries = ["kite","Kite","KiTe","Hare","HARE","Hear","hear","keti","keet","keto"]
输出：["kite","KiTe","KiTe","Hare","hare","","","KiTe","","KiTe"]

__提示:__

1 <= wordlist.length <= 5000
1 <= queries.length <= 5000
1 <= wordlist[i].length <= 7
1 <= queries[i].length <= 7
wordlist 和 queries 中的所有字符串仅由英文字母组成。

__思路__:

哈希表
用 3 个哈希表分别记录原始 word, 小写 word 和将元音统一为 '*' 通配符的 word
按照先后顺序查找 queries 中的查询是否存在即可
时间复杂度为 O(mn), 空间复杂度为 O(mn), 其中 m 为 wordlist 的长度及 wordlist 中单词的长度, n 为 queries 的长度及 queries 中单词的长度

__代码__:
__C++__:

```C++
class Solution
{
public:
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) 
    {
        unordered_map<string, string> word_map, word_pattern, word_lower;
        for (auto& word : wordlist) 
        {
            word_map[word] = word;
            string cur(word);
            lower_word(cur);
            if (!word_pattern.count(cur)) word_pattern[cur] = word;
            helper(cur);
            if (!word_pattern.count(cur)) word_pattern[cur] = word;
        }
        for (int i = 0, n = queries.size(); i < n; i++) 
        {
            string cur1(queries[i]);
            lower_word(cur1);
            string cur2(cur1);
            helper(cur2);
            queries[i] = word_map.count(queries[i]) ? queries[i] : (word_pattern.count(cur1) ? word_pattern[cur1] : word_pattern.count(cur2) ? word_pattern[cur2] : "");
        }
        return queries;
    }
private:
    void helper(string& word) 
    {
        for (auto& c : word) if (c == 'a' or c == 'e' or c == 'i' or c == 'o' or c == 'u') c = '*';
    }
    
    void lower_word(string& word)
    {
        for (auto& c : word) c |= 32;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] spellchecker(String[] wordlist, String[] queries) {
        Map<String, String> wordMap = new HashMap<>(), wordPatternMap = new HashMap<>();
        for (String word : wordlist) {
            wordMap.put(word, word);
            wordPatternMap.putIfAbsent(word.toLowerCase(), word);
            wordPatternMap.putIfAbsent(wordPattern(word), word);
        }
        for (int i = 0; i < queries.length; i++) queries[i] = wordMap.containsKey(queries[i]) ? queries[i] : (wordPatternMap.containsKey(queries[i].toLowerCase()) ? wordPatternMap.get(queries[i].toLowerCase()) : wordPatternMap.containsKey(wordPattern(queries[i])) ? wordPatternMap.get(wordPattern(queries[i])) : "");
        return queries;
    }
    
    private String wordPattern(String word) {
        char[] letters = word.toLowerCase().toCharArray();
        for (int i = 0; i < letters.length; i++) if (letters[i] == 'a' || letters[i] == 'e' || letters[i] == 'i' || letters[i] == 'o' || letters[i] == 'u') letters[i] = '*';
        return String.valueOf(letters);
    }
}
```

__Python__:

```Python
class Solution:
    def spellchecker(self, wordlist: List[str], queries: List[str]) -> List[str]:
        wordset = set(wordlist)
        wordlower = { word.lower(): word for word in wordlist[::-1] }
        wordvowel = { re.sub(r'[aeiou]', 'a', word.lower()): word for word in wordlist[::-1] }
        return list(map(lambda query: query if query in wordset else wordlower.get(query.lower(), wordvowel.get(re.sub(r'[aeiou]','a',query.lower()),'')), queries))
```
