# 809 Expressive Words 情感丰富的文字

__Description__:
Sometimes people repeat letters to represent extra feeling. For example:

"hello" -> "heeellooo"
"hi" -> "hiiii"
In these strings like "heeellooo", we have groups of adjacent letters that are all the same: "h", "eee", "ll", "ooo".

You are given a string s and an array of query strings words. A query word is stretchy if it can be made to be equal to s by any number of applications of the following extension operation: choose a group consisting of characters c, and add some number of characters c to the group so that the size of the group is three or more.

For example, starting with "hello", we could do an extension on the group "o" to get "hellooo", but we cannot get "helloo" since the group "oo" has a size less than three. Also, we could do another extension like "ll" -> "lllll" to get "helllllooo". If s = "helllllooo", then the query word "hello" would be stretchy because of these two extension operations: query = "hello" -> "hellooo" -> "helllllooo" = s.
Return the number of query strings that are stretchy.

__Example:__

Example 1:

Input: s = "heeellooo", words = ["hello", "hi", "helo"]
Output: 1
Explanation:
We can extend "e" and "o" in the word "hello" to get "heeellooo".
We can't extend "helo" to get "heeellooo" because the group "ll" is not size 3 or more.

Example 2:

Input: s = "zzzzzyyyyy", words = ["zzyy","zy","zyy"]
Output: 3

__Constraints:__

1 <= s.length, words.length <= 100
1 <= words[i].length <= 100
s and words[i] consist of lowercase letters.

__题目描述__:
有时候人们会用重复写一些字母来表示额外的感受，比如 "hello" -> "heeellooo", "hi" -> "hiii"。我们将相邻字母都相同的一串字符定义为相同字母组，例如："h", "eee", "ll", "ooo"。

对于一个给定的字符串 S ，如果另一个单词能够通过将一些字母组扩张从而使其和 S 相同，我们将这个单词定义为可扩张的（stretchy）。扩张操作定义如下：选择一个字母组（包含字母 c ），然后往其中添加相同的字母 c 使其长度达到 3 或以上。

例如，以 "hello" 为例，我们可以对字母组 "o" 扩张得到 "hellooo"，但是无法以同样的方法得到 "helloo" 因为字母组 "oo" 长度小于 3。此外，我们可以进行另一种扩张 "ll" -> "lllll" 以获得 "helllllooo"。如果 S = "helllllooo"，那么查询词 "hello" 是可扩张的，因为可以对它执行这两种扩张操作使得 query = "hello" -> "hellooo" -> "helllllooo" = S。

输入一组查询单词，输出其中可扩张的单词数量。

__示例 :__

输入：
S = "heeellooo"
words = ["hello", "hi", "helo"]
输出：1
解释：
我们能通过扩张 "hello" 的 "e" 和 "o" 来得到 "heeellooo"。
我们不能通过扩张 "helo" 来得到 "heeellooo" 因为 "ll" 的长度小于 3 。

__提示:__

0 <= len(S) <= 100。
0 <= len(words) <= 100。
0 <= len(words[i]) <= 100。
S 和所有在 words 中的单词都只由小写字母组成。

__思路__:

双指针
遍历每个 word 和 s
用滑动窗口模拟复写字母的过程
时间复杂度为 O(n + ms), 空间复杂度为 O(1), 其中 n 为字符串 s 的长度, m 为 words 数组的长度, s 为 words 数组中字符串的平均长度

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int expressiveWords(string s, vector<string>& words) 
    {
        return count_if(begin(words), end(words), [&](const auto& b) 
        {
            int left_s = 0, right_s = 0, left_b = 0, right_b = 0, n = s.size(), m = b.size();
            while (right_s < n and right_b < m)
            {
                left_s = right_s;
                left_b = right_b;
                if (s[left_s] != b[left_b]) return false;
                while (right_s < n and s[left_s] == s[right_s]) ++right_s;
                while (right_b < m and b[left_b] == b[right_b]) ++right_b;
                if (right_b - left_b < right_s - left_s and right_s - left_s < 3 or right_s - left_s < right_b - left_b) return false;
            }
            return right_s == n and right_b == m;
        });
    }
};
```

__Java__:

```Java
class Solution {
    public int expressiveWords(String s, String[] words) {
        int result = 0;
        for (String word : words) if (helper(s, word)) ++result;
        return result;
    }
    
    private boolean helper(String s, String word) {
        int leftS = 0, rightS = 0, leftWord = 0, rightWord = 0, n = s.length(), m = word.length();
        while (rightS < n && rightWord < m) {
            leftS = rightS;
            leftWord = rightWord;
            if (s.charAt(leftS) != word.charAt(leftWord)) return false;
            while (rightS < n && s.charAt(leftS) == s.charAt(rightS)) ++rightS;
            while (rightWord < m && word.charAt(leftWord) == word.charAt(rightWord)) ++rightWord;
            if (rightWord - leftWord < rightS - leftS && rightS - leftS < 3 || rightS - leftS < rightWord - leftWord) return false;
        }
        return rightS == n && rightWord == m;
    }
}
```

__Python__:

```Python
class Solution:
    def expressiveWords(self, s: str, words: List[str]) -> int:
        result, n = 0, len(s)
        
        def helper(word: string) -> bool:
            left_s, right_s, left_word, right_word, m = 0, 0, 0, 0, len(word)
            while right_s < n and right_word < m:
                left_s, left_word = right_s, right_word
                if s[left_s] != word[left_word]:
                    return False
                while right_s < n and s[left_s] == s[right_s]:
                    right_s += 1
                while right_word < m and word[left_word] == word[right_word]:
                    right_word += 1
                if (word_size := right_word - left_word) < (s_size := right_s - left_s) and s_size < 3 or s_size < word_size:
                    return False
            return right_word == m and right_s == n
                
        for word in words:
            result += helper(word)
        return result
```
