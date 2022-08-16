# 1255 Maximum Score Words Formed by Letters 得分最高的单词集合

__Description:__

Given a list of words, list of  single letters (might be repeating) and score of every character.

Return the maximum score of any valid set of words formed by using the given letters (words[i] cannot be used two or more times).

It is not necessary to use all characters in letters and each letter can only be used once. Score of letters 'a', 'b', 'c', ... ,'z' is given by score[0], score[1], ... , score[25] respectively.

__Example:__

Example 1:

Input: words = ["dog","cat","dad","good"], letters = ["a","a","c","d","d","d","g","o","o"], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]
Output: 23
Explanation:
Score  a=1, c=9, d=5, g=3, o=2
Given letters, we can form the words "dad" (5+1+5) and "good" (3+2+2+5) with a score of 23.
Words "dad" and "dog" only get a score of 21.

Example 2:

Input: words = ["xxxz","ax","bx","cx"], letters = ["z","a","b","c","x","x","x"], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]
Output: 27
Explanation:
Score  a=4, b=4, c=4, x=5, z=10
Given letters, we can form the words "ax" (4+5), "bx" (4+5) and "cx" (4+5) with a score of 27.
Word "xxxz" only get a score of 25.

Example 3:

Input: words = ["leetcode"], letters = ["l","e","t","c","o","d"], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]
Output: 0
Explanation:
Letter "e" can only be used once.

__Constraints:__

1 <= words.length <= 14
1 <= words[i].length <= 15
1 <= letters.length <= 100
letters[i].length == 1
score.length == 26
0 <= score[i] <= 10
words[i], letters[i] contains only lower case English letters.

__题目描述：__

你将会得到一份单词表 words，一个字母表 letters （可能会有重复字母），以及每个字母对应的得分情况表 score。

请你帮忙计算玩家在单词拼写游戏中所能获得的「最高得分」：能够由 letters 里的字母拼写出的 任意 属于 words 单词子集中，分数最高的单词集合的得分。

单词拼写游戏的规则概述如下：

玩家需要用字母表 letters 里的字母来拼写单词表 words 中的单词。
可以只使用字母表 letters 中的部分字母，但是每个字母最多被使用一次。
单词表 words 中每个单词只能计分（使用）一次。
根据字母得分情况表score，字母 'a', 'b', 'c', ... , 'z' 对应的得分分别为 score[0], score[1], ..., score[25]。
本场游戏的「得分」是指：玩家所拼写出的单词集合里包含的所有字母的得分之和。

__示例：__

示例 1：

输入：words = ["dog","cat","dad","good"], letters = ["a","a","c","d","d","d","g","o","o"], score = [1,0,9,5,0,0,3,0,0,0,0,0,0,0,2,0,0,0,0,0,0,0,0,0,0,0]
输出：23
解释：
字母得分为  a=1, c=9, d=5, g=3, o=2
使用给定的字母表 letters，我们可以拼写单词 "dad" (5+1+5)和 "good" (3+2+2+5)，得分为 23 。
而单词 "dad" 和 "dog" 只能得到 21 分。

示例 2：

输入：words = ["xxxz","ax","bx","cx"], letters = ["z","a","b","c","x","x","x"], score = [4,4,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5,0,10]
输出：27
解释：
字母得分为  a=4, b=4, c=4, x=5, z=10
使用给定的字母表 letters，我们可以组成单词 "ax" (4+5)， "bx" (4+5) 和 "cx" (4+5) ，总得分为 27 。
单词 "xxxz" 的得分仅为 25 。

示例 3：

输入：words = ["leetcode"], letters = ["l","e","t","c","o","d"], score = [0,0,1,1,1,0,0,0,0,0,0,1,0,0,1,0,0,0,0,1,0,0,0,0,0,0]
输出：0
解释：
字母 "e" 在字母表 letters 中只出现了一次，所以无法组成单词表 words 中的单词。

__提示：__

1 <= words.length <= 14
1 <= words[i].length <= 15
1 <= letters.length <= 100
letters[i].length == 1
score.length == 26
0 <= score[i] <= 10
words[i] 和 letters[i] 只包含小写的英文字母。

__思路：__

状态压缩
选择 words 的所有子集
如果所有子母的总数小于 letters 中的字母数, 就更新当前的分数
结果更新为当前分数和结果的较大值
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n), n 为 words 数组的长度

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int maxScoreWords(vector<string>& words, vector<char>& letters, vector<int>& score) 
    {
        int n = words.size(), result = 0;
        vector<int> count(26), state(1 << n);
        for (const auto& c : letters) ++count[c - 'a'];
        for (int mask = 0; mask < (1 << n); mask++)
        {
            vector<int> cur(count);
            for (int i = 0; i < n; i++)
            {
                if (mask & (1 << i))
                {
                    int word_score = 0;
                    bool flag = true;
                    for (const auto& c: words[i])
                    {
                        if (cur[c - 'a'])
                        {
                            --cur[c - 'a'];
                            word_score += score[c - 'a'];
                        } 
                        else 
                        {
                            flag = false;
                            break;
                        }
                    }
                    if (flag) state[mask] += word_score;
                    result = max(state[mask], result);
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
    public int maxScoreWords(String[] words, char[] letters, int[] score) {
        int result = 0, count[] = new int[26], n = words.length, state[] = new int[1 << n];
        for (char c : letters) ++count[c - 'a'];
        for (int mask = 0; mask < (1 << n); mask++) {
            int[] cur = Arrays.copyOf(count, 26);
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    int wordScore = 0;
                    boolean flag = true;
                    for (char c : words[i].toCharArray()) {
                        if (cur[c - 'a'] > 0) {
                            --cur[c - 'a'];
                            wordScore += score[c - 'a'];
                        } else {
                            flag = false;
                            break;
                        }
                    }
                    if (flag) state[mask] += wordScore;
                    result = Math.max(result, state[mask]);
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
    def maxScoreWords(self, words: List[str], letters: List[str], score: List[int]) -> int:
        a, b, result, n = {chr(97 + i): s for i, s in enumerate(score)}, [Counter(w) for w in words], 0, len(words)
        d = [*map(lambda w: sum(a[c] for c in w), words)]
        
        def dfs(start: int, cur: int, pre: dict) -> None:
            if start == n:
                nonlocal result
                result = max(result, cur)
            else:
                dfs(start + 1, cur, pre)
                next = pre.copy()
                next.subtract(b[start])
                if all(next[c] >= 0 for c in next):
                    dfs(start + 1, cur + d[start], next)
        dfs(0, 0, Counter(letters))
        return result
```
