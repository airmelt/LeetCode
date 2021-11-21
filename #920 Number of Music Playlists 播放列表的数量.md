# 920 Number of Music Playlists 播放列表的数量

__Description__:
Your music player contains n different songs. You want to listen to goal songs (not necessarily different) during your trip. To avoid boredom, you will create a playlist so that:

Every song is played at least once.
A song can only be played again only if k other songs have been played.
Given n, goal, and k, return the number of possible playlists that you can create. Since the answer can be very large, return it modulo 109 + 7.

__Example:__

Example 1:

Input: n = 3, goal = 3, k = 1
Output: 6
Explanation: There are 6 possible playlists: [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], and [3, 2, 1].

Example 2:

Input: n = 2, goal = 3, k = 0
Output: 6
Explanation: There are 6 possible playlists: [1, 1, 2], [1, 2, 1], [2, 1, 1], [2, 2, 1], [2, 1, 2], and [1, 2, 2].

Example 3:

Input: n = 2, goal = 3, k = 1
Output: 2
Explanation: There are 2 possible playlists: [1, 2, 1] and [2, 1, 2].

__Constraints:__

0 <= k < n <= goal <= 100

__题目描述__:
你的音乐播放器里有 N 首不同的歌，在旅途中，你的旅伴想要听 L 首歌（不一定不同，即，允许歌曲重复）。请你为她按如下规则创建一个播放列表：

每首歌至少播放一次。
一首歌只有在其他 K 首歌播放完之后才能再次播放。
返回可以满足要求的播放列表的数量。由于答案可能非常大，请返回它模 10^9 + 7 的结果。

__示例 :__

示例 1：

输入：N = 3, L = 3, K = 1
输出：6
解释：有 6 种可能的播放列表。[1, 2, 3]，[1, 3, 2]，[2, 1, 3]，[2, 3, 1]，[3, 1, 2]，[3, 2, 1].

示例 2：

输入：N = 2, L = 3, K = 0
输出：6
解释：有 6 种可能的播放列表。[1, 1, 2]，[1, 2, 1]，[2, 1, 1]，[2, 2, 1]，[2, 1, 2]，[1, 2, 2]

示例 3：

输入：N = 2, L = 3, K = 1
输出：2
解释：有 2 种可能的播放列表。[1, 2, 1]，[2, 1, 2]

__提示:__

0 <= K < N <= L <= 100

__思路__:

动态规划
设 dp[i][j] 表示使用 i 首歌, 其中有 j(0 < j <= min(i, n)) 首不同的歌播放顺序的个数
如果听新歌 dp[i][j] += dp[i - 1][j - 1] \* (n - j + 1), 即 j - 1 首歌不同, 新歌选择有 n - (j - 1) 种
如果听老歌 dp[i][j] += dp[i - 1][j] \* max(0, j - k), 即前面的 k 首歌需要不重复, 最少为 0
注意到 dp[i] 只取决于 dp[i - 1] 可以用滚动数组的方式更新将空间复杂度降低为 O(n)
时间复杂度为 O(nL), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numMusicPlaylists(int n, int goal, int k) 
    {
        vector<long> dp(n + 1, 0);
        dp[1] = n;
        long mod = 1e9 + 7;
        for (int i = 2; i <= goal; i++) for (int j = min(i, n); j > 0; j--) dp[j] = (dp[j - 1] * (n - j + 1) + dp[j] * max(0, j - k) + mod) % mod;
        return dp.back();
    }
};
```

__Java__:

```Java
class Solution {
    public int numMusicPlaylists(int n, int goal, int k) {
        long dp[] = new long[n + 1], mod = 1_000_000_007;
        dp[1] = n;
        for (int i = 2; i <= goal; i++) for (int j = Math.min(i, n); j > 0; j--) dp[j] = (dp[j - 1] * (n - j + 1) + dp[j] * Math.max(j - k, 0) + mod) % mod;
        return (int)dp[n];
    }
}
```

__Python__:

```Python
class Solution:
    def numMusicPlaylists(self, n: int, goal: int, k: int) -> int:
        dp = [0] + [n] + [0] * (n - 1)
        for i in range(2, goal + 1):
            for j in range(min(i, n), 0, -1):
                dp[j] = dp[j - 1] * (n - j + 1) + dp[j] * max(j - k, 0)
        return dp[-1] % (10 ** 9 + 7)
```
