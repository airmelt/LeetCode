# 629 K Inverse Pairs Array K个逆序对数组

__Description__:

For an integer array nums, an inverse pair is a pair of integers [i, j] where 0 <= i < j < nums.length and nums[i] > nums[j].

Given two integers n and k, return the number of different arrays consist of numbers from 1 to n such that there are exactly k inverse pairs. Since the answer can be huge, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: n = 3, k = 0
Output: 1
Explanation: Only the array [1,2,3] which consists of numbers from 1 to 3 has exactly 0 inverse pairs.

Example 2:

Input: n = 3, k = 1
Output: 2
Explanation: The array [1,3,2] and [2,1,3] have exactly 1 inverse pair.

__Constraints:__

1 <= n <= 1000
0 <= k <= 1000

__题目描述__:
给出两个整数 n 和 k，找出所有包含从 1 到 n 的数字，且恰好拥有 k 个逆序对的不同的数组的个数。

逆序对的定义如下：对于数组的第i个和第 j个元素，如果满i < j且 a[i] > a[j]，则其为一个逆序对；否则不是。

由于答案可能很大，只需要返回 答案 mod 10^9 + 7 的值。

__示例 :__

示例 1:

输入: n = 3, k = 0
输出: 1
解释:
只有数组 [1,2,3] 包含了从1到3的整数并且正好拥有 0 个逆序对。

示例 2:

输入: n = 3, k = 1
输出: 2
解释:
数组 [1,3,2] 和 [2,1,3] 都有 1 个逆序对。

__说明:__

 n 的范围是 [1, 1000] 并且 k 的范围是 [0, 1000]。

__思路__:

动态规划
dp[i][j] 表示对于 1-i 中逆序对有 j 个
dp[i][0] = 1, 表示顺序排列有 1 种情况
dp[i][j] = 0, 其中 j < 0
dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1] + ... + dp[i - 1][j - i + 1], 对于 1-(i - 1) 的任意排列, 将 i 插入任何一个位置都可以得到一个新的排列, 插到最后就是 dp[i - 1][j], 其余位置以此类推, 但是这个递推式的时间复杂度为 O(n ^ 2k), 需要进一步压缩
dp[i][j - 1] = dp[i - 1][j - 1] + dp[i - 1][j - 2] + ... + dp[i - 1][j - i], 与上面的式子比较可得 dp[i][j] = dp[i][j - 1] + dp[i - 1][j] - dp[i - 1][j - 1]
这样就可以将时间复杂度压缩至 O(nk)
注意到 dp 只与前面一行有关, 可以用前缀和将空间复杂度优化到 O(k)
时间复杂度 O(nk), 空间复杂度 O(k)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int kInversePairs(int n, int k) 
    {
        vector<int> dp(k + 1, 0);
        int mod = 1000000007;
        dp.front() = 1;
        for (int i = 2; i <= n; i++)
        {
            vector<int> cur(dp);
            for (int j = 1; j <= k; j++)
            {
                dp[j] = (dp[j - 1] + cur[j]) % mod;
                if (j >= i) dp[j] = (dp[j] - cur[j - i] + mod) % mod;
            }
        }
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int kInversePairs(int n, int k) {
        int dp[][] = new int[n + 1][k + 1], mod = 1000000007;
        for (int i = 0; i < n + 1; i++) dp[i][0] = 1;
        for (int i = 1; i < n + 1; i++) for (int j = 1; j < k + 1; j++) dp[i][j] = ((dp[i - 1][j] + dp[i][j - 1] - (j - i > -1 ? dp[i - 1][j - i] : 0)) % mod + mod) % mod;
        return dp[n][k];
    }
}
```

__Python__:

```Python
class Solution:
    def kInversePairs(self, n: int, k: int) -> int:
        dp = [[1] + [0] * k for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                dp[i][j] = dp[i][j - 1] + dp[i - 1][j] - (dp[i - 1][j - i] if j - i > -1 else 0)
        return dp[n][k] % (10 ** 9 + 7)
```
