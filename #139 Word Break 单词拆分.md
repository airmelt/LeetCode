__Description__:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

__Note:__

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.

__Example:__
Example 1:

Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".

Example 2:

Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.

Example 3:

Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false

__题目描述__:
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

__说明：__

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

__示例 :__
示例 1：

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。

示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。

示例 3：

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false

__思路__:
动态规划
dp[i]表示 s[:i]可以被字典分割
转移方程 dp[i + 1] = check(s[:j] and dp[j]), j = 1, 2, ..., i
初始状态 dp[0] = true, 表示空字符串可以被分割
检查字符串需要使用 set存储, 可以将查找时间降低到 O(1)
可以用字典树和剪枝(只要j - i > wordDict中字符串的最长值, 就终止搜索)加快速度
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    bool wordBreak(string s, vector<string>& wordDict) 
    {
        unordered_set<string> d(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size() + 1, false);
        dp[0] = true;
        for (int i = 0; i < s.size(); i++) for (int j = i + 1; j <= s.size(); j++) if (dp[i] and d.find(s.substr(i, j - i)) != d.end()) dp[j] = true;
        return dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> set = new HashSet<>(wordDict);
        boolean dp[] = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 0; i < s.length(); i++) for (int j = i + 1; j <= s.length(); j++) if (dp[i] && set.contains(s.substring(i, j))) dp[j] = true;
        return dp[s.length()];
    }
}
```

__Python__:
```Python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        d, dp = set(wordDict), [True] + [False] * len(s)
        for i in range(len(s)):
            for j in range(i + 1, len(s) + 1):
                if dp[i] and s[i:j] in d:
                    dp[j] = True
        return dp[-1]
```