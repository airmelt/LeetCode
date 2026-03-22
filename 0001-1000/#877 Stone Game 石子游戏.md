# 877 Stone Game 石子游戏

__Description__:
Alice and Bob play a game with piles of stones. There are an even number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].

The objective of the game is to end with the most stones. The total number of stones across all the piles is odd, so there are no ties.

Alice and Bob take turns, with Alice starting first. Each turn, a player takes the entire pile of stones either from the beginning or from the end of the row. This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alice and Bob play optimally, return true if Alice wins the game, or false if Bob wins.

__Example:__

Example 1:

Input: piles = [5,3,4,5]
Output: true
Explanation:
Alice starts first, and can only take the first 5 or the last 5.
Say she takes the first 5, so that the row becomes [3, 4, 5].
If Bob takes 3, then the board is [4, 5], and Alice takes 5 to win with 10 points.
If Bob takes the last 5, then the board is [3, 4], and Alice takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alice, so we return true.

Example 2:

Input: piles = [3,7,2,3]
Output: true

__Constraints:__

2 <= piles.length <= 500
piles.length is even.
1 <= piles[i] <= 500
sum(piles[i]) is odd.

__题目描述__:
亚历克斯和李用几堆石子在做游戏。偶数堆石子排成一行，每堆都有正整数颗石子 piles[i] 。

游戏以谁手中的石子最多来决出胜负。石子的总数是奇数，所以没有平局。

亚历克斯和李轮流进行，亚历克斯先开始。 每回合，玩家从行的开始或结束处取走整堆石头。 这种情况一直持续到没有更多的石子堆为止，此时手中石子最多的玩家获胜。

假设亚历克斯和李都发挥出最佳水平，当亚历克斯赢得比赛时返回 true ，当李赢得比赛时返回 false 。

__示例 :__

输入：[5,3,4,5]
输出：true
解释：
亚历克斯先开始，只能拿前 5 颗或后 5 颗石子 。
假设他取了前 5 颗，这一行就变成了 [3,4,5] 。
如果李拿走前 3 颗，那么剩下的是 [4,5]，亚历克斯拿走后 5 颗赢得 10 分。
如果李拿走后 5 颗，那么剩下的是 [3,4]，亚历克斯拿走后 4 颗赢得 9 分。
这表明，取前 5 颗石子对亚历克斯来说是一个胜利的举动，所以我们返回 true 。

__提示:__

2 <= piles.length <= 500
piles.length 是偶数。
1 <= piles[i] <= 500
sum(piles) 是奇数。

__思路__:

1. 贪心
史上最简单 medium
由于石头堆为偶数, 且石头总数为奇数, 所以总能分出胜负
计算奇数 index 的和及偶数 index 的和, 每次拿较大的 index 对应的石头堆必胜
所以先手可以选择必胜的策略
时间复杂度为 O(1), 空间复杂度为 O(1)
2. 动态规划
记 dp[i][j] 为取区间 [i, j] 能获得的相对分数, 正数表示获胜, 负数表示失败, 0 表示平局
dp[i][j] = max(piles[i] - dp[i + 1][j], piles[j] - dp[i][j - 1])
双方都会尽量找让自己最大的得分
最后比较 dp[0][n - 1] 和 0 的关系决定胜负
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n ^ 2)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool stoneGame(vector<int>& piles) 
    {
        int n = piles.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) dp[i][i] = piles[i];
        for (int i = 1; i < n; i++) for (int j = 0;j < n - i; j++) dp[j][i + j] = max(piles[j] - dp[j + 1][i + j], piles[i + j] - dp[j][i + j - 1]);
        return dp.front().back() > 0;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        return True
```
