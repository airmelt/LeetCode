__Description__:
In an alien language, surprisingly they also use english lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.

Given a sequence of words written in the alien language, and the order of the alphabet, return true if and only if the given words are sorted lexicographicaly in this alien language.

__Example:__
Example 1:

Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.

Example 2:

Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.

Example 3:

Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
 
__Note:__

1 <= words.length <= 100
1 <= words[i].length <= 20
order.length == 26
All characters in words[i] and order are english lowercase letters.

__题目描述__:
某种外星语也使用英文小写字母，但可能顺序 order 不同。字母表的顺序（order）是一些小写字母的排列。

给定一组用外星语书写的单词 words，以及其字母表的顺序 order，只有当给定的单词在这种外星语中按字典序排列时，返回 true；否则，返回 false。

__示例 :__
示例 1：

输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
输出：true
解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。

示例 2：

输入：words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
输出：false
解释：在该语言的字母表中，'d' 位于 'l' 之后，那么 words[0] > words[1]，因此单词序列不是按字典序排列的。

示例 3：

输入：words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
输出：false
解释：当前三个字符 "app" 匹配时，第二个字符串相对短一些，然后根据词典编纂规则 "apple" > "app"，因为 'l' > '∅'，其中 '∅' 是空白字符，定义为比任何其他字符都小（更多信息）。
 
__提示：__

1 <= words.length <= 100
1 <= words[i].length <= 20
order.length == 26
在 words[i] 和 order 中的所有字符都是英文小写字母。

__思路__:
1. 改写比较器的方法, 按照 order的顺序比较两个字符串的长度和对应位置的大小
2. 按照 order的顺序重新给 words中的单词赋值, 再比较两个字符串的大小
时间复杂度O(mn), 空间复杂度O(1), 其中 n为 words数组的大小, m为数组中的每一个字符串的长度

__代码__:
__C++__:
```C++
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) 
    {
        int index[128] = {0};
        for (int i = 0; i < 26; i++) index[order[i]] = i + 'a';
        for (int i = 0; i < words.size(); i++) for (int j = 0; j < words[i].size(); j++) words[i][j] = index[words[i][j]];
        for (int i = 1; i < words.size(); i++) if (words[i - 1] > words[i]) return false;
        return true;
    }
};
```

__Java__:
```Java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        Comparator<String> comparator = (a, b) -> {
            for (int i = 0; i < Math.min(a.length(), b.length()); i++) if (a.charAt(i) != b.charAt(i)) return order.indexOf(a.charAt(i)) - order.indexOf(b.charAt(i));
            return a.length() - b.length();
        };
        for (int i = 1; i < words.length; i++) if (comparator.compare(words[i - 1], words[i]) > 0) return false;
        return true;
    }
}
```

__Python__:
```Python
class Solution:
    def isAlienSorted(self, words: List[str], order: str) -> bool:
        return words == sorted(words, key=lambda word: [order.index(x) for x in word]) 
```