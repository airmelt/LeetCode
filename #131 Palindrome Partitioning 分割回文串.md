# 131 Palindrome Partitioning 分割回文串

__Description__:
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

__Example:__

Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]

__题目描述__:
给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

__示例 :__

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]

__思路__:

回溯法
拆分一个回文串加入临时列表
满足条件的加入结果, 并终止递归, 条件为字符串边界跳出字符串
将回文串从临时列表中删除
这里, 因为回文串中有大量的子问题, 可以考虑用空间换时间, 用 dp数组记录一下 s的哪些子串是回文串
时间复杂度O(2 ^ n), 空间复杂度O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> partition(string s) 
    {
        int n = s.length();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) 
        {
            dp[i][i] = 1;
            if (i < n - 1 and s[i] == s[i + 1]) dp[i][i + 1] = 1;
        }
        for (int l = 3; l <= n; l++)
        {
            for (int i = 0; i + l - 1 < n; i++)
            {
                int j = i + l - 1;
                if (s[i] == s[j] and dp[i+1][j-1] == 1) dp[i][j] = 1;
            }
        }

        vector<vector<string>> result;
        vector<string> temp;
        helper(s, result, temp, dp, 0);
        return result;
    }
private:
    void helper(string &s, vector<vector<string>> &result, vector<string>& temp, vector<vector<int>> &dp, int start)
    {
        
        if (start == s.length())
        {
           result.push_back(temp);
           return;
        }
        for (int i = start; i < s.length(); i++)
        {
            if (dp[start][i])
            {
                temp.push_back(s.substr(start, i - start + 1));
                helper(s, result, temp, dp, i + 1);
                temp.pop_back();
            }
        }
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();
        helper(s, 0, s.length() - 1, result, new ArrayList<>());
        return result;
    }
    
    private boolean isPalindrome(String s) {
        int i = 0, j = s.length() - 1;
        while (i < j) if (s.charAt(i++) != s.charAt(j--)) return false;
        return true;
    }
    
    private void helper(String s, int start, int end, List<List<String>> result, List<String> temp) {
        if (start > end) {
            result.add(new ArrayList<>(temp));
            return;
        }
        for (int i = 1; i < end - start + 2; i++) {
            String part = s.substring(start, start + i);
            if (isPalindrome(part)) {
                temp.add(part);
                helper(s, start + i, end, result, temp);
                temp.remove(temp.size() - 1);
            }
        }
    }
}
```

__Python__:

```Python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        result = []
        def helper(s: str, temp: List[str]):
            if not s:
                result.append(temp[:])
                return
            for i in range(len(s)):
                if s[:i + 1] == s[i::-1]:
                    helper(s[i + 1:], temp + [s[:i + 1]])
        helper(s, [])
        return result
```
