# 140 Word Break II 单词拆分 II

__Description__:
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

__Note:__

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.

__Example:__

Example 1:

Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Example 2:

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.

Example 3:

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]

__题目描述__:
给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

__说明：__

分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

__示例 :__

示例 1：

输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]

示例 2：

输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。

示例 3：

输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]

__思路__:

动态规划 & 回溯法
参考[LeetCode #139 Word Break 单词拆分](https://www.jianshu.com/p/94bfe7c2f243)
先测试是否能够拆分, 不能拆分的直接返回空集
用一个 map记录 s的下标, 及对应的拆分方法
时间复杂度O(n ^ 3), 空间复杂度O(n ^ 3)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) 
    {
        unordered_map<string,vector<string>> m;
        return helper(m, wordDict, s);
    }
private:
    vector<string> helper(unordered_map<string,vector<string>>& m, vector<string>& wordDict, string& s)
    {
        if (m.count(s)) return m[s];
        if (s.empty()) return {""};
        vector<string> result;
        for (auto word : wordDict) 
        {
            if (s.substr(0, word.size()) != word) continue;
            string sub = s.substr(word.size());
            vector<string> temp = helper(m, wordDict, sub);
            for (auto item : temp) result.push_back(word + (item.empty() ? "" : " " + item));
        }
        m[s] = result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        if (wordDict.isEmpty()) return new ArrayList<String>();
        Set<String> set = new HashSet<>(wordDict);
        boolean dp[] = new boolean[s.length() + 1];
        dp[0] = true;
        int maxSize = wordDict.get(0).length();
        for (int i = 1; i < wordDict.size(); i++) maxSize = Math.max(maxSize, wordDict.get(i).length());
        for (int i = 1; i <= s.length(); i++) for (int j = Math.max(i - maxSize, 0); j < i; j++) if (dp[j] && set.contains(s.substring(j, i))) {
            dp[i] = true;
            break;
        }
        if (!dp[s.length()]) return new ArrayList<String>();
        Map<Integer, List <String>> map = new HashMap<>();
        return backtrack(s, 0, maxSize, set, map);
    }
    
    private List<String> backtrack(String s, int start, int maxSize, Set<String> set, Map<Integer, List <String>> map) {
        if (start == s.length()) {
            List<String> result = new ArrayList<>();
            result.add("");
            return result;
        }
        if (map.containsKey(start)) return map.get(start);
        List<String> result = new ArrayList<>();
        map.put(start, result);
        for (int i = 1; i <= maxSize && start + i <= s.length(); i++) {
            String subString = s.substring(start, start + i);
            if (set.contains(subString)) {
                List<String> returnStrings = backtrack(s, start + i, maxSize, set, map);
                for (String returnString: returnStrings) {
                    if (returnString.equals("")) result.add(subString);
                    else result.add(subString + " " + returnString);
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
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        temp = set("".join(wordDict))
        if any([c not in temp for c in s]):
            return []
        dp = [[""], [s[0]] * (s[0] in wordDict)]
        for i in range(1, len(s)):
            dp.append(sum([[f"{k} {s[j:i + 1]}" if k else s[j:i + 1] for k in dp[j]] for j in range(i + 1) if s[j:i + 1] in wordDict and dp[j]], []))
        return dp[-1]
```
