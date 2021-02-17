# 474 Ones and Zeroes 一和零

__Description__:
You are given an array of binary strings strs and two integers m and n.

Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

A set x is a subset of a set y if all elements of x are also elements of y.

__Example:__

Example 1:

Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

Example 2:

Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.

__Constraints:__

1 <= strs.length <= 600
1 <= strs[i].length <= 100
strs[i] consists only of digits '0' and '1'.
1 <= m, n <= 100

__题目描述__:
给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的大小，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

__示例 :__

示例 1：

输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。

示例 2：

输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。

__提示:__

1 <= strs.length <= 600
1 <= strs[i].length <= 100
strs[i] 仅由 '0' 和 '1' 组成
1 <= m, n <= 100

__思路__:

动态规划
dp[i][j]表示用 i个 0和 j个 1能组成的字符串个数
动态转移方程为 dp[i][j] = max(dp[i - zero][j - one] + 1, dp[i][j])
时间复杂度O(mnk), 空间复杂度O(mn), 其中 k为字符串个数

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findMaxForm(vector<string>& strs, int m, int n) 
    {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (auto &s : strs) 
        {
            int zero = 0, one = 0;
            for (auto &c : s) 
            {
                if (c == '0') ++zero;
                else ++one;
            }
            for (int i = m; i >= zero; i--) for (int j = n; j >= one; j--) dp[i][j] = max(dp[i][j], 1 + dp[i - zero][j - one]);
        }
        return dp.back().back();
    }
};
```

__Java__:

```Java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int dp[][] = new int[m + 1][n + 1];
        for (String s : strs) {
            int zero = 0, one = 0;
            for (char c : s.toCharArray()) {
                if (c == '0') ++zero;
                else ++one;
            }
            for (int i = m; i >= zero; i--) for (int j = n; j >= one; j--) dp[i][j] = Math.max(dp[i][j], 1 + dp[i - zero][j - one]);
        }
        return dp[m][n];
    }
}
```

__Python__:

```Python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for s in strs:
            zero = sum(1 for c in s if c == '0')
            one = len(s) - zero
            for i in range(m, zero - 1, -1):
                for j in range(n, one - 1, -1):
                    dp[i][j] = max(dp[i][j], 1 + dp[i - zero][j - one])
        return dp[-1][-1]
```
