__Description__:
Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words.  It is guaranteed there is at least one word that isn't banned, and that the answer is unique.

Words in the list of banned words are given in lowercase, and free of punctuation.  Words in the paragraph are not case sensitive.  The answer is in lowercase.

__Example:__

Input: 
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
Output: "ball"
Explanation: 
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.
 
__Note:__

1 <= paragraph.length <= 1000.
0 <= banned.length <= 100.
1 <= banned[i].length <= 10.
The answer is unique, and written in lowercase (even if its occurrences in paragraph may have uppercase symbols, and even if it is a proper noun.)
paragraph only consists of letters, spaces, or the punctuation symbols !?',;.
There are no hyphens or hyphenated words.
Words only consist of letters, never apostrophes or other punctuation symbols.

__题目描述__:
给定一个段落 (paragraph) 和一个禁用单词列表 (banned)。返回出现次数最多，同时不在禁用列表中的单词。题目保证至少有一个词不在禁用列表中，而且答案唯一。

禁用列表中的单词用小写字母表示，不含标点符号。段落中的单词不区分大小写。答案都是小写字母。


__示例 :__

输入: 
paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
输出: "ball"
解释: 
"hit" 出现了3次，但它是一个禁用的单词。
"ball" 出现了2次 (同时没有其他单词出现2次)，所以它是段落里出现次数最多的，且不在禁用列表中的单词。 
注意，所有这些单词在段落里不区分大小写，标点符号需要忽略（即使是紧挨着单词也忽略， 比如 "ball,"）， 
"hit"不是最终的答案，虽然它出现次数更多，但它在禁用单词列表中。
 
__说明：__

1 <= 段落长度 <= 1000.
1 <= 禁用单词个数 <= 100.
1 <= 禁用单词长度 <= 10.
答案是唯一的, 且都是小写字母 (即使在 paragraph 里是大写的，即使是一些特定的名词，答案都是小写的。)
paragraph 只包含字母、空格和下列标点符号!?',;.
不存在没有连字符或者带有连字符的单词。
单词里只包含字母，不会出现省略号或者其他标点符号。

__思路__:
用 set存放 banned列表, 查找更快, 用 map计数, 之后遍历即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution {
public:
    string mostCommonWord(string paragraph, vector<string>& banned) {
        unordered_map<string, int> count;
        unordered_set<string> s;
        string result;
        int c = -1;
        for (auto ban : banned) s.insert(ban);
        string temp;
        for (int i = 0; i < paragraph.size(); i++)
        {
            if (paragraph[i] >= 'a' && paragraph[i] <= 'z' || paragraph[i] >= 'A' && paragraph[i] <= 'Z')
            {
                if (paragraph[i] >= 'a') temp += paragraph[i];
                else temp += paragraph[i] - 'A' + 'a';
            }
            else 
            {
                if (!temp.empty() && s.find(temp) == s.end()) count[temp]++;
                temp.clear();
            }
        }
        if (!temp.empty() && s.find(temp) == s.end()) count[temp]++;
        for (auto item : count)
        {
            if (item.second > c) 
            {
                c = item.second;
                result = item.first;
            }
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) {
        Map<String, Integer> map = new HashMap<>();
        String result = "";
        for (String str : banned) map.put(str, 0);
        paragraph = paragraph.toLowerCase().replaceAll("[^a-z]", " ");
        String[] words = paragraph.split("\\s+");
        int count = -1;
        for (String word : words) {
            if (!map.containsKey(word)) map.put(word, 1);
            else if (map.get(word) != 0) map.put(word, map.get(word) + 1);
        }
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            if (entry.getValue() > count) {
                count = entry.getValue();
                result = entry.getKey();
            }
        }
        return result;
    }
}
```

__Python__:
```Python
from collections import Counter
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        return [word[0] for word in list(Counter(re.split(r'\W+', paragraph.lower())).most_common(len(banned) + 1)) if word[0] not in banned][0]
```