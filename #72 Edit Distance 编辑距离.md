# 72 Edit Distance 编辑距离

__Description__:
Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.

You have the following 3 operations permitted on a word:

Insert a character
Delete a character
Replace a character

__Example:__

Example 1:

Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation:
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

Example 2:

Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation:
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

__题目描述__:
给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符

__示例 :__

示例 1：

输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')

示例 2：

输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')

__思路__:

动态规划

1. dp[i][j]表示 word1[:i]和 word2[:j]的编辑距离
2. 如果 word1[i] = word2[j], dp[i + 1][j + 1] = dp[i][j], 不需要增加编辑距离
3. 否则需要增加编辑距离, 即需要修改字符, dp[i + 1][j + 1] = min(dp[i][j], dp[i + 1][j], dp[i][j + 1]) + 1, dp[i][j]表示修改, dp[i + 1][j]表示删除, dp[i][j + 1]表示插入
时间复杂度O(mn), 空间复杂度O(mn)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minDistance(string word1, string word2) 
    {
        int dp[word1.size() + 1][word2.size() + 1];
        for (int i = 0; i <= word1.size(); i++) dp[i][0] = i;
        for (int i = 0; i <= word2.size(); i++) dp[0][i] = i;
        for (int i = 1; i <= word1.size(); i++) 
        {
            for (int j = 1; j <= word2.size(); j++) 
            {
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]}) + 1;
            }
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minDistance(String word1, String word2) {
        int dp[][] = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i <= word1.length(); i++) dp[i][0] = i;
        for (int i = 0; i <= word2.length(); i++) dp[0][i] = i;
        for (int i = 1; i <= word1.length(); i++) {
            for (int j = 1; j <= word2.length(); j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
            }
        }
        return dp[word1.length()][word2.length()];
    }
}
```

__Python__:

```Python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[i for i in range(len(word2) + 1)] for _ in range(len(word1) + 1)]
        for i in range(1, len(word1) + 1):
            dp[i][0] = i
        for i in range(1, len(word1) + 1):
            for j in range(1, len(word2) + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i - 1][j - 1], dp[i][j - 1], dp[i - 1][j]) + 1
        return dp[-1][-1]
```
