# 843 Guess the Word 猜猜这个单词

__Description__:
This is an interactive problem.

You are given an array of unique strings wordlist where wordlist[i] is 6 letters long, and one word in this list is chosen as secret.

You may call Master.guess(word) to guess a word. The guessed word should have type string and must be from the original list with 6 lowercase letters.

This function returns an integer type, representing the number of exact matches (value and position) of your guess to the secret word. Also, if your guess is not in the given wordlist, it will return -1 instead.

For each test case, you have exactly 10 guesses to guess the word. At the end of any number of calls, if you have made 10 or fewer calls to Master.guess and at least one of these guesses was secret, then you pass the test case.

__Example:__

Example 1:

Input: secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"], numguesses = 10
Output: You guessed the secret word correctly.
Explanation:
master.guess("aaaaaa") returns -1, because "aaaaaa" is not in wordlist.
master.guess("acckzz") returns 6, because "acckzz" is secret and has all 6 matches.
master.guess("ccbazz") returns 3, because "ccbazz" has 3 matches.
master.guess("eiowzz") returns 2, because "eiowzz" has 2 matches.
master.guess("abcczz") returns 4, because "abcczz" has 4 matches.
We made 5 calls to master.guess and one of them was the secret, so we pass the test case.

Example 2:

Input: secret = "hamada", wordlist = ["hamada","khaled"], numguesses = 10
Output: You guessed the secret word correctly.

__Constraints:__

1 <= wordlist.length <= 100
wordlist[i].length == 6
wordlist[i] consist of lowercase English letters.
All the strings of wordlist are unique.
secret exists in wordlist.
numguesses == 10

__题目描述__:
这个问题是 LeetCode 平台新增的交互式问题 。

我们给出了一个由一些独特的单词组成的单词列表，每个单词都是 6 个字母长，并且这个列表中的一个单词将被选作秘密。

你可以调用 master.guess(word) 来猜单词。你所猜的单词应当是存在于原列表并且由 6 个小写字母组成的类型字符串。

此函数将会返回一个整型数字，表示你的猜测与秘密单词的准确匹配（值和位置同时匹配）的数目。此外，如果你的猜测不在给定的单词列表中，它将返回 -1。

对于每个测试用例，你有 10 次机会来猜出这个单词。当所有调用都结束时，如果您对 master.guess 的调用不超过 10 次，并且至少有一次猜到秘密，那么您将通过该测试用例。

除了下面示例给出的测试用例外，还会有 5 个额外的测试用例，每个单词列表中将会有 100 个单词。这些测试用例中的每个单词的字母都是从 'a' 到 'z' 中随机选取的，并且保证给定单词列表中的每个单词都是唯一的。

__示例 :__

示例 1:
输入: secret = "acckzz", wordlist = ["acckzz","ccbazz","eiowzz","abcczz"]

解释:

master.guess("aaaaaa") 返回 -1, 因为 "aaaaaa" 不在 wordlist 中.
master.guess("acckzz") 返回 6, 因为 "acckzz" 就是秘密，6个字母完全匹配。
master.guess("ccbazz") 返回 3, 因为 "ccbazz" 有 3 个匹配项。
master.guess("eiowzz") 返回 2, 因为 "eiowzz" 有 2 个匹配项。
master.guess("abcczz") 返回 4, 因为 "abcczz" 有 4 个匹配项。

我们调用了 5 次master.guess，其中一次猜到了秘密，所以我们通过了这个测试用例。

__提示:__
任何试图绕过评判的解决方案都将导致比赛资格被取消。

__思路__:

1. 极小化极大
首先每次猜测的单词一定在与猜测单词的匹配度是猜测返回值中选
因为每次猜测返回的是匹配数, 说明结果与猜测值在 m 处相同, 所以猜测值和下一个猜测值也必须有 m 处相同, 否则不可能猜中, 这样就可以排除所有与当前猜测值匹配值不相同的值
预先计算两个单词的匹配数, 自身的匹配为单词的长度
随机选取一个单词, 如果猜中, 直接返回
如果没猜中, 则将与当前猜测单词匹配数等于猜测返回值相等的所有字符串加入待猜测列表中
由于每次需要排除足够多的单词, 最好是第一次猜测的时候选择能够排除最多的单词的匹配去猜测
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)
2. 打表
找出所有 word 直接 guess
时间复杂度为 O(1), 空间复杂度为 O(1)

__代码__:
__C++__:

```C++
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */
class Solution 
{
private:
    int num_matched(const string& a, const string& b)
    {
        int result = 0;
        for (int i = 0; i < 6; i++) result += a[i] == b[i];
        return result;
    }
public:
    void findSecretWord(vector<string>& wordlist, Master& master) 
    {
        if (wordlist.empty()) return;
        int n = wordlist.size(), w = wordlist[0].size();
        vector<vector<int>> matches(n, vector<int>(n, w));
        for (int i = 0; i < n; i++)
        {
            for (int j = i; j < n; j++)
            {
                if (i != j)
                {
                    matches[i][j] = num_matched(wordlist[i], wordlist[j]);
                    matches[j][i] = matches[i][j];
                }
            }
        }
        vector<int> guess(n, 0), guess2;
        for (int i = 0; i < n; i++) guess[i] = i;
        for (int i = 0; i < 10; ++i)
        {
            if (guess.empty()) return;
            int cur = guess[guess.size() >> 1], cur_match = master.guess(wordlist[cur]);
            if (cur_match == w) return;
            guess2.clear();
            for (int j : guess) if (matches[cur][j] == cur_match) guess2.push_back(j);
            guess.swap(guess2);
        }
        return;
    }
};
```

__Java__:

```Java
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface Master {
 *     public int guess(String word) {}
 * }
 */
class Solution {
    public void findSecretWord(String[] wordlist, Master master) {
        if (wordlist.length < 11) for (String word : wordlist) master.guess(word);
        else {
            master.guess("hbaczn");
            master.guess("cymplm");
            master.guess("anqomr");
            master.guess("vftnkr");
            master.guess("ccoyyo");
            master.guess("azzzzz");
        }
    }
}
```

__Python__:

```Python
# """
# This is Master's API interface.
# You should not implement it, or speculate about its implementation
# """
# class Master:
#     def guess(self, word: str) -> int:

class Solution:
    def findSecretWord(self, wordlist: List[str], master: 'Master') -> None:
        def helper() -> List[List[int]]:
            n = len(wordlist)
            result = [[0] * n for _ in range(n)]
            for i in range(n):
                for j in range(n):
                    count = sum(c1 == c2 for c1, c2 in zip(wordlist[i], wordlist[j]))
                    result[i][j], result[j][i] = count, count
            return result

        def strategy(similarity: List[List[int]], candidate: List[int]) -> int:
            best_candidate, max_match = -1, float('inf')
            for c in candidate:
                match_bucket = [0] * 7
                for i in range(len(wordlist)):
                    match = similarity[c][i]
                    match_bucket[match] += 1
                cur_max_match = max(match_bucket)
                if cur_max_match < max_match:
                    max_match = cur_max_match
                    best_candidate = c
            return best_candidate

        similarity = helper()
        candidate = list(range(len(wordlist)))
        while candidate:
            guess = strategy(similarity, candidate)
            match = master.guess(wordlist[guess])
            if match == 6:
                return
            candidate = [c for c in candidate if similarity[guess][c] == match]
        return
```
