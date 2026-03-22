# 903 Valid Permutations for DI Sequence DI 序列的有效排列

__Description__:
You are given a string s of length n where s[i] is either:

'D' means decreasing, or
'I' means increasing.
A permutation perm of n + 1 integers of all the integers in the range [0, n] is called a valid permutation if for all valid i:

If s[i] == 'D', then perm[i] > perm[i + 1], and
If s[i] == 'I', then perm[i] < perm[i + 1].
Return the number of valid permutations perm. Since the answer may be large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: s = "DID"
Output: 5
Explanation: The 5 valid permutations of (0, 1, 2, 3) are:
(1, 0, 3, 2)
(2, 0, 3, 1)
(2, 1, 3, 0)
(3, 0, 2, 1)
(3, 1, 2, 0)

Example 2:

Input: s = "D"
Output: 1

__Constraints:__

n == s.length
1 <= n <= 200
s[i] is either 'I' or 'D'.

__题目描述__:
我们给出 S，一个源于 {'D', 'I'} 的长度为 n 的字符串 。（这些字母代表 “减少” 和 “增加”。）
有效排列 是对整数 {0, 1, ..., n} 的一个排列 P[0], P[1], ..., P[n]，使得对所有的 i：

如果 S[i] == 'D'，那么 P[i] > P[i+1]，以及；
如果 S[i] == 'I'，那么 P[i] < P[i+1]。
有多少个有效排列？因为答案可能很大，所以请返回你的答案模 10^9 + 7.

__示例 :__

输入："DID"
输出：5
解释：
(0, 1, 2, 3) 的五个有效排列是：
(1, 0, 3, 2)
(2, 0, 3, 1)
(2, 1, 3, 0)
(3, 0, 2, 1)
(3, 1, 2, 0)

__提示:__

1 <= S.length <= 200
S 仅由集合 {'D', 'I'} 中的字符组成。

__思路__:

dp
设 dp[i][j] 表示使用前 i 个数, 下一个数为 j(j = p[i]) 时的组合数
比如 'DID', 前两个数对应 'DI', 可以组成 102 和 201 两种
第三个数可以为 0, 1, 2, 3, 可以得到 1032, 2031, 2130, 3021, 3120 五种
这里下一个数只取决与前一个数, 比如 234 已经使用, 这时 s[i] == 'D', 那么下一个数只能使用 0 和 1
先得到一个大概的转移方程
if s[i - 1] == 'D', dp[i][j] = dp[i - 1][j + 1] + dp[i - 1][j + 2] + ... + dp[i - 1][i - 1], 由所有大于 j 的数字的和组成
if s[i - 1] == 'I', dp[i][j] = dp[i - 1][0] + dp[i - 1][1] + ... + dp[i - 1][j - 1], 由所有小于 j 的数字的和组成
那么还剩下 dp[i - 1][j] 没有处理
再来看 102, 后面可以加上 0, 1, 2, 3
首先 3 不能加在末尾, 因为加上 3 一定需要 'I'
如果加上 2, 那么需要把之前的 2 改成 3, 102 -> 1032, 不改变相对顺序
如果加上 1, 可以将之前的大于等于 1 的都加上 1, 102 -> 2031
如果加上 0, 可以将之前的大于等于 0 的都加上 1, 102 -> 2130
同理, 如果这时的 s[i - 1] == 'I', 可以用类似的方法进行分析
那么能够得到 dp[i - 1][j] 应该加到 'D' 分类中去
这时的转移方程为
if s[i - 1] == 'D', dp[i][j] = dp[i - 1][j] + dp[i - 1][j + 1] + ... + dp[i - 1][i - 1], 由所有大于等于 j 的数字的和组成
if s[i - 1] == 'I', dp[i][j] = dp[i - 1][0] + dp[i - 1][1] + ... + dp[i - 1][j - 1], 由所有小于 j 的数字的和组成
由前缀和的思想, 可以简化为
if s[i - 1] == 'D', dp[i][j + 1] = dp[i][j] + dp[i - 1][j]
if s[i - 1] == 'I', dp[i][j] = dp[i][j - 1] + dp[i - 1][j - 1]
可以看到 dp[i] 只取决于 dp[i - 1], 再用滚动数组可以将空间优化为 O(n)
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numPermsDISequence(string s) 
    {
        int n = s.size(), mod = 1000000007;
        vector<int> dp(n + 1);
        dp.front() = 1;
        for (int i = 1; i <= n; i++) 
        {
            vector<int> cur(n + 1);
            if (s[i - 1] == 'D') for (int j = i - 1; j > -1; j--) cur[j] = (cur[j + 1] + dp[j]) % mod;
            else for (int j = 0; j < i; j++) cur[j + 1] = (cur[j] + dp[j]) % mod;
            dp = cur;
        }
        for (int i = 0; i < n; i++) dp[i + 1] = (dp[i + 1] + dp[i]) % mod;
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numPermsDISequence(String s) {
        int n = s.length(), dp[] = new int[n + 1], mod = 1_000_000_007;
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            int[] cur = new int[n + 1];
            if (s.charAt(i - 1) == 'D') for (int j = i - 1; j > -1; j--) cur[j] = (cur[j + 1] + dp[j]) % mod;
            else for (int j = 0; j < i; j++) cur[j + 1] = (cur[j] + dp[j]) % mod;
            dp = cur;
        }
        for (int i = 0; i < n; i++) dp[i + 1] = (dp[i + 1] + dp[i]) % mod;
        return dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def numPermsDISequence(self, s: str) -> int:
        dp, mod = [1] + [0] * (n := len(s)), 10 ** 9 + 7
        for i in range(1, n + 1):
            cur = [0] * (n + 1)
            if s[i - 1] == 'D':
                for j in range(i - 1, -1, -1):
                    cur[j] = cur[j + 1] + dp[j]
            else:
                for j in range(i):
                    cur[j + 1] = cur[j] + dp[j]
            dp = cur
        return sum(dp) % mod
```
