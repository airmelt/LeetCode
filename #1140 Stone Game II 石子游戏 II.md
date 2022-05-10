# 1140 Stone Game II 石子游戏 II

__Description__:
Alice and Bob continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first.  Initially, M = 1.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

__Example:__

Example 1:

Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger.

Example 2:

Input: piles = [1,2,3,4,5,100]
Output: 104

__Constraints:__

1 <= piles.length <= 100
1 <= piles[i] <= 10^4

__题目描述__:
爱丽丝和鲍勃继续他们的石子游戏。许多堆石子 排成一行，每堆都有正整数颗石子 piles[i]。游戏以谁手中的石子最多来决出胜负。

爱丽丝和鲍勃轮流进行，爱丽丝先开始。最初，M = 1。

在每个玩家的回合中，该玩家可以拿走剩下的 前 X 堆的所有石子，其中 1 <= X <= 2M。然后，令 M = max(M, X)。

游戏一直持续到所有石子都被拿走。

假设爱丽丝和鲍勃都发挥出最佳水平，返回爱丽丝可以得到的最大数量的石头。

__示例 :__

示例 1：

输入：piles = [2,7,9,4,4]
输出：10
解释：如果一开始Alice取了一堆，Bob取了两堆，然后Alice再取两堆。爱丽丝可以得到2 + 4 + 4 = 10堆。如果Alice一开始拿走了两堆，那么Bob可以拿走剩下的三堆。在这种情况下，Alice得到2 + 7 = 9堆。返回10，因为它更大。

示例 2:

输入：piles = [1,2,3,4,5,100]
输出：104

__提示:__

1 <= piles.length <= 100
1 <= piles[i] <= 10^4

__思路__:

动态规划
设 dp[i][j] 表示从 i 到 n - 1 的石头中选择 M = j 时可以取得的最大分数
因为爱丽丝是从 1 开始, 从第 0 堆开始取, 最后爱丽丝的得分应当为 dp[0][1]
从后往前遍历, 因为 j 需要从 1 开始遍历
如果 i + 2 * j >= n, 说明可以全部取完, 此时 dp[i][j] = sum(piles[i:])
否则在 [1, 2 * j] 中选择一个 k 使得下个人取得的分数最小
dp[i][j] = sum(piles[i:]) - rival
其中 rival = dp[i + k][max(k, j)]
时间复杂度为 O(n ^ 3), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int stoneGameII(vector<int>& piles) 
    {
        int n = piles.size(), sum = 0;
        vector<vector<int>> dp(n, vector<int>(n + 1));
        for (int i = n - 1; i > -1; i--) 
        {
            sum += piles[i];
            for (int j = 1; j <= n; j++) 
            {
                if (i + (j << 1) >= n) dp[i][j] = sum;
                else for (int k = 1; k <= (j << 1); k++) dp[i][j] = max(dp[i][j], sum - dp[i + k][max(k, j)]);
            }
        }
        return dp.front()[1];
    }
};
```

__Java__:

```Java
class Solution {
    public int stoneGameII(int[] piles) {
        int n = piles.length, sum = 0, dp[][] = new int[n][n + 1];
        for (int i = n - 1; i > -1; i--) {
            sum += piles[i];
            for (int j = 1; j <= n; j++) {
                if (i + (j << 1) >= n) dp[i][j] = sum;
                else for (int k = 1; k <= (j << 1); k++) dp[i][j] = Math.max(dp[i][j], sum - dp[i + k][Math.max(j, k)]);
            }
        }
        return dp[0][1];
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        n, s = len(piles), 0
        dp = [[0] * (n + 1) for _ in range(n)]
        for i in range(n - 1, -1, -1):
            s += piles[i]
            for j in range(1, n + 1):
                if i + (j << 1) >= n:
                    dp[i][j] = s;
                else:
                    for k in range(1, (j << 1) + 1):
                        dp[i][j] = max(dp[i][j], s - dp[i + k][max(j, k)])
        return dp[0][1]
```
