__Description__:
You are given an array of strings words and a string chars.

A string is good if it can be formed by characters from chars (each character can only be used once).

Return the sum of lengths of all good strings in words.

__Example:__
Example 1:

Input: words = ["cat","bt","hat","tree"], chars = "atach"
Output: 6
Explanation: 
The strings that can be formed are "cat" and "hat" so the answer is 3 + 3 = 6.

Example 2:

Input: words = ["hello","world","leetcode"], chars = "welldonehoneyr"
Output: 10
Explanation: 
The strings that can be formed are "hello" and "world" so the answer is 5 + 5 = 10.
 
__Note:__

1 <= words.length <= 1000
1 <= words[i].length, chars.length <= 100
All strings contain lowercase English letters only.

__题目描述__:
给你一份『词汇表』（字符串数组） words 和一张『字母表』（字符串） chars。

假如你可以用 chars 中的『字母』（字符）拼写出 words 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。

注意：每次拼写时，chars 中的每个字母都只能用一次。

返回词汇表 words 中你掌握的所有单词的 长度之和。

__示例 :__
示例 1：

输入：words = ["cat","bt","hat","tree"], chars = "atach"
输出：6
解释： 
可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。

示例 2：

输入：words = ["hello","world","leetcode"], chars = "welldonehoneyr"
输出：10
解释：
可以形成字符串 "hello" 和 "world"，所以答案是 5 + 5 = 10。
 
__提示：__

1 <= words.length <= 1000
1 <= words[i].length, chars.length <= 100
所有字符串中都仅包含小写英文字母

__思路__:
用 hash表记录 chars中每个字符出现的次数, 对 words表中的每一个单词进行比较
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
    public int countCharacters(String[] words, String chars) 
    {
        int count[26] = {0}, result = 0;
        for (auto c : chars) ++count[c - 'a'];
        for (auto word: words)
        {
            int temp[26], flag = 1;
            copy(begin(count), end(count), begin(temp));
            for (auto c : word) 
            {
                if (temp[c - 'a'] == 0) 
                {
                    flag = 0;
                    break;
                }
                --temp[c - 'a'];
            }
            if (flag) result += word.size();
        }
        return result;
    }
}
```

__Java__:
```Java
class Solution {
    public int countCharacters(String[] words, String chars) {
        int count[] = new int[26], result = 0;
        for (char c : chars.toCharArray()) ++count[c - 'a'];
        for (String word: words) {
            int temp[] = count.clone();
            boolean flag = true;
            for (char c : word.toCharArray()) {
                if (temp[c - 'a'] == 0) {
                    flag = false;
                    break;
                }
                --temp[c - 'a'];
            }
            if (flag) result += word.length();
        }
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        c, result = collections.Counter(chars), 0
        for word in words:
            w = collections.Counter(word)
            if all([c[i] >= w[i] for i in w]):
                result += len(word)
        return result
```