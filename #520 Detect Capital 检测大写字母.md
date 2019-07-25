__Description__:
Given a word, you need to judge whether the usage of capitals in it is right or not.

We define the usage of capitals in a word to be right when one of the following cases holds:

All letters in this word are capitals, like "USA".
All letters in this word are not capitals, like "leetcode".
Only the first letter in this word is capital, like "Google".
Otherwise, we define that this word doesn't use capitals in a right way.
 
__Example:__
Example 1:
Input: "USA"
Output: True

Example 2:
Input: "FlaG"
Output: False
 
__Note:__
The input will be a non-empty word consisting of uppercase and lowercase latin letters.

__题目描述__:
给定一个单词，你需要判断单词的大写使用是否正确。

我们定义，在以下情况时，单词的大写用法是正确的：

全部字母都是大写，比如"USA"。
单词中所有字母都不是大写，比如"leetcode"。
如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。
否则，我们定义这个单词没有正确使用大写字母。

__示例：__
示例 1:
输入: "USA"
输出: True

示例 2:
输入: "FlaG"
输出: False

__注意:__
输入是由大写和小写拉丁字母组成的非空单词。

__思路__:
大写字母出现的次数, 要么等于单词长度, 要么是1
用一个指针记录下大写字母出现的次数, 利用与的短路机制(&&前面的条件为假, 则后面的条件不执行)可以少写一点代码
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    bool detectCapitalUse(string word) {
        int count = 0;
        for (int i = 0; i < word.size(); i++) if (isupper(word[i]) && count++ < i) return false;
        return count == word.size() || count <= 1;
    }
};
```

__Java__:
```
class Solution {
    public boolean detectCapitalUse(String word) {
        int count = 0;
        for (int i = 0; i < word.length(); i++) if (word.charAt(i) >= 'A' && word.charAt(i) <= 'Z' && count++ < i) return false;
        return count == word.length() || count <= 1;
    }
}
```

__Python__:
```
class Solution:
    def detectCapitalUse(self, word: str) -> bool:
        return word.upper() == word or word.capitalize() == word or word.lower() == word
```
