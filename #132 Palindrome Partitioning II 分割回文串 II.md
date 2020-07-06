__Description__:
Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

__Example:__

Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

__题目描述__:
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回符合要求的最少分割次数。

__示例 :__

输入: "aab"
输出: 1
解释: 进行一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。

__思路__:
1. 参考[LeetCode #131 Palindrome Partitioning 分割回文串](https://www.jianshu.com/p/1715b6ab5741), 在最后的结果中找到最短的列表, 其长度减 1即为最小的分割方式
时间复杂度太高
时间复杂度O(2 ^ n), 空间复杂度O(n ^ 2)
2. 注意到字符串中存在大量重复子问题, 考虑用动态规划
dp数组表示 s[:i]分割成回文字符串需要的分割次数
初始化 dp数组, 假定所有字符均需要分割, 那么需要分割 s的长度减 1次, 所以初始化 dp数组为 i - 1, i对应 s的下标
遍历字符串, 以每一个下标为中心向两边搜索, 直到搜索出的字符串不是回文字符串
注意这里回文字符串长度可以是奇数也可以是偶数
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    int minCut(string s) 
    {
        int n = s.size(), edge = 0;
        vector<int> dp(n + 1);
        for (int i = 0; i < n + 1; i++) dp[i] = i - 1;
        for (int i = 0; i < n; i++)
        {
            edge = min(n - i, i + 1);
            for (int j = 0; j < edge; j++)
            {
                if (s[i + j] != s[i - j]) break;
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j] + 1);
            }
            edge = min(n - i, i + 2);
            for (int j = 1; j < edge; j++)
            {
                if (s[i + j] != s[i - j + 1]) break;
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j + 1] + 1);
            }
        }
        return dp.back();
    }
};
```

__Java__:
```Java
class Solution {
    public int minCut(String s) {
        if (s == null || s.length() <= 1) return 0;
        int dp[] = new int[s.length() + 1], n = s.length(), edge = 0;
        for (int i = 0; i < n + 1; i++) dp[i] = i - 1;
        for (int i = 0; i < n; i++) {
            edge = Math.min(n - i, i + 1);
            for (int j = 0; j < edge; j++) {
                if (s.charAt(i + j) != s.charAt(i - j)) break;
                dp[i + j + 1] = Math.min(dp[i + j + 1], dp[i - j] + 1);
            }
            edge = Math.min(n - i, i + 2);
            for (int j = 1; j < edge; j++) {
                if (s.charAt(i + j) != s.charAt(i - j + 1)) break;
                dp[i + j + 1] = Math.min(dp[i + j + 1], dp[i - j + 1] + 1);
            }
        }
        return dp[n];
    }
}
```

__Python__:
```Python
class Solution:
    def minCut(self, s: str) -> int:
        if not s or s == s[::-1]:
            return 0
        n = len(s)
        for i in range(1, n + 1):
            if s[:i] == s[i - n - 1:-n - 1:-1] and s[i:] == s[-1:-i - 1:-1]:
                return 1
        dp = list(range(-1, n))
        for i in range(n):
            for j in range(min(n - i, i + 1)):
                if s[i + j] != s[i - j]:
                    breaj
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j] + 1)
            for j in range(1, min(n - i, i + 2)):    
                if s[i + j] != s[i - j + 1]:
                    breaj
                dp[i + j + 1] = min(dp[i + j + 1], dp[i - j + 1] + 1)
        return dp[-1]
```