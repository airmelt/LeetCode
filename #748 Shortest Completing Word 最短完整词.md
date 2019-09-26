__Description__:
Find the minimum length word from a given dictionary words, which has all the letters from the string licensePlate. Such a word is said to complete the given string licensePlate

Here, for letters we ignore case. For example, "P" on the licensePlate still matches "p" on the word.

It is guaranteed an answer exists. If there are multiple answers, return the one that occurs first in the array.

The license plate might have the same letter occurring multiple times. For example, given a licensePlate of "PP", the word "pair" does not complete the licensePlate, but the word "supper" does.

__Example:__
Example 1:

Input: licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
Output: "steps"
Explanation: The smallest length word that contains the letters "S", "P", "S", and "T".
Note that the answer is not "step", because the letter "s" must occur in the word twice.
Also note that we ignored case for the purposes of comparing whether a letter exists in the word.

Example 2:

Input: licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
Output: "pest"
Explanation: There are 3 smallest length words that contains the letters "s".
We return the one that occurred first.

__Note:__

licensePlate will be a string with length in range [1, 7].
licensePlate will contain digits, spaces, or letters (uppercase or lowercase).
words will have a length in the range [10, 1000].
Every words[i] will consist of lowercase letters, and have length in range [1, 15].

__题目描述__:
如果单词列表（words）中的一个单词包含牌照（licensePlate）中所有的字母，那么我们称之为完整词。在所有完整词中，最短的单词我们称之为最短完整词。

单词在匹配牌照中的字母时不区分大小写，比如牌照中的 "P" 依然可以匹配单词中的 "p" 字母。

我们保证一定存在一个最短完整词。当有多个单词都符合最短完整词的匹配条件时取单词列表中最靠前的一个。

牌照中可能包含多个相同的字符，比如说：对于牌照 "PP"，单词 "pair" 无法匹配，但是 "supper" 可以匹配。

__示例 :__
示例 1：

输入：licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
输出："steps"
说明：最短完整词应该包括 "s"、"p"、"s" 以及 "t"。对于 "step" 它只包含一个 "s" 所以它不符合条件。同时在匹配过程中我们忽略牌照中的大小写。
 
示例 2：

输入：licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
输出："pest"
说明：存在 3 个包含字母 "s" 且有着最短长度的完整词，但我们返回最先出现的完整词。
 
__注意：__

牌照（licensePlate）的长度在区域[1, 7]中。
牌照（licensePlate）将会包含数字、空格、或者字母（大写和小写）。
单词列表（words）长度在区间 [10, 1000] 中。
每一个单词 words[i] 都是小写，并且长度在区间 [1, 15] 中。

__思路__:
先用一个长度为 26的数组存储下牌照(licensePlate)的字符的个数, 遍历每一个单词, 找到最短的单词, 满足其字符数大于牌照的字符即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    string shortestCompletingWord(string licensePlate, vector<string>& words) {
        string result = "";
        int count[26] = {0};
        for (char c : licensePlate) if (isalpha(c)) count[tolower(c) - 'a']++;
        for (string word : words) {
            int temp[26] = {0};
            for (char c : word) if (count[c - 'a']) temp[c - 'a']++;
            for (int i = 0; i < 26; i++) {
                if (temp[i] < count[i]) break;
                if (i == 25) if (!result.size() || word.size() < result.size()) result = word;
            }
        }
        return result;
    }
};
```

__Java__:
```
class Solution {
    public String shortestCompletingWord(String licensePlate, String[] words) {
        int count[] = new int[26];
        for (char c : licensePlate.toLowerCase().toCharArray()) if (Character.isLetter(c)) count[c - 'a']++;
        String result = "";
        for (int i = 0; i < words.length; i++) {
            boolean flag = true;
            int wordCount[] = statistic(words[i]);
            for (int j = 0; j < 26; j++) {
                if (wordCount[j] < count[j]) {
                    flag = false;
                    break;
                }
            }
            if (flag && (result.length() == 0 || words[i].length() < result.length())) result = words[i];
        }
        return result;
    }
    
    private int[] statistic(String word) {
        int result[] = new int[26];
        for (char c : word.toCharArray()) result[c - 'a']++;
        return result;
    }
}
```

__Python__:
```
from collections import Counter
class Solution:
    def shortestCompletingWord(self, licensePlate: str, words: List[str]) -> str:
        return min(filter(lambda w : not(Counter(filter(str.isalpha, licensePlate.lower())) - Counter(w.lower())), words), key=len)
```