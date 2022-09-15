# 1312 Minimum Insertion Steps to Make a String Palindrome 让字符串成为回文串的最少插入次数

__Description:__

Given a string s. In one step you can insert any character at any index of the string.

Return the minimum number of steps to make s palindrome.

A Palindrome String is one that reads the same backward as well as forward.

__Example:__

Example 1:

Input: s = "zzazz"
Output: 0
Explanation: The string "zzazz" is already palindrome we don't need any insertions.

Example 2:

Input: s = "mbadm"
Output: 2
Explanation: String can be "mbdadbm" or "mdbabdm".

Example 3:

Input: s = "leetcode"
Output: 5
Explanation: Inserting 5 characters the string becomes "leetcodocteel".

__Constraints:__

1 <= s.length <= 500
s consists of lowercase English letters.

__题目描述：__

给你一个字符串 s ，每一次操作你都可以在字符串的任意位置插入任意字符。

请你返回让 s 成为回文串的 最少操作次数 。

「回文串」是正读和反读都相同的字符串。

__示例：__

示例 1：

输入：s = "zzazz"
输出：0
解释：字符串 "zzazz" 已经是回文串了，所以不需要做任何插入操作。

示例 2：

输入：s = "mbadm"
输出：2
解释：字符串可变为 "mbdadbm" 或者 "mdbabdm" 。

示例 3：

输入：s = "leetcode"
输出：5
解释：插入 5 个字符后字符串变为 "leetcodocteel" 。

__提示：__

1 <= s.length <= 500
s 中所有字符都是小写字母。

__思路：__

1. 动态规划 - 最长子序列
将字符串 s 反序得到 s'
设 s 与 s‘ 的最长子序列为 m
则 s 除了 m 以外的字符都需要添加到结果中
比如 "leetcode" -> "edocteel", 两个字符串的最长子序列为 "ede"
所以其他字符都需要添加到结果中变为 "leetcodocteel"
设 dp\[i][j] 表示 s[i:j] 与 s\[i:j][::-1] 最长子序列的长度
dp\[i][i] = 1
dp\[i][j] = dp\[i + 1][j - 1] + 2 if s[i] == s[j] else max(dp\[i + 1][j], dp\[i][j - 1])
遍历需要从后往前遍历
注意到 dp\[i] 只和 dp[i + 1] 有关, 可以用滚动数组将空间复杂度压缩到O(n)
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)
2. 动态规划
设 dp\[i][j] 表示使 s[i:j] 成为回文串需要的最少插入次数
dp\[i][j] = dp\[i + 1][j - 1] if s[i] == s[j] else min(dp\[i + 1][j], dp\[i][j - 1]) + 1
如果 s[i] != s[j], 则可以选择 s[i] 或者 s[j] 处插入字符
注意到 dp\[i] 只和 dp[i + 1] 有关, 可以用滚动数组将空间复杂度压缩到O(n)
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minInsertions(string s) 
    {
        int n = s.size();
        vector<int> dp(n);
        for (int i = n - 1; i > -1; i--)
        {
            vector<int> cur(n);
            cur[i] = 1;
            for (int j = i + 1; j < n; j++) cur[j] = s[i] == s[j] ? dp[j - 1] + 2 : max(dp[j], cur[j - 1]);
            dp = cur;
        }
        return n - dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int minInsertions(String s) {
        int n = s.length(), dp[] = new int[n];
        for (int i = n - 1; i > -1; i--) {
            int[] cur = new int[n];
            cur[i] = 1;
            for (int j = i + 1; j < n; j++) cur[j] = s.charAt(i) == s.charAt(j) ? dp[j - 1] + 2 : Math.max(dp[j], cur[j - 1]);
            dp = cur;
        }
        return n - dp[n - 1];
    }
}
```

__Python__:

```Python
class Solution:
    def watchedVideosByFriends(self, watchedVideos: List[List[str]], friends: List[List[int]], id: int, level: int) -> List[str]:
        friend, cur = set(), {id}
        for i in range(level):
            friend |= cur
            cur = set(sum([friends[r] for r in cur], [])) - friend
        result = Counter(sum([watchedVideos[r] for r in cur], []))
        return sorted(result, key=lambda a: (result[a], a))
```
